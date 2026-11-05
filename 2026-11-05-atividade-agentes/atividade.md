# Atividade 10. Agentes spec e coder

Atividade individual de **0,5 ponto**. Entrega em **05/11/2026, até 23h59**, pelo formulário da sua turma.

Crie dois agentes para o pedido definido pelo seu RA. O **spec** escreve os critérios e as dúvidas. O **coder** implementa a função depois que as dúvidas forem respondidas. A conversa principal coordena os agentes e executa os testes.

## Material de apoio

- [Slides da atividade](slide.pdf), bloco “Criar agentes no harness” e seção de consulta.
- [Kit e exemplos no GitHub](https://github.com/pedrosatin/processo-software-agentes/tree/main/criar-agentes). Consulte primeiro os requisitos e a instalação no README.

## Prepare o kit

Use Node.js 22.18 ou posterior e uma das ferramentas dos slides. Escolha apenas uma. O Antigravity é a opção gratuita indicada no kit; Codex e Claude Code são alternativas para quem já tem acesso.

Para obter o repositório pela primeira vez:

```bash
git clone https://github.com/pedrosatin/processo-software-agentes.git
cd processo-software-agentes/criar-agentes
```

Quem já tem o repositório pode atualizá-lo com `git pull` e entrar na pasta `criar-agentes`. Dentro dela, execute:

```bash
node pedido.mjs SEU-RA
```

Substitua `SEU-RA` pelo seu RA completo, com hífen e dígito. O script informa o pedido, a função e a pasta com as respostas. Todos os passos seguintes acontecem dentro de `criar-agentes`.

## Crie os dois agentes

Use os caminhos e o formato da sua ferramenta mostrados nos slides e nas seções “Onde fica cada agente” e “Declarar um agente em cada harness” do README. Cada agente precisa de nome, descrição e instruções. A descrição deve explicar quando chamar aquele papel.

- O **spec** lê `pedido.md` e `respostas.md` e grava `saida/spec.md` com critérios numerados e dúvidas. Ele deve copiar as mensagens de erro das respostas para os critérios, sem mudar o texto, e pedir esclarecimento quando faltar uma decisão.
- O **coder** lê `pedido.md` e `saida/spec.md` e grava a função no arquivo indicado no pedido, seguindo as regras do kit.

Ao copiar um exemplo, troque o nome da função, a assinatura e o arquivo de saída pelos do seu `pedido.md`. Substitua frases resumidas como “Mesmas instruções do Claude Code” pelo corpo completo das instruções acima.

## Execute e responda às dúvidas

1. Abra a ferramenta nessa pasta (`agy`, `codex` ou `claude`). Se a sessão já estava aberta antes de você criar os agentes, feche e abra de novo. Peça:

   ```text
   Execute o processo de desenvolvimento para pedido.md.
   ```

2. Confira a chamada ao spec e espere a conversa principal apresentar as dúvidas. Depois, copie as respostas da pasta indicada pelo script para `respostas.md` e acrescente sua decisão para qualquer dúvida nova. Essa cópia é feita por você; os agentes não devem consultar `pedidos/`.

   Mesmo se o spec não levantar dúvidas, preencha as respostas e remova `saida/spec.md` para que ele gere uma nova especificação com essas informações. Em ambos os casos, siga para o passo 3.

3. Retome a conversa com:

   ```text
   Preenchi respostas.md. Execute o processo de desenvolvimento para pedido.md.
   ```

   Confira a chamada ao coder e os resultados dos testes. Capture o print enquanto a chamada ao agente estiver visível. Se surgirem novas dúvidas, responda em `respostas.md` e use o mesmo comando de retomada.

## Gere a evidência e entregue

No terminal, execute:

```bash
node evidencia.mjs
```

O comando executa os seis testes, confere os arquivos e grava `entrega/evidencia-<RA>.json`. Se houver falhas, leia a orientação “O que fazer”. Para falhas na função, envie as mensagens à conversa principal e peça uma correção pelo coder. Depois, gere a evidência novamente. Evite repetir `node pedido.mjs` depois de responder às dúvidas, pois esse comando esvazia `respostas.md`.

[Formulário de entrega da Turma A](https://docs.google.com/forms/d/e/1FAIpQLSeFfT-vfPXTnHjl55UufCSllTJwLKETH_l1_zZSpNhFVTZRoA/viewform)

Informe nome e RA e anexe:

- O JSON gerado, sem editar.
- Um print da tela mostrando a chamada ao agente `spec` ou `coder`. No Codex, pode usar `/subagents`. No Antigravity, procure `invoke_subagent`; no Claude Code, `Agent`. O resumo final do modelo, sozinho, não comprova a delegação.

Se ainda houver testes com falha, envie o JSON gerado e descreva o que ocorreu no campo opcional **Dificuldades ou falhas**. A pontuação dos testes é proporcional aos acertos. Se instalação, login ou cota impedirem a execução, avise o professor; não contrate um plano para concluir a atividade.
