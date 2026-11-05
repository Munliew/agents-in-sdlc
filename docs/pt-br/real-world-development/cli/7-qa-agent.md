---
title: "Lição 7 - Criar e usar um agente de QA"
description: "Crie um agente personalizado de QA que combine requisitos da issue, a skill quality-checks e o MCP do Playwright."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você usou a skill `quality-checks` para executar verificações automatizadas e o MCP do Playwright para observar a filtragem no navegador. Agora, reúna esses recursos em um agente personalizado com um processo de QA definido.

Nesta lição, você vai:

- explorar como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criar e inspecionar um perfil reutilizável de garantia de qualidade.
- selecionar o agente de QA e revisar as conclusões com base na issue de filtragem.

## Cenário

Antes de abrir um pull request (PR), a Tailspin Toys quer uma revisão consistente dos requisitos, da qualidade do código, das verificações automatizadas, da cobertura de testes e do comportamento no navegador. Um agente personalizado pode coordenar esse processo e gerar um relatório reutilizável.

## O que é um agente personalizado?

Um agente personalizado é uma versão especializada do Copilot, definida em um perfil Markdown que descreve sua finalidade, suas instruções e as ferramentas disponíveis. Neste workshop, o perfil de QA ficará em `.github/agents/qa.agent.md` e será selecionado no Copilot CLI.

Cada personalização tem uma função: as instruções do repositório descrevem os padrões da equipe; a skill `quality-checks` reúne verificações repetíveis; o MCP do Playwright fornece ferramentas de navegador; e o perfil de QA orienta o Copilot a avaliar os requisitos e relatar as conclusões. O perfil não substitui esses recursos nem exige outra conversa.

## Criar o perfil de QA

Peça ao Copilot para criar um perfil reutilizável que defina as verificações e os limites do trabalho de QA.

1. Volte ao Codespace e confirme que a conversa de filtragem está no modo Interactive.
2. Peça ao agente padrão para criar o agente:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspecionar o perfil

Revise o perfil antes de usar o agente para confirmar que ele seguirá o fluxo e os limites definidos.

1. Digite `/diff` e abra `.github/agents/qa.agent.md`.
2. Leia o frontmatter. `description` é obrigatório; `name` é opcional, mas dá ao agente um nome de exibição claro.
3. Confirme que o perfil começa pelos requisitos, segue as instruções do repositório, executa a skill `quality-checks` e usa o MCP do Playwright.
4. Confirme que o agente apresenta evidências, pede autorização antes de alterar o código de implementação e não faz commits nem abre PRs.
5. Se o perfil não incluir alguma responsabilidade ou limite, peça ao agente padrão para revisá-lo.

## Executar o QA para a issue

O Copilot CLI carrega os agentes do projeto ao iniciar uma conversa. Retome a conversa de filtragem e selecione o agente de QA para que ele use o contexto da issue e do plano.

1. Ative o agente com:

   ```plaintext
   /agent QA
   ```

2. Peça a revisão:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. Leia o relatório depois que o agente terminar.

> [!NOTE]
> Como o agente acabou de ser criado, talvez não apareça na lista de agentes disponíveis. O comando acima ainda o ativa.

## Resumo e próximos passos

Você:

- explorou como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criou e inspecionou um perfil reutilizável de garantia de qualidade.
- selecionou o agente de QA e revisou suas conclusões com base na issue.

Agora a implementação, a atualização da skill, o perfil de QA, os testes e o relatório de verificação estão prontos para revisão. Em seguida, [reúna-os no PR da funcionalidade e use o Agent Merge][next-lesson].

## Recursos

- [Criar agentes personalizados para o Copilot CLI][create-agents]
- [Configuração de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
