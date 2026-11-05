---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use Plan para definir a abordagem, Autopilot para implementar a filtragem a partir de uma issue e Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando uma pequena funcionalidade, mas mudanças maiores exigem um processo mais robusto. Nesta lição, você seguirá um fluxo de desenvolvimento orientado por agentes: partirá de uma issue, verificará o código e o comportamento da funcionalidade e, por fim, preparará uma alteração para integração.

> [!NOTE]
> Você continuará o fluxo da funcionalidade usando a mesma conversa e branch. Em geral, tarefas diferentes podem usar branches ou PRs separados; aqui, vamos simplificar para manter o foco nos conceitos principais.

Nesta lição, você vai:

- iniciar uma conversa do Copilot a partir de uma issue do GitHub.
- definir os requisitos no modo Plan.
- implementar a funcionalidade no modo Autopilot.
- revisar o código.
- validar manualmente a funcionalidade no navegador encaminhado.

Nas lições seguintes, você atualizará as instruções do repositório, personalizará a skill `quality-checks`, adicionará validação por MCP, criará um agente de QA e abrirá o PR da funcionalidade.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar jogos por categoria e editora. A issue do backlog descreve a funcionalidade, mas decisões como combinar categorias precisam ser definidas antes de programar. Use o modo Plan para resolver essas questões e depois autorize uma implementação delimitada com o Autopilot.

## Contexto

Agentes de programação com IA não mudam os fundamentos do desenvolvimento. Um fluxo comum é:

1. Começar por uma issue registrada que descreva o trabalho.
2. Planejar o que será criado.
3. Desenvolver e revisar o código.
4. Executar os testes.
5. Validar manualmente a funcionalidade.
6. Criar um pull request (PR).
7. Integrar o código depois da revisão e da aprovação da integração contínua.

> [!NOTE]
> Os detalhes variam de acordo com a equipe e a organização, mas muitos fluxos seguem uma abordagem semelhante.

Esse processo ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pela mesma revisão que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar:

- **Interactive**: você e o agente trabalham juntos. O agente sugere mudanças e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano e não pode editar os arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma, escrevendo código, executando testes e iterando até concluir a tarefa.

Comece no modo Plan, revise o plano e depois use o Autopilot para implementá-lo.

## Começar pela issue

Prepare o Codespace, o repositório e o terminal antes de iniciar o trabalho de filtragem.

1. Volte ao Codespace. Se estiver parado, reinicie-o.
2. Confirme que o PR das avaliações por estrelas foi integrado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as setas para selecionar a issue **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

O prompt começará com `#7` ou um número semelhante. O prefixo `#` adiciona o contexto de uma issue ou de um pull request do GitHub.

## Planejar a filtragem

Planeje a abordagem antes de delegar ao Copilot a implementação de uma funcionalidade complexa.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme que o indicador abaixo do prompt mostra **Plan**.
2. Depois da referência à issue, digite:

   ```plaintext
   Create a plan for implementing this feature.
   ```

3. Se o Copilot perguntar como a filtragem deve funcionar, responda de acordo com suas preferências.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Percorra o plano e revise-o.
6. Peça ao Copilot para corrigir as partes que não correspondam às suas decisões.

## Aprovar o Autopilot

Depois de revisar o plano, use o Autopilot para implementá-lo.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção equivalente na versão instalada.
2. Confirme que o indicador do modo mostra **Autopilot**.
3. Acompanhe o Copilot enquanto ele implementa o plano e executa os testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente; revise o plano antes. Se o Copilot informar que faltam dependências ou que há conflito de porta, resolva o problema antes de considerar as verificações concluídas.

## Revisar e verificar a implementação

Revise o código e os resultados das verificações antes do merge.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador não mostra **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com a issue e com as decisões do plano.
4. Ao terminar a revisão, pressione <kbd>Esc</kbd> para sair da visualização do diff.
5. Revise os resultados das verificações do projeto e peça ao Copilot para corrigir eventuais falhas.

## Explorar a funcionalidade

Execute o aplicativo e abra o site pela porta encaminhada do Codespaces para testar a filtragem.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste os filtros por categoria e editora, inclusive as combinações acordadas no plano.
4. Confirme que o comportamento de redefinição e de resultados vazios corresponde à issue e às suas decisões.
5. Volte ao Codespace e peça ao Copilot para parar o servidor iniciado.

## Resumo e próximos passos

Você:

- iniciou uma conversa do Copilot a partir de uma issue do GitHub.
- definiu os requisitos no modo Plan.
- implementou a funcionalidade no modo Autopilot.
- revisou o código.
- validou manualmente a funcionalidade no navegador encaminhado.

Em seguida, [use instruções personalizadas][next-lesson] para orientar como o código é gerado.

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
