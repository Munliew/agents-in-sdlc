---
title: "Lição 6 - Validar a funcionalidade com o MCP do Playwright"
description: "Configure ou inspecione o MCP do Playwright no Copilot CLI e explore a filtragem em um navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Desenvolver também envolve trabalhar com dados, serviços externos e automações adicionais para o Copilot. Os servidores do Model Context Protocol (MCP) fornecem ferramentas e serviços que vão além dos recursos integrados à CLI.

Nesta lição, você vai:

- entender o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa.
- adicionar o servidor MCP do Playwright, se ainda não estiver disponível.
- pedir ao agente que controle um navegador e explore a filtragem.

## Cenário

Testes unitários e de ponta a ponta são importantes, mas verificar atualizações da interface também exige interagir com ela. Com o Playwright MCP, o Copilot pode usar o site como uma pessoa usuária e explorar o comportamento das alterações.

## O que é o Model Context Protocol?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem em tempo real com ferramentas e serviços externos. Um servidor MCP funciona como ponte entre o agente e as ferramentas ou serviços; cada servidor oferece um conjunto próprio de recursos.

- O **[servidor MCP do GitHub][github-mcp]** fornece acesso a APIs para gerenciar repositórios, issues e pull requests.
- O **[servidor MCP do Playwright][playwright-mcp-server]** oferece recursos de automação de navegador.

O GitHub mantém um [registro MCP][mcp-registry] para facilitar a descoberta de servidores.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência. Antes de usar um, revise o código-fonte, verifique o publicador e considere as implicações de segurança.

## Adicionar o servidor MCP do Playwright

Se o servidor ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site.

> [!NOTE]
> Se o MCP do Playwright já estiver configurado no Copilot CLI, ignore estas etapas e continue na próxima seção.

1. Volte ao Codespace.
2. No Copilot CLI, digite:

   ```plaintext
   /mcp add
   ```

3. Informe `playwright` como nome e pressione <kbd>Tab</kbd>.
4. Confirme o tipo de servidor **STDIO** pressionando <kbd>Enter</kbd> e, em seguida, pressione <kbd>Tab</kbd>.
5. Na caixa de diálogo **Command**, cole:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o servidor.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo MCP.

## Pedir ao Copilot para explorar a funcionalidade

Use o servidor MCP do Playwright para repetir a verificação manual da funcionalidade.

1. Peça ao Copilot para validar a funcionalidade:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

2. Acompanhe as etapas executadas no navegador.
3. Leia o relatório e confirme que o comportamento está correto.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, encerrará o servidor e apresentará um relatório.

## Resumo e próximos passos

Você:

- entendeu o que é o MCP e como o Copilot CLI o usa.
- adicionou o servidor MCP do Playwright, caso ainda não estivesse disponível.
- pediu ao agente que controlasse um navegador e explorasse a filtragem.

Em seguida, [crie um agente personalizado de QA][next-lesson] que reúna a skill e as ferramentas do navegador.

## Recursos

- [O que é MCP e por que todos estão falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
