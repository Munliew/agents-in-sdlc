---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos exercícios anteriores, você atribuiu três tarefas diferentes ao agente de nuvem do Copilot para melhorar a experiência e adicionar funcionalidades. Embora o agente trabalhe de forma assíncrona e autônoma, é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse centro de controle, você vê as tarefas com pull requests abertos, examina as operações realizadas e orienta sessões em andamento.

Neste exercício, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente de nuvem.
- orientar uma sessão em andamento para solicitar outra funcionalidade.

## Cenário

Depois de atribuir ao agente a implementação do modo de alto contraste, a equipe decidiu adicionar também um modo claro. Como o agente já está atualizando os estilos e criando uma opção de alternância, peça que amplie o trabalho para incluir o modo claro.

## Revisar as tarefas do agente de nuvem

Confira o estado atual das tarefas atribuídas ao agente.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Examine as tarefas nas áreas principal e lateral. Você deve encontrar as tarefas que atribuiu, incluindo:
   - adicionar documentação ao código.
   - criar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e revise as operações do Copilot, que podem incluir:
   - obter o código do repositório.
   - criar o ambiente de trabalho.
   - configurar servidores MCP.
   - executar etapas para concluir a tarefa.

> [!NOTE]
> As etapas exibidas variam de acordo com o estado do trabalho e a abordagem do Copilot.

4. Examine também a área de pull requests à direita para acompanhar o PR e os arquivos alterados.

## Orientar o agente de nuvem

Envie uma orientação adicional à sessão que está implementando o modo de alto contraste.

1. Selecione a sessão relacionada ao modo de alto contraste. O título exato depende do nome escolhido pelo Copilot e do estado do trabalho.

   ![Sessão de acessibilidade no centro de controle](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que a configuração termine e o trabalho comece.
3. Na caixa de diálogo **Steer active session while Copilot is working**, envie:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Tarefa do agente de nuvem com a caixa de diálogo de orientação da sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento e inclui a solicitação no trabalho.

## Deixar o Copilot trabalhar

O Copilot incorporará a nova solicitação ao fluxo depois de concluir a etapa em andamento. Isso pode levar alguns minutos; aproveite o tempo para rever o que explorou até agora.

## Resumo e próximos passos

Neste exercício, você explorou a página de agentes do Copilot, o centro de controle para acompanhar e orientar tarefas do agente de nuvem. Você pode acompanhar tarefas em andamento ou concluídas, revisar o trabalho e redirecionar uma sessão ativa.

Você:

- explorou a página de agentes para acompanhar tarefas do agente de nuvem.
- orientou uma sessão em andamento para solicitar outra funcionalidade.

Enquanto o Copilot termina os recursos de acessibilidade, [revise e itere sobre os pull requests que ele criou][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
