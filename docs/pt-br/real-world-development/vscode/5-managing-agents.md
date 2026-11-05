---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

No modo de agente, o GitHub Copilot trabalha de forma autônoma: explora o código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real no seu computador, o Copilot Chat no VS Code mostra as chamadas de ferramentas e as edições. Você pode revisar diffs, aceitar ou rejeitar mudanças individuais e orientar a conversa com prompts complementares sem sair do editor.

Neste exercício, você vai:

- acompanhar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições propostas.
- orientar a sessão para adicionar um controle do modo claro junto ao modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade do [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Durante o trabalho, a equipe de design solicitou também um modo claro para pessoas que consideram o tema padrão escuro demais. Em vez de começar outra conversa, oriente a sessão existente para ampliar o trabalho.

## Acompanhar o progresso do Copilot

A exibição do Copilot Chat mostra as etapas executadas pelo agente: arquivos explorados, comandos executados e alterações propostas. Cada edição aparece no chat com um diff e os controles **Keep** e **Undo**.

1. Volte à exibição Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, acompanhe as chamadas de ferramentas, leituras e edições de arquivos e os comandos de terminal solicitados.
3. Quando uma edição aparecer com diff:
   - selecione **Keep** para aceitar a alteração.
   - selecione **Undo** para revertê-la e permitir outra tentativa.
4. Abra o indicador **Files changed** na parte superior do chat para revisar todos os arquivos alterados na sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão, por exemplo, para executar um comando de terminal, o agente exibirá **Allow**. Aprove apenas os comandos que considerar seguros.

5. Aguarde até que o agente termine a implementação do modo de alto contraste. Uma mensagem final resumirá as alterações.

## Orientar o agente para adicionar o modo claro

Amplie a conversa para incluir um controle do modo claro. Como é a mesma sessão, o agente mantém o contexto dos componentes, dos estilos e do padrão de armazenamento local que já usou.

1. No campo de entrada do Copilot Chat, envie:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. O agente examinará as alterações existentes e proporá mudanças para o modo claro.
3. Revise cada nova edição e use **Keep** ou **Undo** no diff.
4. Se o agente seguir uma direção indesejada, como refatorar os controles de maneira inadequada, interrompa-o com **Stop** ao lado do campo de entrada e envie uma orientação mais clara.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que recomeçar. Use prompts complementares para ajustar ou redirecionar o trabalho, por exemplo: "simplifique o CSS usando variáveis", "integre o modo claro ao painel de configurações existente" ou "adicione atalhos de teclado aos dois controles".

## Revisar as alterações

Revise tudo o que o agente alterou antes de fazer commit.

1. Abra a exibição **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados, incluindo componentes Astro, estilos e testes relacionados.
3. Abra alguns arquivos e examine os diffs. Confirme se há atributos ARIA, navegação por teclado, HTML semântico e persistência pelo armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações pelo painel Source Control. Você as publicará na próxima lição.

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento, revisando cada edição, aceitando ou rejeitando mudanças e enviando prompts complementares sem perder o contexto.

Você:

- acompanhou o trabalho do agente em tempo real.
- aceitou e rejeitou edições individuais.
- orientou a sessão com prompts complementares.

Na [próxima lição][next-lesson], publique o trabalho de acessibilidade em um pull request e revise as alterações do agente personalizado local feitas durante o workshop.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
