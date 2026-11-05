---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Muitas organizações têm dívida técnica: problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de funcionalidades paradas no backlog. O agente de nuvem do GitHub Copilot pode atualizar código e adicionar funcionalidades de forma autônoma. Ao terminar, ele cria um pull request (PR) em rascunho para revisão de uma pessoa desenvolvedora. Assim, é possível delegar tarefas repetitivas e concentrar-se em outras prioridades.

Neste exercício, você explorará como:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- criar issues com escopo claro.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys quer lidar com parte da dívida técnica. A documentação do código está incompleta; como primeira etapa, a equipe quer comentários TSDoc em todas as funções exportadas.

A equipe de design também quer facilitar a descoberta de jogos. Na página de detalhes de cada jogo, deve haver uma seção de títulos relacionados da mesma categoria para permitir testes de aceitação da experiência. Essa tarefa bloqueia a exploração do design, mas há issues de prioridade maior.

São bons exemplos de tarefas que podem ser adiadas. O agente de nuvem pode trabalhar nelas de forma assíncrona enquanto você cuida de outras atividades e, depois, revisa os resultados.

## Sobre o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot][copilot-agents] executa tarefas em segundo plano, de forma semelhante a uma pessoa desenvolvedora. Por exemplo, você pode [atribuir uma issue do GitHub ao Copilot][assign-issue]. O Copilot criará um PR em rascunho, preparará um ambiente e começará o trabalho. Você pode acompanhar a sessão enquanto ela está em andamento ou depois que for concluída. Quando a solução estiver pronta para revisão, o agente marcará você no PR.

## A importância de instruções com escopo claro

O GitHub Copilot não elimina a necessidade de pensar. Até tarefas aparentemente simples podem ter detalhes complexos; por isso, defina com cuidado o escopo do trabalho do agente de nuvem e siga [as práticas recomendadas][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação: trabalhe por etapas, aprenda, experimente e adapte o processo.

## Instruções personalizadas neste repositório

Antes de atribuir trabalho ao agente de nuvem, examine os arquivos de instruções do repositório para reconhecer a influência deles nos resultados.

1. Abra `.github/copilot-instructions.md` na interface Web do GitHub ou em um Codespace.
2. Revise a seção **Code standards**, principalmente as convenções de TypeScript e as expectativas para comentários TSDoc.
3. Abra `.github/instructions/unit-tests.instructions.md` e observe o frontmatter `applyTo`, que limita as instruções a arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente, observe os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de arquivo no PR gerado. Essas orientações vêm dos arquivos de instruções do repositório.

## Configurar o ambiente de desenvolvimento do agente

O agente de nuvem usa o [GitHub Actions][github-actions] para preparar o ambiente de trabalho. Você pode personalizá-lo com um workflow especial em `.github/workflows/copilot-setup-steps.yml`, que é executado antes do agente começar. O modelo do workshop já inclui essa configuração: ela disponibiliza Node.js, dependências do projeto, navegadores para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido do aplicativo Astro.

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina apenas as permissões mínimas necessárias para estas etapas.
    permissions:
      # Use contents: read se as etapas precisarem clonar o repositório,
      # por exemplo, para instalar dependências. Sem isso, o Copilot fará o clone.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do aplicativo Astro com Drizzle e libSQL.
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e preenche o banco de dados SQLite local para builds e testes.
      - name: Set up the database
        run: npm run db:setup
```

Esse arquivo se parece com outros workflows do GitHub Actions, mas tem alguns pontos importantes:

- O job único, `copilot-setup-steps`, é executado antes de o Copilot começar a trabalhar no PR.
- O gatilho `workflow_dispatch` permite executar o workflow manualmente na guia **Actions** para testá-lo sem aguardar uma sessão do Copilot.

## Adicionar documentação ao código

Documentação desatualizada ou ausente dificulta a manutenção do código e a integração de novas pessoas à equipe. A criação de documentação é uma tarefa adequada para o agente de nuvem, que pode trabalhar em segundo plano enquanto você faz outras atividades.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de criação de issue.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Code lacks documentation`.
6. No campo **Description**, digite:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.

   ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

   ![Detalhes da atribuição do Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o PR recém-criado, com título semelhante a `[WIP]: Code lacks documentation`. Se não aparecer, aguarde um momento e atualize a página.
12. Aguarde o Copilot criar uma lista de tarefas.

> [!NOTE]
> A lista pode levar alguns minutos para aparecer. O Copilot está preparando o ambiente com o workflow descrito acima, analisando o projeto e definindo a abordagem.

13. Revise a lista de tarefas.
14. Na linha do tempo do PR, procure a atualização que indica que o Copilot começou a trabalhar na issue.
15. Selecione **View session**.

   ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

16. Acompanhe a sessão, incluindo a exploração do código, a análise do estado do projeto, o planejamento e a geração de código.

O trabalho pode levar alguns minutos. Enquanto o agente trabalha, você pode começar outra tarefa. Atribua mais uma issue ao agente de nuvem e depois continue com o exercício.

## Adicionar uma seção de jogos relacionados

Delegue também a funcionalidade de jogos relacionados enquanto trabalha em outras tarefas.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** e depois **Blank issue**.
4. No campo **Title**, digite `Show related games on the game details page`.
5. No campo **Description**, digite:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

6. Selecione **Create** para criar a issue.
7. À direita, selecione **Assign to Copilot** e depois **Assign**.

   ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

O Copilot começará a trabalhar na segunda solicitação. Como o agente de nuvem trabalha de forma assíncrona, não é necessário acompanhá-lo continuamente. Mais tarde, você revisará o resultado. Enquanto isso, passe a criar e usar agentes personalizados.

## Resumo e próximos passos

Neste exercício, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents] e como atribuir issues para execução assíncrona. O agente pode lidar com dívida técnica, criar funcionalidades ou ajudar na migração de código entre frameworks.

Você:

- personalizou o ambiente de geração de código.
- considerou como realizar operações com segurança.
- criou issues com escopo claro.
- atribuiu issues ao Copilot.

Enquanto o agente trabalha em segundo plano, passe a criar e usar agentes personalizados. O [agente de nuvem também pode usar servidores MCP][cloud-agent-mcp] e instruções personalizadas, exploradas anteriormente.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
