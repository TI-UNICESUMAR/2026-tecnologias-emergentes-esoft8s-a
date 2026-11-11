# Atividade 11. Contrato de ferramentas MCP

Atividade individual de **0,5 ponto**. Entrega em **11/11/2026, até 23h59**, pelo formulário da sua turma.

Um servidor MCP oferece duas ferramentas de um domínio definido pelo seu RA, com descrição genérica e entrada sem regras. Corrija `description` e `inputSchema`, confira o resultado pelo protocolo e use as ferramentas pelo harness.

## Referências

- Slides das aulas de [16/09](../2026-09-16-MCP/slides.pdf) e [09/11](../2026-11-09-agentes-mcp/slides.pdf).
- [Ferramentas na especificação do MCP](https://modelcontextprotocol.io/specification/2025-11-25/server/tools).
- JSON Schema, [objetos](https://json-schema.org/understanding-json-schema/reference/object) e [números](https://json-schema.org/understanding-json-schema/reference/numeric).
- [mcp-contrato.zip](mcp-contrato.zip), nesta pasta. O README do kit traz os arquivos de cada harness e os problemas comuns.

## Prepare o kit

Use Node.js 22.18 ou posterior e uma das ferramentas: Antigravity, Codex ou Claude Code. O Antigravity é a opção gratuita. O kit não usa `npm install` nem Docker.

Baixe `mcp-contrato.zip` desta pasta pelo botão de download do GitHub, extraia e abra um terminal na pasta `mcp-contrato`.

```bash
node pedido.mjs SEU-RA
```

Substitua `SEU-RA` pelo seu RA completo, com hífen e dígito.

O comando mostra o contrato do seu RA, grava `pedido.md`, `frases.md` e `contrato.json` e registra o servidor `aula` nos três harnesses. Cada RA recebe um limite próprio para a ferramenta de alteração, informado em `pedido.md`. Se mudar a pasta de lugar, rode o comando de novo, porque o registro guarda o caminho. Ele mantém o seu `contrato.json`.

## Corrija o contrato

Reescreva as duas ferramentas de `contrato.json` conforme `pedido.md`, direto no arquivo ou com ajuda do harness. Se usar o harness, feche a sessão ao terminar, pois ela guarda o contrato antigo. Mantenha os nomes. Além das conferências automáticas, o professor lê as duas descrições.

Confira o resultado quantas vezes precisar:

```bash
node conferir.mjs
```

O comando faz `initialize`, `tools/list` e cinco `tools/call`, sem usar modelo, e mostra nove conferências. Continue até todas ficarem com `[ok]` e sem nenhuma linha `[aviso]`. A correção executa outros casos sobre as mesmas regras de `pedido.md`.

## Use as ferramentas pelo harness

Termine o contrato antes deste passo. Só contam as chamadas feitas com a versão final de `contrato.json`.

1. Abra a ferramenta na pasta `mcp-contrato` (`agy`, `codex` ou `claude`). Se a sessão já estava aberta, feche e abra de novo. No Claude Code, escolha `Yes, I trust this folder` e depois `Use this MCP server`, porque o cursor começa na opção de recusar. No Codex, aceite confiar na pasta, se a pergunta aparecer.

   No Antigravity, o comando `/mcp` mostra o servidor `aula` e as duas ferramentas do seu domínio. Se aparecerem ferramentas de outro domínio, o Antigravity está usando um registro antigo do servidor. Feche a sessão, rode `agy mcp remove aula` na pasta do kit e abra de novo.

2. Peça:

   ```text
   Execute os pedidos de frases.md.
   ```

3. Aprove as chamadas às ferramentas e capture um print da tela do harness em que apareça o nome de uma delas, na linha da chamada ou no pedido de aprovação.

Um único pedido cobre as cinco frases. Evite repetir a execução sem necessidade, pois a cota gratuita também atende aos testes da AEP.

## Gere a evidência e entregue

```bash
node evidencia.mjs
```

O comando grava `entrega/evidencia-<RA>.json`. Se aparecer `[pendente]`, faltam as chamadas pelo harness. O resultado das cinco frases aparece com acertos, falsos positivos e falsos negativos. Esse número é informativo e fica fora da nota, pois a resposta do modelo varia entre execuções.

[Formulário de entrega](https://forms.gle/b3T3LSjpcrSTU3cP8)

Informe nome e RA e anexe:

- O JSON gerado, sem editar.
- O print da chamada à ferramenta.

Se instalação, login ou cota impedirem o uso do harness, entregue o JSON mesmo assim e descreva o que ocorreu no campo opcional **Dificuldades ou falhas**.
