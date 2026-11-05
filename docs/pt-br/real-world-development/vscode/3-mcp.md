---
title: "Exercício 3 - Testar a funcionalidade com o servidor do Model Context Protocol (MCP) do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar a funcionalidade de filtragem com o modo de agente. Antes de abrir um pull request, confirme que ela funciona no navegador. Configure o **servidor MCP do Playwright** para que o Copilot controle um navegador real e teste a funcionalidade; depois, publique sua branch e abra o PR.

Neste exercício, você vai:

- entender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot.
- adicionar o servidor MCP do Playwright ao espaço de trabalho.
- pedir ao Copilot que teste manualmente a filtragem no navegador.
- publicar a branch e abrir um pull request.

## O que é o Model Context Protocol?

O modo de agente fica mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) é um padrão que permite ao agente se comunicar com ferramentas e serviços externos.

![Diagrama da estrutura do modo de agente e de sua interação com o contexto, o modelo de linguagem de grande porte (LLM) e as ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem em tempo real com ferramentas e serviços externos. Eles podem acessar informações atualizadas por meio de recursos e executar ações em seu nome por meio de ferramentas.

Um servidor MCP funciona como ponte entre o agente e as ferramentas e serviços externos, gerenciando a comunicação com APIs ou ferramentas locais, como pacotes npm. Cada servidor fornece um conjunto próprio de ferramentas e recursos.

- O **[servidor MCP do GitHub](https://github.com/github/github-mcp-server)** oferece APIs para gerenciar repositórios, issues e pull requests.
- O **[servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp)** permite automatizar o navegador, por exemplo, navegar em páginas, preencher formulários e selecionar botões.

O GitHub mantém um [registro MCP](https://github.com/mcp) para facilitar a descoberta de servidores e contribuições ao ecossistema.

> [!CAUTION]
> Por segurança, trate servidores MCP como qualquer outra dependência. Revise o código-fonte, verifique o publicador e considere as implicações de segurança. Use somente servidores confiáveis e tenha cuidado ao conceder acesso a recursos ou operações sensíveis.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste espaço de trabalho.

1. Abra `.vscode/mcp.json` no Codespace.
2. Observe que já há um servidor `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

Essa entrada vem do modelo do projeto. Ela permite que o Copilot leia o backlog de issues usando o [servidor MCP remoto do GitHub][remote-github-mcp-server]. Não é preciso instalá-lo localmente: o VS Code se conecta por HTTP e solicita autenticação do GitHub na primeira utilização.

## Adicionar o servidor MCP do Playwright

Adicione um segundo servidor para que o Copilot possa controlar um navegador e testar a funcionalidade.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Salve o arquivo.

O servidor `playwright` é local: o VS Code o inicia no computador com `npx @playwright/mcp@latest`. A opção `--headless` executa o navegador sem uma janela visível, necessário em um Codespace sem área de trabalho gráfica.

> [!NOTE]
> O projeto já usa Playwright em testes de ponta a ponta, então o navegador necessário geralmente já está instalado. Se o Copilot informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia servidores MCP sob demanda. Na primeira vez que o Copilot precisar das ferramentas do servidor, o VS Code o iniciará e pedirá confirmação de confiança.

1. Em `.vscode/mcp.json`, selecione **Start**, exibido acima da entrada `playwright`, para iniciar o servidor e confirmar a configuração.
2. Se o VS Code perguntar se você confia no servidor, revise a configuração e confirme para permitir a inicialização.

Iniciá-lo manualmente é opcional. Se você pular essa etapa, o VS Code iniciará o servidor e pedirá confirmação quando o Copilot usar as ferramentas.

## Testar a filtragem

Com o servidor conectado e o aplicativo em execução, peça ao Copilot para testar a filtragem usando o navegador.

1. Envie este prompt:

   ```text
   Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

   1. Go to the games page and note how many games are listed.
   2. Apply a category filter and confirm the list updates to only show games in that category.
   3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
   4. Combine a category and a publisher filter and confirm the results respect both.

   Report what you observe at each step, and call out anything that does not behave as expected.
   ```

2. Compare o relatório do Copilot com os critérios de aceitação da issue. Se algo não estiver correto, faça perguntas complementares ou peça que corrija o código antes de abrir o PR.

O Copilot abrirá o navegador pelo servidor MCP do Playwright, seguirá as etapas e relatará o que observou.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321`. Se o servidor de desenvolvimento tiver sido parado, inicie-o antes de enviar o prompt. Na primeira utilização, o Playwright MCP pode precisar baixar um navegador; se isso acontecer, peça ao Copilot que execute `npx playwright install chromium`.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a branch e criar um pull request

Depois de confirmar que a funcionalidade funciona, publique a branch `filtering-vscode` para que a equipe possa revisar.

1. Abra o painel **Source Control** no Codespace e revise as alterações do Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit com o botão **Sparkle**.

   ![Painel Source Control com as alterações do Copilot](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Criar o pull request

Peça ao Copilot para localizar a issue relacionada e abrir um PR associado a ela.

1. Abra o painel Copilot Chat e selecione **New Chat**.
2. Na lista de agentes, confirme que **Agent** está selecionado para permitir o uso das ferramentas do GitHub.
3. Envie este prompt:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Selecione **Continue** quando for necessário permitir que o Copilot reúna informações e execute operações. Na primeira utilização de uma ferramenta do GitHub, talvez seja solicitado que você se autentique.
5. Observe o Copilot pesquisar as issues, localizar a correta e criar o PR.
6. Abra o link criado pelo Copilot para revisar o PR, mas **não faça o merge ainda**.

## Resumo e próximos passos

Você:

- entendeu o que é o MCP e como os servidores ampliam os recursos do Copilot.
- adicionou o servidor MCP do Playwright ao espaço de trabalho.
- usou o servidor para testar a filtragem no navegador antes de publicá-la.
- publicou a branch e abriu um pull request.

Você usou um servidor MCP para testar uma funcionalidade. Em seguida, [crie um agente personalizado][next-lesson] para tarefas focadas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todos estão falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
