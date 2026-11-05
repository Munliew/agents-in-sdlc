---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot são assistentes de IA especializados em tarefas ou domínios do fluxo de desenvolvimento. Arquivos Markdown na pasta `.github/agents` do repositório fornecem instruções focadas, práticas recomendadas, padrões de código e conhecimento de domínio. Assim, as equipes podem transformar sua experiência em agentes reutilizáveis, como agentes de acessibilidade que seguem as diretrizes [WCAG][wcag], agentes de segurança ou agentes de testes.

Os agentes personalizados são definidos por arquivos Markdown em `.github/agents` ou, globalmente, em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm algumas semelhanças: ambos são definidos principalmente em arquivos Markdown e orientam uma IA sobre como executar tarefas. Uma forma simples de distingui-los é pensar no **agente personalizado** como quem executa o trabalho e nas **skills** como ferramentas.

Agentes personalizados têm sua própria janela de contexto e podem coordenar skills e outros agentes. Neste workshop, o agente de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade e poderá usar skills de fluxo de pull requests ou de execução e gerenciamento de testes.

> [!NOTE]
> Não existe uma única forma correta de criar um agente personalizado. Como em outros usos de IA, teste e itere para encontrar o que funciona no seu ambiente e nos seus cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Neste exercício, você vai:

- revisar como agentes personalizados são definidos.
- atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys quer que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente da visão ou das preferências. Comentários recentes indicam que algumas pessoas têm dificuldade para ler o tema escuro por causa do contraste insuficiente entre texto e fundo. A equipe de design pediu um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é importante, use um agente personalizado para implementar a funcionalidade rapidamente.

## Revisar o agente personalizado de acessibilidade

O repositório já inclui um agente personalizado de acessibilidade. Revise o perfil para entender como ele orientará o Copilot.

1. Volte ao Codespace e abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML e os campos `name` e `description`.

> [!CAUTION]
> O frontmatter com `name` e `description` é obrigatório para agentes personalizados.

3. Examine as seções seguintes, que descrevem responsabilidades para gerar um site acessível, práticas recomendadas, exemplos de HTML, CSS e JavaScript e erros comuns.

## Criar e atribuir uma issue

A página de agentes é o centro de controle para atribuir tarefas ao agente de nuvem, acompanhar o trabalho e fornecer orientações adicionais. Crie uma issue para o modo de alto contraste e atribua-a ao agente personalizado.

1. Acesse seu repositório.
2. Selecione a guia **Issues**.
3. Selecione **New issue** e depois **Blank issue**.
4. No campo **Title**, digite `Add high contrast mode to website`.
5. No campo **Description**, digite:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

6. Selecione **Create** para criar a issue.
7. À direita, selecione **Assign to Copilot**.
8. Na lista de agentes personalizados, selecione **Accessibility agent**.

   ![Caixa de atribuição do agente de nuvem com o agente personalizado de acessibilidade selecionado](../../../_images/ex5-select-custom-agent.png)

9. Selecione **Assign**.

O Copilot começará a trabalhar na tarefa em segundo plano.

## Resumo e próximos passos

Neste exercício, você explorou os [agentes personalizados][custom-agents], assistentes especializados em tarefas e domínios específicos. Eles permitem transformar a experiência e os padrões da equipe em orientações reutilizáveis para o Copilot.

Você:

- revisou como agentes personalizados são definidos.
- atribuiu uma tarefa a um agente personalizado.

Enquanto o Copilot implementa o modo de alto contraste, [acompanhe e oriente a sessão do agente][next-lesson] no centro de controle.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar o uso de agentes personalizados na organização][org-custom-agents]
- [Preparar o uso de agentes personalizados na empresa][enterprise-custom-agents]

---

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
