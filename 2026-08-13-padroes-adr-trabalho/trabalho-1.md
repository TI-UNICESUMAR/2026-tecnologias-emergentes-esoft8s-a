# Trabalho Prático 1: Engenharia de Prompt e Contexto na Prática (com evidências)

Vale 1,5 ponto. Cobre só o que foi dado até agora: anatomia de prompt (system/user), few-shot, chain-of-thought, JSON mode, janela de contexto, economia de tokens e custo por chamada.

Leia este documento inteiro antes de começar. Ele existe justamente pra não sobrar dúvida de "como eu ia saber que precisava disso" na hora da apresentação.

---

## 1. Formato

- Grupos de 3 a 5 pessoas.
- Trabalho e apresentação são em grupo. **A nota é individual**: na apresentação, cada integrante responde uma pergunta específica sobre a parte que ele mesmo construiu (o prompt que escreveu, os dados que coletou). Quem não souber explicar sua própria parte perde nota, mesmo que o grupo entregue tudo certo.

## 2. Escolha do projeto

Duas opções, livre escolha do grupo:

- Um projeto de estudo de qualquer outra disciplina do semestre (ex: um site pra revisar conteúdo, sistema pra solucionar algum problema que conhecem ou vivenciaram). Aqui é a hora de ser criativo: não precisa ser óbvio nem "seguro", foge do feijão com arroz e tenta algo que você mesmo usaria de verdade.
- Uma feature pequena e isolada pensada pra Escola de TI (não precisa integrar com sistema real nenhum, é um protótipo).

Se escolher a Escola de TI: vocês vão implementar só a feature, não o projeto inteiro, de propósito. Uma LLM sem contexto do projeto real (arquitetura, convenções, arquivos vizinhos) tende a inventar decisão errada ou gerar algo desconectado do resto do sistema. Por isso, tentem a mesma feature mais de uma vez, variando a quantidade de contexto fornecido, do mínimo possível até um pacote completo (arquitetura, arquivos relacionados, convenções do projeto), e comparem resultado e consumo de tokens entre as tentativas. Essa comparação alimenta direto o requisito 3 (curadoria de contexto) da próxima seção, não é trabalho extra.

## 3. Pense nas evidências antes de codar

Evidência aqui significa print de tela, não texto contando o que você fez. Se aconteceu, tem print. Sem print, é como se não tivesse acontecido.

Erro mais comum: codar tudo primeiro e só depois tentar reconstruir os dados pra entregar. **Planeje o que vai capturar antes da primeira chamada de IA.** Faça testes antes, veja que dados consegue coletar com um "MVP". Sugestão de decisões antes de começar:

- Qual vai ser o system prompt do projeto? O que justifica essa decisão?
- Qual ou quais técnicas de prompt você vai aplicar?
- Onde você vai salvar cada chamada feita? Qual o padrão de log dessas chamadas, contemplando os requisitos abaixo.
- Qual ferramenta vai usar pra gerar os números de tokens. Sugestão: https://ccusage.com ou o Default dos Harness.

## 4. Requisitos técnicos obrigatórios

1. **System prompt explícito**, definido e documentado antes de começar a construir.
2. **Pelo menos uma técnica de prompt engineering aplicada de propósito**. Com justificativa escrita de por que essa técnica ajuda nesse caso específico.
3. **Teste de curadoria de contexto**: fazer a mesma pergunta de duas formas, uma colando algum arquivo/trecho inteiro no prompt (caso seja um projeto já em andamento) e outra só com o trecho relevante, e comparar quantos tokens cada uma consumiu. Isso seria o equivalente ao `@file`/`@folder` do Claude/Cursor, mas medido em número. Se não for utilizar um projeto já em andamento, pode de fato criar um arquivo com código ou requisitos e referenciar ele.
4. **Cálculo de custo estimado**: para cada chamada, `custo = (tokens_input / 1_000_000) * preco_input + (tokens_output / 1_000_000) * preco_output`, usando o preço do modelo usado (tabela oficial do provedor). Se utilizar algo em free tier, calcule o custo hipotético como se fosse pago. Some o total da sessão.

## 5. Como conseguir os números de tokens e custo

Não precisa ser exclusivamente utilizando o Google AI Studio. Qualquer ferramenta serve, desde que você consiga extrair os dois dados abaixo.

| Ferramenta         | Onde pegar tokens in/out                                                                                                                                                                                                                                                                                                                                                                     | Onde pegar preço                                                                                                                                |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| Google AI Studio   | curl direto na API (endpoint `generativelanguage.googleapis.com`), a resposta JSON traz `usageMetadata.promptTokenCount` e `usageMetadata.candidatesTokenCount`                                                                                                                                                                                                                              | `ai.google.dev/gemini-api/docs/pricing`                                                                                                         |
| Claude Code        | Transcript local em `~/.claude/projects/`, cada mensagem tem `usage.input_tokens`/`output_tokens`; ou comando `/cost` no CLI                                                                                                                                                                                                                                                                 | `anthropic.com/pricing`                                                                                                                         |
| Codex CLI (OpenAI) | CLI: `find ~/.codex/sessions -name '*.jsonl'`; nos eventos `token_count`, usar `last_token_usage` ou `total_token_usage`                                                                                                                                                                                                                                                                     | `platform.openai.com/docs/pricing`                                                                                                              |
| Cursor             | Dashboard em `cursor.com/dashboard` (Usage -> Exportar CSV)                                                                                                                                                                                                                                                                                                                                  | mesmo dashboard, se estiver usando chave própria                                                                                                |
| OpenCode           | CLI: `opencode stats --models 10` mostra tokens in/out e custo por modelo; `opencode export <sessionID> --sanitize > sessao.json` exporta a sessão inteira em JSON (cada mensagem traz `tokens.input`/`tokens.output`); o ID sai de `opencode session list`. O `opencode export` também funciona no TUI via `/export` (atalho `ctrl+x x`), abrindo o editor configurado na variável `EDITOR` | `opencode.ai/pricing` (a tabela do plano Zen é uma referência curada; se você plugou provider próprio, use a tabela de preços daquele provider) |

