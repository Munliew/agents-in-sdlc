---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa deve ser feita de uma maneira específica — ou se o Copilot precisa conhecer informações de contexto — disponibilize essas informações. Os [arquivos de instruções][instruction-files] fornecem orientações para que o Copilot entenda não apenas *o que* você quer, mas também *como* realizar o trabalho.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot pelas instruções personalizadas do repositório e pelos arquivos com escopo por caminho.
- adicionar um novo padrão válido para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos percursos de VS Code e CLI, aqui você não fará uma comparação antes e depois do prompt. O agente de nuvem trabalha de forma assíncrona nas issues do GitHub, então é mais difícil demonstrar a mudança em tempo real. Você verá a influência das instruções ao revisar os pull requests gerados pelo agente mais adiante.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes de desenvolvimento, incluindo testes unitários para a camada de dados, uma interface escura e moderna, comentários TSDoc no código e um bloco de comentários no início de cada arquivo explicando sua finalidade. Use arquivos de instruções para fornecer ao Copilot o contexto necessário para seguir essas diretrizes.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot, como convenções de programação, bibliotecas e comentários. Você pode criar instruções para todo o repositório ou para tipos específicos de arquivos.

- `.github/copilot-instructions.md` é o único arquivo de instruções enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações gerais do projeto, como a stack, o que está sendo criado e as práticas recomendadas.
- `.github/instructions/*.instructions.md` contém instruções para tipos de arquivo ou tarefas específicas, como TypeScript, Astro, componentes da interface ou testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, não para preenchimento de código nem sugestões da próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem usam os arquivos do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também oferece suporte a outros padrões de arquivos de instruções, incluindo `AGENTS.md` e `CLAUDE.md`; consulte a [referência de suporte a instruções personalizadas][custom-instructions-support].

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

## Explorar os arquivos de instruções do projeto

Leia os arquivos existentes no editor ou na interface Web do GitHub para entender as orientações disponíveis.

1. Abra `.github/copilot-instructions.md`.
2. Examine a descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**.
3. Explore os arquivos da pasta `.github/instructions`, que contém orientações para Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo` no frontmatter. Esse glob, relativo à raiz do repositório, define quais arquivos correspondem às instruções. Por exemplo, arquivos de teste TypeScript como `**/*.test.ts` correspondem a ele.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e examine os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de programação, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de arquivo.

## Adicionar um padrão para todo o repositório

Adicione orientações para comentários TSDoc e um comentário que explique a finalidade de cada arquivo. O agente de nuvem usará essas instruções nos exercícios seguintes.

1. No terminal do Codespace, crie e selecione uma branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Abra `.github/copilot-instructions.md`.
3. Localize a seção **Code formatting requirements**, por volta da linha 27.
4. Logo abaixo dos padrões existentes, adicione:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. Salve `copilot-instructions.md`.

> [!TIP]
> Use o arquivo do repositório para padrões globais e arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. A regra de comentários de documentação é válida para todo o projeto.

## Fazer commit, enviar e integrar a atualização

O agente de nuvem lê as instruções da branch de destino da issue. No próximo exercício, ele partirá de `main`; portanto, integre suas alterações em `main` para que o agente possa usá-las.

1. Prepare e faça commit da atualização:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request de `custom-instructions-cloud` para `main` e faça o merge.

> [!TIP]
> Se preferir trabalhar diretamente em `main` neste workshop, você pode ignorar a etapa da branch e fazer o commit diretamente em `main`. A branch serve para reproduzir o fluxo de um projeto real.

## Resumo e próximos passos

Você explorou como o Copilot recebe contexto dos arquivos de instruções do projeto e adicionou um padrão global a `.github/copilot-instructions.md`. Você verá a influência desse padrão nos pull requests que o agente de nuvem gerar.

Em seguida, [atribua sua primeira issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
