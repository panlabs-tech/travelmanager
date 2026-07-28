# MCPs

Quais servidores MCP este repo configura, o que cada um serve e o que ele exige do ambiente.

O documento existe porque este repo **versiona configuração de MCP** ([`.mcp.json`](../../.mcp.json)). Sem ele, quem chega vê três servidores declarados e não sabe qual deles é necessário para quê, nem quais precisam de credencial.

## Os três

| servidor | transporte | para que serve | exige credencial |
| --- | --- | --- | --- |
| `hostinger-vps` | stdio, `npx` | operar a VPS que hospeda o ambiente: reiniciar, inspecionar, ver recurso | `HOSTINGER_API_TOKEN` |
| `coolify` | stdio, `npx` | criar e operar o deploy: aplicação, env, redeploy, log | `COOLIFY_ACCESS_TOKEN` |
| `cloudflare` | http | DNS e borda do domínio | não, autentica no navegador |

Nenhum dos três é necessário para **rodar** a aplicação localmente. Eles servem à operação, e o caminho de subir o repo está em [`local-dev.md`](local-dev.md).

## O arquivo é versionado, e por isso ele não carrega segredo

`.mcp.json` traz **placeholder de variável de ambiente**, nunca o valor:

```json
"COOLIFY_ACCESS_TOKEN": "${COOLIFY_ACCESS_TOKEN}"
```

Isto corrige um padrão que estava vivo aqui: a configuração real ficava **gitignored, com o token literal dentro**, e por isso nunca era de fato compartilhada apesar do nome. Duas consequências, e as duas custavam: quem clonava o repo não tinha como saber quais MCPs existiam, e o segredo dependia de o `.gitignore` nunca ser editado por engano.

`COOLIFY_BASE_URL` traz default (`${COOLIFY_BASE_URL:-https://vps.panlabs.tech}`) porque é endereço público, não credencial: cravá-lo economiza uma variável de quem só quer ler.

## Como preencher

As variáveis vêm do ambiente do agente, não de arquivo dentro do repo. Exporte-as no shell da máquina, ou no arquivo de ambiente do agente:

```bash
export HOSTINGER_API_TOKEN=...
export COOLIFY_ACCESS_TOKEN=...
```

Nenhum dos dois é gerado pela máquina: são credenciais de terceiro, e obtê-las é ato do operador. É um dos quatro casos em que o agente **para e chama**, pela regra que o [`AGENTS.md`](../../AGENTS.md) declara e o [ADR-0007](../adr/0007-autonomia-total-do-agente.md) fundamenta.
