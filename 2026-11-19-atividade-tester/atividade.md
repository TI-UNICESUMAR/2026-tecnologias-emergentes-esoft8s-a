# Atividade 12. Agente tester do contrato MCP

Atividade individual de **0,5 ponto**. Entrega em **19/11/2026, até 23h59**, pelo formulário da sua turma.

Você cria um agente tester para o seu contrato MCP da A11. O comando `node testar.mjs` executa os casos que ele escreve no contrato correto do seu RA e em oito contratos com um defeito cada. Depois você explica, sem o harness, duas fronteiras do contrato. Este enunciado diz o que precisa existir no fim. O caminho sai da aula de 18/11 e do contrato que você escreveu na A11.

## Referências

- Aula de 18/11, agente tester, e slides de [09/11](../2026-11-09-agentes-mcp/slides.pdf).
- O seu `pedido.md` e o seu `contrato.json` da A11.
- JSON Schema, [objetos](https://json-schema.org/understanding-json-schema/reference/object) e [números](https://json-schema.org/understanding-json-schema/reference/numeric).
- [mcp-contrato.zip](mcp-contrato.zip), nesta pasta. É o kit da A11 com `testar.mjs` e `a12.mjs`.

## Prepare o kit

Use Node.js 22.18 ou posterior e uma das ferramentas: Antigravity, Codex ou Claude Code. O Antigravity é a opção gratuita. A atividade foi testada no Antigravity e no Claude Code. O Codex ficou sem teste, então prefira um dos outros dois.

Baixe `mcp-contrato.zip` desta pasta pelo botão de download do GitHub. Se você ainda tem a pasta `mcp-contrato` da A11, extraia o zip novo por cima dela. O zip não traz `contrato.json`, então o seu contrato da A11 continua na pasta. Se não tem a pasta, extraia o zip em uma pasta nova.

Nos dois casos, abra um terminal na pasta `mcp-contrato` e execute:

```bash
node pedido.mjs SEU-RA
```

Substitua `SEU-RA` pelo seu RA completo, com hífen e dígito.

## Crie o agente tester

Crie o agente `tester` na pasta `mcp-contrato`, no formato da ferramenta que você usa.

| Ferramenta | Arquivo |
|---|---|
| Antigravity | `.agents/agents/tester.md` |
| Claude Code | `.claude/agents/tester.md` |
| Codex | `.codex/agents/tester.toml` |

O que o agente lê, quais ferramentas recebe e o que a instrução proíbe são decisões suas. Na aula de 18/11, um tester executou o servidor e escreveu casos errados. A sua instrução precisa impedir isso. A correção confere no arquivo uma descrição que diz quando usar o agente e uma instrução que cita `pedido.md` e `casos.json`.

O agente grava `casos.json`. O arquivo tem o campo `agente`, com o valor `"tester"`, e a lista `casos`. Cada caso tem `nome`, `ferramenta`, `argumentos` e `espera`. `argumentos` é um objeto com os argumentos da chamada, e `espera` vale `"aceita"` ou `"recusa"`. Se faltar algum campo, `node testar.mjs` diz qual.

## Peça os casos pelo harness

Abra a ferramenta na pasta `mcp-contrato`. Se a sessão já estava aberta antes de você criar o agente, feche e abra de novo. Na conversa principal, peça que o subagent `tester` escreva os casos, como você fez com spec e coder na A10.

Aprove a gravação de `casos.json` e capture um print da janela do terminal, com Win+Shift+S no Windows, em que apareça a linha da chamada ao agente, como `Agent(tester ...)` no Antigravity. O resumo final do modelo não comprova a chamada.

## Teste os casos

```bash
node testar.mjs
```

| Marca | Significado |
|---|---|
| `[ok]` | O caso dá o resultado esperado no contrato correto |
| `[caso errado]` | O contrato correto dá outro resultado. O esperado do caso está errado |
| `[fora]` | O inputSchema aceita e o servidor responde outra coisa. O caso não conta |
| `[detectado]` | Algum caso falhou no contrato com esse defeito |
| `[sobreviveu]` | Nenhum caso falhou no contrato com esse defeito. A saída mostra só a ferramenta |

Quando um defeito sobrevive, descubra pelo `pedido.md` qual regra daquela ferramenta nenhum caso quebra. Depois mude a instrução do tester para que ele cubra esse tipo de regra em qualquer contrato, sem escrever o valor do caso na instrução, e peça os casos de novo. O mesmo vale para `[caso errado]` e `[fora]`. A atividade avalia o que o tester escreve, então não edite `casos.json` à mão. Cada pedido gasta cota.

A parte "Seu contrato.json, informativo" executa os mesmos casos no contrato que está na sua pasta. Quem tem o contrato da A11 vê ali o que ele aceita e não deveria. Sem a pasta da A11, o contrato é o inicial, sem regras, e a lista sai longa. Essa parte fica fora da nota.

## Escreva a análise

No fim da saída, `testar.mjs` mostra o defeito da sua análise, como `d5`. Crie `analise.md` na pasta `mcp-contrato` e responda as duas perguntas com as suas palavras, sem pedir ao harness. As perguntas têm o tipo de raciocínio que a prova pede sobre `inputSchema`.

1. Com o limite do seu RA, quais dois valores do parâmetro com máximo separam o contrato correto de um contrato com o máximo errado por uma unidade, para mais e para menos? Dê o esperado de cada valor e explique por que um caso no meio da faixa não detecta nenhum desses dois defeitos.
2. Qual regra do `pedido.md` o contrato com o defeito da sua análise deixa de cumprir? Qual palavra-chave do `inputSchema` muda nesse contrato em relação ao que você escreveu na A11, e qual dos seus casos detecta o defeito? Se nenhum caso detecta, escreva a chamada que detectaria.

Rode `node testar.mjs` de novo depois de salvar `analise.md`. O texto entra na evidência.

## Entregue

`node testar.mjs` grava `entrega/a12-<RA>.json` a cada execução. Envie o arquivo da última execução.

[Formulário de entrega](https://docs.google.com/forms/d/e/1FAIpQLSfgvKv7YeOhf2BGEs4DDjjwNFWw7SEntgl0GTU30x8i1Cir8A/viewform)

Informe nome e RA e anexe:

- O JSON gerado, sem editar.
- O print com a chamada ao agente `tester`.

| Item | Valor | Como é conferido |
|---|---:|---|
| Evidência | 0,05 | RA nas listas das turmas e JSON gerado por `testar.mjs` |
| Casos certos | 0,05 | Casos com o esperado certo no contrato correto, com pelo menos cinco casos válidos, contados de novo na correção |
| Defeitos detectados | 0,20 | Proporcional aos oito contratos com defeito, contados de novo na correção |
| Agente tester | 0,10 | Arquivo do agente na evidência e print da chamada |
| Análise | 0,10 | 0,05 por resposta de `analise.md`, conferida com o limite e o defeito do seu RA |

Se instalação, login ou cota impedirem o uso do harness, entregue o JSON mesmo assim e descreva o que ocorreu no campo opcional **Dificuldades ou falhas**.
