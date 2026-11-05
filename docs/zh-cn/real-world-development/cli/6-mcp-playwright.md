---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并通过浏览器探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

前面提到过，开发工作不只是编写代码。我们还要处理数据、外部服务，并让 Copilot 使用更多自动化工具。模型上下文协议 (MCP) 服务器可以扩展 Copilot CLI 的内置功能，提供更多工具和服务。

在本课中，你将：

- 了解什么是 Model Context Protocol (MCP)，以及 Copilot CLI 如何使用它。
- 如果尚不可用，则添加 Playwright MCP 服务器。
- 请智能体驱动浏览器并探索筛选功能。

## 场景

单元测试和端到端测试很重要，但要验证界面更新，还需要实际与界面交互。你希望 Copilot 能像用户一样使用正在开发的网站，从而进一步自动化更改流程，并增强对功能行为的信心。

## 什么是 Model Context Protocol (MCP)？

[模型上下文协议 (MCP)][mcp-blog-post] 为人工智能 (AI) 智能体与外部工具和服务通信提供了一种方式。借助 MCP，AI 智能体可以实时访问外部工具和服务，获取最新信息并代表你执行操作。

智能体通过 MCP 服务器访问这些工具和资源。每个 MCP 服务器都代表 AI 智能体可以访问的一组工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的应用程序编程接口 (API)。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 提供了一个 [MCP 注册表][mcp-registry]，方便发现和参与 MCP 生态系统。

> [!CAUTION]
> 对待 MCP 服务器应与对待项目中的其他依赖项一样。使用前请审查其源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 尚未配置 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过这些设置步骤，直接转到下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux)，保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 请 Copilot 通过 Playwright 探索功能

之前你已手动确认功能行为正常。现在使用 Copilot 中的 Playwright MCP 服务器完成同样的检查。

1. 使用以下提示，请 Copilot 使用 Playwright MCP 验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常 Copilot 会自行判断应使用 MCP 服务器，无需明确要求。不过，指出要使用的服务器有助于获得更一致的结果，也能节省一些令牌。

2. 观察 Copilot 在浏览器中执行的步骤，确认功能是否正常。
3. 阅读报告，确认所有行为符合预期。

Copilot 会启动服务器、使用 Playwright 与网站交互、停止服务器，然后提供报告。

## 总结与后续步骤

恭喜！你已在 Copilot CLI 中使用 Playwright MCP 服务器，通过真实浏览器探索功能。本课中，你：

- 了解了 Model Context Protocol (MCP) 以及 Copilot CLI 如何使用它。
- 如果尚不可用，则添加了 Playwright MCP 服务器。
- 请智能体驱动浏览器并探索筛选功能。

接下来创建一个[质量保证 (QA) 自定义智能体][next-lesson]，将技能和浏览器工具整合到专业角色中。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