Free tier (Gemini/AI Studio) custa R$0 de verdade. O cálculo de custo do item 4 é hipotético, como se fosse tier pago. Deixe isso claro no README.

## 6. Entrega no GitHub

- Repositório no GitHub com o código do projeto.
- Adicionar **@pedrosatin** como collaborator do repositório (`Settings > Collaborators > Add people`). Obrigatório mesmo se o repo for público.
- Repositório público é o padrão recomendado (mais simples e sem dependência de plano pago). Privado só se o grupo tiver um motivo específico, nesse caso confirme que seu GitHub cobre Pages em repo privado, senão o deploy não funciona.
- O projeto precisa **funcionar de verdade**, com uma URL publicada e acessível. **Não só rodar local.**

## 7. Como publicar (passo a passo, dois caminhos)

### Caminho A: Google AI Studio (modo Build)

Só funciona se você usou o modo **Build** do AI Studio (gera um app de verdade). Se você só usou o chat normal, não tem projeto pra sincronizar, use o Build ou o Caminho B.

1. No app gerado, clique em **Publish** (canto superior direito).
2. Vá na aba **GitHub**, clique em **Create new repository**.
3. Defina nome, descrição e visibilidade (Public recomendado) e confirme a criação.
4. Confira os arquivos que vão subir e clique em **Stage and commit all changes**.
5. Se o projeto for só HTML/CSS/JS puro, sem framework: vá em `Settings > Pages` no repositório, em **Source** escolha "Deploy from a branch", selecione a branch `main` e a pasta (`/` ou `/docs`, conforme onde estão os arquivos).
6. Se o projeto usa framework (o padrão do AI Studio é React + Vite, que é o caso mais comum), o GitHub Pages não builda sozinho. É preciso um workflow de GitHub Actions customizado (não existe um pronto no marketplace pra isso). Use como base: `github.com/pedrosatin/repo-teste/blob/main/.github/workflows/static.yml`. Nesse caso, em `Settings > Pages`, o **Source** deve ser "GitHub Actions", não "Deploy from a branch".
7. **Atenção ao `base` do Vite**: GitHub Pages de projeto serve em `usuario.github.io/nome-do-repo/`, não na raiz. Se o `vite.config.ts` não tiver `base: '/nome-do-repo/'`, os arquivos estáticos (JS, CSS) dão 404 depois do deploy e a página fica em branco. Ajuste isso antes de publicar.
8. **Permissão do Actions**: se o deploy falhar sem erro claro, confira `Settings > Actions > General > Workflow permissions` e marque "Read and write permissions". Sem isso o workflow não tem permissão de publicar.

### Caminho B: qualquer outra ferramenta (Copilot, Cursor, Claude Code, código feito local)

1. `git init`, commit, `git push` pra um repositório novo no GitHub.
2. Adicione `.env` no `.gitignore` antes do primeiro commit, nunca depois.
3. Deploy via import direto do repositório: **Vercel** (`vercel.com/new`) ou **Cloudflare Pages** (`dash.cloudflare.com` > Pages > Create > conectar ao GitHub). Ambos detectam o framework sozinhos na maioria dos casos e geram a URL automaticamente, sem precisar mexer em Actions.

## 8. README obrigatório

Todo item marcado "com evidências" abaixo exige print de tela. Descrição em texto sem imagem não vale como evidência.

O README do repositório precisa conter, nessa ordem:

1. O que o projeto faz e qual opção você escolheu (projeto de estudo, pessoal, feature da Escola de TI, etc.).
2. O system prompt usado, completo.
3. A técnica aplicada (few-shot ou chain-of-thought) e por que você escolheu ela. Com evidências.
4. O teste de curadoria de contexto: as duas versões do prompt (arquivo inteiro vs. trecho) e a comparação de tokens. Com evidências.
5. Tabela com todas as chamadas (de entrada) feitas: tokens de entrada, tokens de saída, custo estimado por chamada, custo total da sessão. Com evidências.
6. Print ou export do dashboard/log da ferramenta usada, comprovando os números da tabela.
7. Link da URL publicada.
8. Nome e RA de todos os alunos que participaram.

## 9. Rubrica (1,5 ponto)

- **0,6** - Relatório de dados: prompts completos, tokens in/out corretos, custo calculado certo, evidências (prints/logs) batendo com o que está escrito.
- **0,45** - Qualidade da engenharia de prompt: system prompt claro, técnica (few-shot/CoT) bem aplicada e justificada, teste de curadoria de contexto feito de verdade (não simulado).
- **0,45** - Apresentação: clareza do grupo ao explicar o projeto e resposta individual "correta" ou bem fundamentada nas perguntas feitas.

Projeto sem URL publicada funcionando, ou sem @pedrosatin como colaborador, não é considerado entregue. **CERTIFIQUEM-SE DISSO!!**

## 10. Apresentação em sala

Cada grupo tem cerca de 3~5 minutos de pitch/apresentação (o que construiu, os dados coletados) mais até 5 minutos de perguntas. Não se prolonguem.

OBS: **Saiba usar as ferramentas**, explicar o projeto, como fez o deploy no Github, como consultou os dados de custos, como elaborou o prompt. Isso pode e provavelmente será questionado na apresentação. Estejam preparados.
