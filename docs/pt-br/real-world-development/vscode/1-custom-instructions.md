---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa deve ser feita de uma maneira específica — ou se o Copilot precisa conhecer informações de contexto — disponibilize essas informações. Os [arquivos de instruções][instruction-files] orientam como o Copilot deve gerar o código.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot pelas instruções personalizadas do repositório e pelos arquivos com escopo por caminho.
- gerar a primeira parte dos dados de filtragem, um helper de editoras, com as instruções atuais.
- adicionar um novo padrão válido para todo o repositório em `.github/copilot-instructions.md`.
- repetir o prompt e observar o código gerado adotar o novo padrão.
- fazer commit das instruções e da primeira parte da filtragem em `main` para que o Copilot use as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir todos os padrões definidos, pois o Copilot é não determinístico. O objetivo é observar a tendência de mudança após atualizar as instruções, não obter uma saída idêntica caractere por caractere.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes de desenvolvimento, incluindo testes unitários para a camada de dados, uma interface escura e moderna, comentários TSDoc no código e um bloco de comentários no início de cada arquivo explicando sua finalidade. Use arquivos de instruções para fornecer ao Copilot o contexto necessário.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot, como convenções de programação, bibliotecas e comentários. Você pode criar instruções para todo o repositório ou para tipos específicos de arquivos.

- `.github/copilot-instructions.md` é o único arquivo de instruções enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações gerais do projeto, como a stack, o que está sendo criado e as práticas recomendadas.
- `.github/instructions/*.instructions.md` contém instruções para tipos de arquivo ou tarefas específicas, como TypeScript, Astro, componentes da interface ou testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, não para preenchimento de código nem sugestões da próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem usam os arquivos do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também oferece suporte a outros padrões, incluindo `AGENTS.md` e `CLAUDE.md`; consulte a [referência de suporte a instruções personalizadas][custom-instructions-support].

### Práticas recomendadas para gerenciar arquivos de instruções

Os exemplos no projeto mostram uma abordagem possível:

- Mantenha `copilot-instructions.md` focado nas orientações gerais, como descrição e estrutura do projeto e padrões globais de programação.
- Use arquivos `*.instructions.md` para tipos de arquivo ou tarefas específicas, como testes unitários, componentes Astro e a camada de dados.
- Use linguagem natural e orientações claras. Inclua exemplos do que fazer e do que evitar.

Não há uma única forma correta de criar arquivos de instruções ou usar IA. Experimente para descobrir o que funciona melhor no projeto.

> [!TIP]
> Projetos que usam o GitHub Copilot podem se beneficiar de uma coleção sólida de arquivos de instruções. Neste projeto, há orientações para [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada ambiente oferece essa opção de maneira diferente, como **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI.
>
> Para encontrar modelos e exemplos, explore o [awesome-copilot][awesome-copilot], que reúne arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas do projeto

Leia os arquivos existentes no editor ou na interface Web do GitHub para entender as orientações disponíveis.

1. Abra `.github/copilot-instructions.md`.
2. Examine a descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**.
3. Explore os arquivos na pasta `.github/instructions`, que contém orientações para Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo` no frontmatter. Esse glob, relativo à raiz do repositório, define quais arquivos correspondem às instruções. Por exemplo, arquivos de teste TypeScript como `**/*.test.ts` correspondem a ele.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e examine os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de programação, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de arquivo.

## Criar uma branch para as alterações

Crie uma branch para trabalhar nas instruções e no helper.

1. Volte ao Codespace do exercício anterior.
2. Abra um terminal com <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie e selecione uma branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat antes de atualizar as instruções

Gere o helper com as instruções atuais para depois comparar o resultado com a mesma solicitação após a atualização.

> [!TIP]
> **Abrir o Copilot Chat**
>
> Volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa informa o que selecionar.

1. Feche as guias do editor abertas em exercícios anteriores para limitar o contexto do Copilot.
2. Abra `src/lib/publishers.ts` para mostrar onde o helper deve ficar.
3. Na lista de agentes da exibição Chat, selecione **Agent** para permitir alterações nos arquivos.

   ![Seletor de agente na exibição Chat](../../../_images/shared-chat-mode-selector.png)

4. Envie este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará alterações, que podem abranger o helper e seus testes.
6. Observe que a função tipada sugerida recebe um cliente `db` como primeiro argumento e retorna um array tipado de editoras. Isso segue as convenções da camada de dados em `.github/instructions/drizzle.instructions.md`, aplicável a `src/lib/*.ts`.
7. Observe se faltam comentários TSDoc e um comentário no nível do arquivo.

> [!CAUTION]
> Como o Copilot é não determinístico, talvez ele adicione comentários mesmo sem essa orientação. Tudo bem: o objetivo continua sendo observar a consistência depois da atualização.

## Adicionar um padrão para todo o repositório

Atualize as orientações gerais para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, por volta da linha 27, e observe que ainda não há uma regra de documentação no código.
3. Logo abaixo dos padrões existentes, adicione:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> O arquivo do repositório é adequado para padrões globais. Arquivos `*.instructions.md` são úteis para linguagens, tipos de arquivo ou tarefas específicas.

## Repetir o prompt e observar a mudança

Envie novamente o mesmo prompt para ver como o Copilot aplica o novo padrão.

1. No Copilot Chat, selecione **New Chat** para limpar o contexto da conversa.
2. Selecione novamente `src/lib/publishers.ts`.
3. Envie o mesmo prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe se o arquivo proposto começa com um comentário semelhante a:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe se a função inclui um comentário TSDoc semelhante a:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

O Copilot recebeu um novo padrão e o aplicou ao código que servirá de base para o próximo exercício.

## Fazer commit das instruções e enviar a branch

Registre as instruções e todas as alterações associadas ao helper no controle de versão.

1. Abra um terminal com <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Confira as alterações:

   ```bash
   git status
   ```

3. Prepare e faça commit das instruções e de todos os arquivos alterados pelo Copilot para o helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras alterações relacionadas, como testes, adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e integrar um pull request

Crie um pull request associado à issue do backlog sobre as instruções. As ferramentas do GitHub já estão conectadas ao projeto e o Copilot pode encontrar a issue e abrir o PR.

> [!NOTE]
> Esta é a primeira etapa em que o Copilot atuará no GitHub. Na primeira utilização de uma ferramenta do GitHub, o VS Code pode pedir que você entre na conta e autorize o acesso. Siga as instruções exibidas. A conexão será explicada em um exercício posterior sobre Model Context Protocol (MCP).

1. Abra o Copilot Chat no Codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a exibição Copilot Chat e confirme que **Agent** está selecionado.
3. Peça ao Copilot para localizar a issue e abrir um PR a partir da branch atual:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Aguarde o Copilot localizar a issue e criar o PR.
5. Quando solicitado a permitir operações do GitHub, revise o comando e selecione **Allow** se estiver de acordo.
6. Depois da criação do PR, peça ao Copilot para integrá-lo a `main` e atualizar a branch local:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Quando solicitado a permitir operações do GitHub ou comandos de shell, revise-os e selecione **Allow** se estiver de acordo.

## Resumo e próximos passos

Você explorou como o Copilot recebe contexto dos arquivos de instruções e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código com as instruções existentes.
- adicionar um padrão global a `.github/copilot-instructions.md`.
- repetir o prompt e observar as sugestões adotarem o padrão.
- integrar as instruções e a base do helper em `main` para que o próximo exercício possa usá-las.

Em seguida, aplique as instruções no [modo de agente][next-lesson] para adicionar uma funcionalidade.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
