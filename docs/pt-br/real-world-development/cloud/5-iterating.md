---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Durante este workshop, você trabalhou com o GitHub Copilot em tarefas para melhorar a experiência e adicionar funcionalidades. Pediu ao Copilot que documentasse o código, criasse uma área de jogos relacionados para a equipe de design explorar e implementasse recursos de acessibilidade, como modos de alto contraste e claro. Agora, revise as alterações e, se necessário, peça ajustes antes de habilitar a revisão do restante da equipe.

### Cenário

Os fundamentos de desenvolvimento de software e DevOps continuam os mesmos com a IA generativa. Revise o código gerado e siga o processo habitual de DevOps. Analise as sugestões do GitHub Copilot para a documentação, os jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão da equipe.

## Segurança e agente de nuvem do GitHub Copilot

O agente de nuvem trabalha de forma assíncrona e sem supervisão direta, por isso há restrições de segurança:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch usada para o código.
- O agente trabalha no GitHub Actions em um ambiente separado e temporário.
- Uma pessoa precisa aprovar a execução de qualquer workflow do GitHub Actions.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), inclusive a servidores MCP.

## Revisar a documentação gerada

Comece pelo primeiro pull request (PR) criado pelo agente de nuvem, que adiciona documentação ao código.

> [!NOTE]
> O PR pode mostrar um aviso de que o Copilot está bloqueado por um firewall. Isso é esperado, pois o acesso a recursos externos, inclusive servidores MCP, é limitado por padrão. Se quiser, você pode [personalizar ou desativar o firewall do agente de nuvem][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de PRs.
3. Abra o PR com um título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR estará marcado com **[WIP]**. Aguarde a conclusão, que pode levar alguns minutos.

4. Quando o PR estiver pronto, selecione a guia **Files changed** e revise as alterações.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, que pode incluir comentários TSDoc e outra documentação. As alterações exatas variam. Procure comentários TSDoc, convenções de TypeScript e cabeçalhos de arquivo, orientados pelas instruções que você revisou no Exercício 2.
6. Volte à guia **Conversation** e role a página para baixo.
7. Procure um aviso de que há workflows aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows serão adicionados às verificações do PR. Confirme que as verificações do projeto para o aplicativo Astro foram aprovadas. Isso pode levar alguns minutos.

## Pedir alterações ao GitHub Copilot

Você pode mencionar o Copilot em comentários do PR ou comentários em linha no código. O Copilot lerá o comentário e iniciará outra sessão para atender à solicitação. Como os resultados são não determinísticos, não há um prompt único recomendado. Você pode pedir, por exemplo, para:

- adicionar um cabeçalho breve ao início de cada arquivo de código.
- adicionar comentários TSDoc a arquivos TypeScript e Astro.
- criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário marcando **@copilot** como faria com outra pessoa e solicite uma alteração na documentação gerada. Use uma das ideias acima ou outra solicitação.
2. Selecione **View Session** para acompanhar o trabalho do Copilot.
3. Selecione **Back to pull request** para voltar ao PR.

   ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Depois que o Copilot terminar, confirme que há um novo commit no PR.
5. Selecione **Files changed** e revise as alterações.

Continue iterando até que o resultado esteja adequado. Depois, marque o PR como pronto para revisão e faça o merge.

![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar a funcionalidade de jogos relacionados

Revise o PR que o Copilot criou para mostrar jogos relacionados na página de detalhes.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione **Files changed** e revise o código gerado.
5. Volte à guia **Conversation** e role a página para baixo.
6. Procure um aviso de que há workflows aguardando aprovação.
7. Se houver, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Confirme que as verificações do projeto para o aplicativo Astro foram aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** faça um teste manual no Codespace. Abra o terminal e execute os comandos abaixo, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot criou a funcionalidade de jogos relacionados. Você pode pedir alterações pelo comentário na guia **Conversation**, como ajustar o número de jogos exibidos ou adicionar cabeçalhos e comentários TSDoc. Observe que essa issue foi atribuída antes de atualizar as instruções personalizadas.

## Revisar os recursos de acessibilidade

Por fim, revise o PR do agente personalizado de acessibilidade. Ele deve incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR estará marcado com **[WIP]**. Aguarde alguns minutos até que o trabalho termine.

4. Selecione **Files changed** e revise o código gerado.
5. Examine a implementação, principalmente:
   - os controles de interface para alternar os modos.
   - o uso do armazenamento local do navegador para persistir as preferências.
   - as alterações de CSS ou estilo dos modos de alto contraste e claro.
   - os atributos de acessibilidade, como rótulos ARIA e navegação por teclado.
   - o código JavaScript ou TypeScript que controla a alternância.
6. Volte à guia **Conversation** e role a página para baixo.
7. Procure um aviso de que há workflows aguardando aprovação.
8. Se houver, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Confirme que as verificações do projeto para o aplicativo Astro foram aprovadas.
10. **Opcional:** teste manualmente os recursos de acessibilidade no Codespace. Abra o terminal e execute, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Inicie o aplicativo e teste os controles dos modos de alto contraste e claro no navegador. Confirme que funcionam e que as preferências persistem depois de recarregar a página.

O agente personalizado de acessibilidade orientou o Copilot a seguir práticas de acessibilidade. Se encontrar algo a melhorar, marque **@copilot** em um comentário e peça a alteração, como fez nos outros PRs.

## Exercício opcional: continue delegando

O agente de nuvem é útil para itens reais do backlog que você pode delegar enquanto trabalha em outras tarefas. Crie mais issues no repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- implementar paginação na lista de jogos.
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Você:

- **inspecionou os arquivos de instruções do repositório** para reconhecer sua influência nas respostas do agente.
- **atribuiu issues ao agente de nuvem do Copilot** e acompanhou a preparação do ambiente, o planejamento e a execução assíncrona.
- **criou e usou um agente personalizado** para acessibilidade, incluindo controles para os modos de alto contraste e claro.
- **usou a página de agentes do Copilot como centro de controle** para acompanhar e orientar a sessão de acessibilidade.
- **revisou e ajustou os pull requests do agente de nuvem**, mencionando `@copilot` para solicitar mudanças e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Para continuar, explore outros ambientes:

- **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP no IDE.
- **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills, um agente de QA, Playwright MCP, Agent Merge e a integração opcional `/delegate` com o agente de nuvem.

No seu repositório, experimente também:

- atribuir ao agente de nuvem uma tarefa de refatoração ou cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
