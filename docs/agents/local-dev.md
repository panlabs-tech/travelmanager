# Desenvolvimento local

Como subir este repo na máquina, e qual dependência com estado ele tem.

O documento existe porque este repo **declara composição de serviços locais**: [`docker-compose.yml`](../../docker-compose.yml) na raiz. Onde há dependência local com estado, quem chega precisa saber o que subir antes de rodar teste, e descobrir isso por tentativa custa a primeira sessão inteira.

## A dependência com estado

**Postgres 17**, e ele espelha o componente `travelmanager-db` de produção. A versão não é acidente: divergir dela faria "passou no local" e "passou em produção" significarem coisas diferentes sobre migration e sobre comportamento de tipo.

O compose sobe três serviços:

| serviço | o que é | porta |
| --- | --- | --- |
| `db` | Postgres 17, com healthcheck e volume `pgdata` | 5432 |
| `api` | FastAPI, build de `apps/api/Dockerfile` | 8000 |
| `web` | Next.js, build de `apps/web/Dockerfile` | 3000 |

`api` só sobe com o `db` **saudável** (`condition: service_healthy`), e não apenas iniciado: o Postgres aceita conexão antes de estar pronto para query, e sem essa condição o boot do Alembic falha de forma intermitente.

## Os dois modos de rodar, e quando cada um serve

**Compose inteiro**, para exercitar a integração como ela é implantada:

```bash
docker compose up --build
```

**Só o banco, com api e web nativos**, que é o modo de trabalho do dia a dia, porque dá reload e depuração:

```bash
docker compose up -d db

# API, de dentro de apps/api/
uv run uvicorn travelmanager.main:app --reload    # :8000

# Web, da raiz
pnpm --filter @travelmanager/web dev              # :3000
```

## Testes, e a marca que separa os dois mundos

```bash
# de dentro de apps/api/
uv run pytest -m "not integration"    # não toca banco: é o que a CI roda
uv run pytest -m integration          # exige o Postgres de pé
```

A marca `integration` existe para que a suíte que a CI roda **não** dependa de serviço nenhum. Um teste de use-case usa fakes dos Ports; só o que exercita repositório de verdade é marcado. Rodar a suíte inteira sem o `db` de pé falha nos marcados, e isso é o desenho funcionando, não defeito.

## Variáveis de ambiente

`DATABASE_URL` é a única que o `api` exige para subir, e o compose já a fornece apontando para o serviço `db`. Fora do compose, ela precisa apontar para o Postgres local:

```
postgresql+psycopg://travelmanager:travelmanager@localhost:5432/travelmanager
```

Configuração de MCP é assunto de [`mcps.md`](mcps.md), e não entra aqui: nada dela é necessário para subir a aplicação.
