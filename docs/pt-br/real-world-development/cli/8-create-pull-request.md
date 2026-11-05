---
title: "Lição 8 - Criar e integrar o PR da funcionalidade"
description: "Revise a filtragem, as instruções, a skill de QA e os testes; depois crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma branch. Revise tudo em conjunto e abra um pull request. Você integrou manualmente o PR das avaliações por estrelas; desta vez, deixe o **Agent Merge** gerenciar o processo.

> [!NOTE]
> Em geral, a funcionalidade, as instruções, a atualização da skill e o agente de QA seriam divididos em alguns PRs. Para simplificar o workshop, todo o fluxo de filtragem e qualidade está nesta conversa e branch e será incluído neste PR.

Nesta lição, você vai:

- conhecer o Agent Merge e como ele automatiza o ciclo de integração.
- inspecionar a alteração completa e as evidências de verificação.
- criar o PR da filtragem.
- habilitar o Agent Merge somente depois da revisão e confirmar a integração.

## Cenário

Você planejou, implementou e verificou a filtragem com o Copilot. Agora a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização de merge sob o controle do desenvolvedor.

## Sobre o Agent Merge

O **Agent Merge** automatiza o trabalho necessário para integrar um pull request. Depois de habilitá-lo, o Copilot resolve impedimentos, como verificações de integração contínua (CI) com falha, comentários de revisão e necessidade de rebase. Em seguida, habilita o merge automático do GitHub quando o repositório permite.

Revise o trabalho e confirme que está pronto antes de habilitar o Agent Merge.

## Usar o Agent Merge para gerenciar o PR

Revise o código, crie o PR e permita que o Agent Merge gerencie o restante do processo.

1. Volte ao Codespace.
2. Abra a caixa de diálogo de agentes digitando `/agent`.
3. Selecione **Default** e pressione <kbd>Enter</kbd>.
4. Crie um PR com `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. Acompanhe as verificações de integração contínua do PR; quando forem aprovadas, o Copilot fará o merge.
7. Confirme que o Copilot informou que o PR foi integrado, por exemplo, com a mensagem "PR #14 was squash-merged successfully."

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de merge, configurações do repositório ou permissões ausentes. Se houver um bloqueio, leia o motivo informado e, quando o repositório permitir, conclua manualmente o merge revisado.

## Resumo e próximos passos

Você:

- conheceu o Agent Merge e como ele automatiza o ciclo de integração.
- inspecionou a alteração completa e as evidências de verificação.
- criou o PR de filtragem.
- habilitou o Agent Merge após a revisão e confirmou a integração.

Em seguida, [explore outros comandos de barra do Copilot CLI][next-lesson] para gerenciar contexto, modelos e compartilhamento e, se quiser, delegar tarefas à nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
