---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot são assistentes de IA especializados em tarefas ou domínios do fluxo de desenvolvimento. Arquivos Markdown na pasta `.github/agents` do repositório fornecem instruções focadas, práticas recomendadas, padrões de código e conhecimento de domínio. Assim, as equipes podem transformar sua experiência em agentes reutilizáveis, como agentes de acessibilidade que seguem as diretrizes [WCAG][wcag], agentes de segurança ou agentes de testes.

Os agentes personalizados são definidos por arquivos Markdown em `.github/agents` ou, globalmente, em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm algumas semelhanças: ambos são definidos principalmente em arquivos Markdown e orientam uma IA sobre como executar tarefas. Uma forma simples de distingui-los é pensar no **agente personalizado** como quem executa o trabalho e nas **skills** como ferramentas.

Agentes personalizados têm sua própria janela de contexto e podem coordenar skills e outros agentes. Neste laboratório, o agente de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade e poderá usar skills de fluxo de pull requests ou de execução e gerenciamento de testes.

> [!NOTE]
> Não existe uma única forma correta de criar um agente personalizado. Como em outros usos de IA, teste e itere para encontrar o que funciona no seu ambiente e nos seus cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente da visão ou das preferências. Comentários recentes indicam que algumas pessoas têm dificuldade para ler o tema escuro por causa do contraste insuficiente entre texto e fundo. A equipe de design pediu um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é importante, use um agente personalizado para implementar a funcionalidade rapidamente.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar o modo de alto contraste.

## Revisar o agente personalizado de acessibilidade

O repositório já inclui um agente personalizado de acessibilidade. Revise o perfil para entender como ele orientará o Copilot.

1. Volte ao Codespace e abra um terminal.
2. Crie uma branch atualizada a partir de `main` para o trabalho de acessibilidade, separado do PR de filtragem do Exercício 3:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

3. Abra `.github/agents/accessibility.md`.
4. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> O frontmatter com `name` e `description` é obrigatório para agentes personalizados.

5. Examine as seções sobre responsabilidades para gerar um site acessível, práticas recomendadas, exemplos de HTML, CSS e JavaScript e erros comuns.

## Usar o agente personalizado no Copilot Chat

O VS Code mostra os agentes personalizados definidos em `.github/agents` na lista de agentes na parte inferior da exibição Copilot Chat. Selecione um agente para usar suas instruções e ferramentas na sessão.

> [!TIP]
> **Abrir o Copilot Chat**
>
> Volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa informa o que selecionar.

1. Na lista de agentes da exibição Chat, selecione **Agent**, se ainda não estiver selecionado.

   ![Seletor de agente na exibição Chat](../../../_images/shared-chat-mode-selector.png)

2. Abra a lista de agentes na parte inferior do chat, que mostra o agente ativo, normalmente **default**.
3. Selecione **Accessibility agent**.
4. Envie este prompt:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. O Copilot Chat explorará o código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá no chat com o caminho e um diff.
6. Observe o indicador **Files changed**, que é atualizado à medida que o agente altera arquivos.

> [!NOTE]
> O trabalho pode levar alguns minutos. O Copilot editará arquivos reais do repositório, como componentes Astro, estilos CSS e testes relacionados. Você acompanhará e orientará a sessão no próximo exercício.

## Resumo e próximos passos

Neste exercício, você explorou os [agentes personalizados][custom-agents], assistentes especializados em tarefas e domínios específicos. Eles permitem transformar a experiência e os padrões da equipe em orientações reutilizáveis para o Copilot.

Você:

- revisou um agente personalizado de acessibilidade existente.
- usou um agente personalizado no modo de agente do Copilot Chat.

Em seguida, [acompanhe e oriente o trabalho do agente][next-lesson], revise as alterações enquanto são feitas e solicite a inclusão de um controle para o modo claro na mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar a funcionalidade com o MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
