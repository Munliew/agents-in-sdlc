---
title: "Lição 4 - Orientar o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é essencial ao trabalhar com IA generativa. Os [arquivos de instruções][instruction-files] descrevem não apenas *o que* você quer, mas também *como* o código deve ser estruturado. Agora que a filtragem está implementada, explore as instruções usadas pelo Copilot, adicione um padrão de documentação e aplique-o ao código.

Nesta lição, você vai:

- explorar como as instruções do repositório e os arquivos com escopo por caminho chegam ao agente.
- atualizar as instruções para que os padrões de programação sejam seguidos.
- observar o impacto dos arquivos de instruções no código.

## Cenário

A Tailspin Toys tem diretrizes de desenvolvimento: comentários devem explicar a intenção e decisões não óbvias; funções exportadas em `db/` e `src/lib/` devem documentar finalidade, parâmetros e retornos com TSDoc/JSDoc; componentes Astro reutilizáveis devem documentar seus contratos `Props`; e os comentários devem continuar atualizados quando o código mudar. As orientações existentes de formatação e lint também devem ser preservadas.

## Arquivos de instruções

As instruções personalizadas fornecem ao Copilot contexto e preferências, como convenções de programação, bibliotecas e comentários. Você pode criar instruções para todo o repositório ou para tipos específicos de arquivos.

- `.github/copilot-instructions.md` é o arquivo único enviado ao Copilot em todas as solicitações do repositório e deve conter orientações gerais do projeto.
- `.github/instructions/*.instructions.md` contém orientações para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> O suporte a outros formatos de instrução varia de acordo com o ambiente. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explorar as instruções personalizadas do projeto

Leia os arquivos existentes antes de alterá-los para entender como o Copilot recebe orientações.

1. Volte ao Codespace.
2. No editor do Codespaces, não no terminal, abra `.github/copilot-instructions.md`.
3. Examine a descrição do projeto e as orientações de programação, aplicáveis a todas as interações com o Copilot neste repositório.
4. Explore os arquivos na pasta `.github/instructions`, que incluem orientações para Astro, a camada de dados Drizzle, testes e outros tópicos.
5. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo`, que define um glob para determinar a quais arquivos as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e examine os links para outros arquivos de instruções e arquivos do projeto. Esses links permitem dividir instruções extensas em arquivos menores e reutilizáveis, além de apontar exemplos.

## Atualizar as instruções para refletir os padrões da equipe

Adicione aos padrões globais a exigência de comentários TSDoc para o TypeScript recém-gerado.

1. Em `.github/copilot-instructions.md`, localize a seção **Code formatting guidance**, por volta da linha 35.
2. Adicione este item como o último tópico:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

## Usar as instruções atualizadas

O Copilot CLI carrega as instruções do repositório ao iniciar uma conversa. Retome a conversa de filtragem para aplicar as novas orientações sem perder o contexto da funcionalidade.

1. Se a conversa ainda estiver aberta, digite `/exit` para fechá-la. O Copilot CLI salva as conversas automaticamente.
2. Na raiz do repositório e na branch `game-filters-cli`, retome a conversa:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme que a branch `game-filters-cli` está selecionada.
3. Peça ao Copilot que aplique as orientações:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Digite `/diff` e revise os arquivos TypeScript alterados. Confirme que os comentários TSDoc recém-gerados descrevem o código corretamente.

## Resumo e próximos passos

Você:

- explorou como as instruções do repositório e os arquivos com escopo por caminho chegam ao agente.
- atualizou as instruções para que os padrões de programação sejam seguidos.
- observou o impacto dos arquivos de instruções no código.

Em seguida, [personalize e execute a skill reutilizável `quality-checks`][next-lesson] para executar testes e lint de maneira consistente.

## Recursos

- [Adicionar instruções personalizadas ao Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Práticas recomendadas para instruções personalizadas][instructions-best-practices]
- [Awesome Copilot — coleção de instruções e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
