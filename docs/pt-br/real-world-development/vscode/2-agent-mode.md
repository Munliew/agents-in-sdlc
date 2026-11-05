---
title: "Exercício 2 - Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até uma pequena atualização de aplicativo pode exigir alterações em vários arquivos, execução de testes e depuração. O modo de agente do Copilot pode explorar a estrutura do projeto, atualizar os arquivos necessários, executar tarefas como testes e corrigir problemas encontrados. Veja como usá-lo para adicionar uma funcionalidade ao site.

> [!NOTE]
> Apesar dos nomes semelhantes, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente trabalha no IDE, com ciclos rápidos de feedback e interação. O agente de nuvem trabalha de forma assíncrona, como uma pessoa da equipe, por meio de issues e pull requests.

Neste exercício, você verá como:

- o modo de agente do Copilot explora o projeto, identifica arquivos relevantes e coordena alterações.
- o modo de agente implementa funcionalidades na interface e na camada de dados.
- revisar alterações e testes antes do merge.

## Cenário

À medida que a lista de jogos cresce, permita que as pessoas filtrem por categoria e editora. Você já criou um helper de editoras no exercício anterior; agora, use o modo de agente do Copilot para concluir o trabalho restante na camada de dados, na interface e nos testes.

## Executar o site da Tailspin Toys

Antes de alterar o projeto, explore o site para conhecer os recursos existentes.

O site da Tailspin Toys é uma plataforma de financiamento coletivo para jogos de tabuleiro com tema de desenvolvimento. Ele lista jogos e exibe seus detalhes. O site é um único aplicativo Astro que gera páginas como HTML estático durante o build. As páginas consultam diretamente um banco de dados SQLite local com o Drizzle ORM; não há uma API de backend separada nem um framework de interface no cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade usa um `<script>` Astro pequeno e escopado, com APIs padrão do DOM.

### Iniciar o site

Use a tarefa `npm run dev`, que inicia o servidor de desenvolvimento Astro no Codespace.

1. Volte ao Codespace. Você deve estar na branch `main` depois do exercício anterior.
2. Abra um terminal com <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie uma branch para a filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, será exibido um endereço semelhante a `http://localhost:4321`.

> [!NOTE]
> Se aparecer uma caixa de diálogo perguntando se deseja abrir o navegador para `http://localhost:4321`, feche-a selecionando **x**.

5. Mantenha pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecione o endereço `http://localhost:4321` no terminal.

> [!NOTE]
> No Codespace, selecionar o link local redireciona para `https://<your-codespace-name>-4321.app.github.dev/`, um túnel privado para o servidor do seu Codespace.

### Explorar o site

Com o site aberto, explore os recursos principais:

- **Página inicial**: lista jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá mais informações, como título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

Peça ao Copilot para mostrar e priorizar as issues do backlog criadas pelo modelo do projeto.

> [!TIP]
> **Abrir o Copilot Chat**
>
> Volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa informa o que selecionar.

1. Na lista de agentes da exibição Chat, selecione **Agent**. Esse agente planeja e implementa alterações em vários arquivos, executa comandos de terminal e usa ferramentas.

   ![Seletor de agente na exibição Chat](../../../_images/shared-chat-mode-selector.png)

2. Na lista de modelos disponíveis, selecione **Claude Sonnet 4.5**.

> [!CAUTION]
> A inclusão de Claude Sonnet 4.5 reflete o modelo usado ao preparar este workshop, não uma preferência por um modelo específico. As sugestões podem variar porque os modelos de linguagem são probabilísticos.

> [!NOTE]
> O Copilot pode usar um comando MCP diferente, mas ainda deve conseguir realizar a tarefa.

3. Peça ao Copilot que mostre e priorize o backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista as issues.
5. Revise a lista gerada.

## Revisar os arquivos de instruções

Antes de pedir a implementação, examine o arquivo de instruções da interface do usuário (UI), que fornece contexto sobre como adicionar funcionalidades ao site.

1. No Codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações, incluindo:
   - a visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para instruções sobre Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os caminhos nos arquivos de instruções são relativos ao próprio arquivo. Assim, é possível reutilizar orientações, dividir conjuntos complexos em arquivos menores e apontar exemplos para o Copilot.

## Implementar a filtragem

A filtragem exige alterações na camada de dados, nos testes e na página de listagem. Peça ao modo de agente para concluir essas tarefas e executar os testes antes do merge.

1. Continue na conversa atual ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** para adicionar o arquivo de instruções.

   ![Seleção do arquivo de instruções da interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado.
4. Confirme que **Claude Sonnet 4.5** continua selecionado.
5. Peça ao Copilot que implemente a funcionalidade da issue:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Acompanhe o Copilot explorando o projeto, encontrando os helpers de dados, a interface e os testes e, em seguida, editando os arquivos e executando os testes.

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> A tarefa pode levar alguns minutos. Durante o trabalho, os controles **Keep** e **Undo** podem aparecer no editor. Aguarde o término antes de aceitar ou rejeitar as alterações.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Solicitação do Copilot Chat para confirmar a execução dos testes](../../../_images/ex3-agent-mode-run-tests.png)

8. É possível que haja pausas ou falhas em alguns testes durante o processo. O Copilot pode alternar entre gerar código e executar testes até concluir o trabalho.
9. Examine o código gerado para identificar possíveis problemas.

   ![Sessão concluída do modo de agente do Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

> [!CAUTION]
> Revise o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador em que o site está em execução e teste a nova funcionalidade.
11. Depois de confirmar o funcionamento e revisar o código, selecione **Keep** na janela Copilot Chat.

## Resumo e próximos passos

Você usou o modo de agente do GitHub Copilot para adicionar recursos ao site da Tailspin Toys. Você:

- usou o modo de agente para explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- implementou funcionalidades na interface e na camada de dados.
- revisou as alterações e os testes antes do merge.

Em seguida, [teste a funcionalidade com o servidor MCP do Playwright][next-lesson] e abra um pull request.

### Exercício extra: implementar paginação

À medida que a lista de jogos crescer, será necessário oferecer paginação. Use as habilidades deste exercício para pedir ao Copilot que a implemente. Considere se a pessoa usuária poderá escolher o tamanho da página ou se ele será fixo. Inclua orientações suficientes no prompt e itere conforme necessário.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
