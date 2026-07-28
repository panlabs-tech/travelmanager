# Design

Como um agente consome o sistema visual deste repo ao implementar ou revisar interface.

O documento existe porque este repo **tem interface declarada em manifesto Node** (`next`/`react` em `apps/web/package.json`). Onde há interface, há decisão visual, e decisão visual que só vive num protótipo externo não sobrevive à primeira sessão que não o abriu.

## Onde o design mora

O contrato vivo é [`docs/design/`](../design/README.md), versionado neste repo. Ele descreve **Noturno**, a direção visual do travelmanager: pele quente de aviação analógica, fundo petróleo profundo, cremes e off-whites quentes por cima, accent terracota (`#df6a4d`) carregando marca, CTA, estado ativo e destaque.

A metáfora não é decoração. Cartão de embarque, tripulação, código IATA, mapa de voo e caderno de bordo são o vocabulário visual porque o produto é sobre **translado**: combinar paradas, comparar caminhos e decidir passagens. Um tema genérico de SaaS não carregaria essa carga semântica.

| assunto | arquivo |
| --- | --- |
| tokens, tipografia, voz, movimento, regras de forma | [`design-spec.md`](../design/design-spec.md) |
| o que existe hoje em `apps/web`, tela a tela | [`as-built.md`](../design/as-built.md) |
| o que ainda falta virar produto persistido | [`blueprint.md`](../design/blueprint.md) |
| tokens como dado | [`tokens.json`](../design/tokens.json) |

## A ordem de leitura, e ela importa

1. [`design-spec.md`](../design/design-spec.md), para tokens, tipografia, voz e regras de forma.
2. [`as-built.md`](../design/as-built.md), **antes de tocar qualquer superfície já implementada**. Landing, Login, Onboarding, Painel de bordo, Nova viagem, Painel da viagem e Pesquisa de translado já existem, e mexer nelas sem ler o as-built é reimplementar uma decisão que já foi tomada.
3. [`blueprint.md`](../design/blueprint.md), antes de transformar uma casca ou modelo local em produto persistido.
4. [`../../CONTEXT.md`](../../CONTEXT.md), sempre que a tela cruzar dado de domínio: Viagem, Parada, Trajeto, Rota, Trecho, Pesquisa, Preferida, Comprada.

## Os tokens têm fonte e espelho, e eles não podem divergir

A fonte viva é [`tokens.json`](../design/tokens.json). O espelho executável é `apps/web/app/globals.css`. Editar um sem o outro produz uma interface que não corresponde ao que o documento afirma, e o sintoma aparece uma tela por vez, semanas depois.

## O protótipo é origem, não dependência

O redesenho navegável aceito vive em `.claude/design/redesign-travelmanager`, que é **local e gitignored**. Ele continua sendo a referência de intenção quando houver dúvida visual sobre as telas já implementadas, mas a verdade operacional é o código versionado, descrito no as-built. O repo compila, roda e é revisável sem ele.
