---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

许多组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot cloud agent 可以自主更新代码和添加功能。任务完成后，它会创建草稿拉取请求，供开发者审查。这样可以把繁琐任务交给智能体处理，加快开发进度，让开发者专注于更重要的工作。

本课将探索如何使用 Copilot cloud agent：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初负责创建网站的承包团队没有为代码编写文档。团队希望首先为应用中所有导出的函数添加 TypeScript 文档注释 (TSDoc)。

此外，设计团队希望改进游戏发现体验：在每款游戏的详情页显示同一类别中的其他游戏，方便用户继续浏览。他们暂时不需要完善的实现，只需要一个可用于用户体验验收测试的版本。这项工作目前受阻，不过待办事项中还有优先级更高的议题。

这两项任务都可能被推迟，很适合交给 Copilot cloud agent 异步处理。开发者可以继续处理其他工作，之后再回来审查 Copilot 的成果。

## 认识 GitHub Copilot cloud agent

[GitHub Copilot cloud agent][copilot-agents] 可以像人类开发者一样在后台执行任务。你可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot][assign-issue]。分配后，Copilot 会创建用于跟踪进度的草稿拉取请求、设置环境并开始处理任务。你可以在会话进行中或完成后查看详情。解决方案准备好供你审查时，Copilot 会在拉取请求中提及你。

## 明确指令和任务范围的重要性

GitHub Copilot 并非魔法工具，使用它仍然需要思考。即使看似简单的操作，也可能包含复杂环节，因此要谨慎确定 Copilot cloud agent 的任务范围。把它当作 AI 结对程序员：分阶段工作、学习、试验并不断调整。加入生成式 AI 不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果已完成自定义指令的练习，就会看到 `.github/copilot-instructions.md` 的效果；如果还没有，现在可以快速查看。将任务分配给 Copilot cloud agent 前，先只读查看存储库中已有的指令文件，稍后便能发现它们的影响。

在 GitHub Web UI 中或已运行的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：检查 **Code standards** 部分，尤其留意 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

下一部分将 `Code lacks documentation` 议题分配给 cloud agent 后，检查其拉取请求中是否包含 TypeScript 文档注释 (TSDoc)、TypeScript 约定和文件注释头。这些内容来自上述指令文件。后续练习会再次介绍如何审查拉取请求 (PR)。

## 设置 Copilot cloud agent 的开发环境

无论由谁编写代码，通常都需要特定环境和设置脚本，确保一切就绪。将任务分配给 Copilot 时也一样，它会像软件工程师一样完成任务。

Cloud agent 使用 [GitHub Actions][github-actions] 环境来执行任务。你可以创建专用的[设置工作流][setup-workflow]，并将其配置在 `.github/workflows/copilot-setup-steps.yml` 中，在 Copilot 开始工作前安装开发工具和依赖项。实验已预先配置此工作流，以确保流程顺利并提供学习机会。它会为单体 Astro 应用准备 Node.js、项目依赖项、端到端测试所需的浏览器，以及已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的 "Actions" 选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为 *你的步骤* 设置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有数据可用。
      - name: Set up the database
        run: npm run db:setup
```

这与其他 GitHub 工作流文件类似，但有几点需要注意：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器允许你在存储库的 **Actions** 选项卡中手动运行工作流。这有助于先验证工作流，而不必等待 Copilot 启动它。

## 添加文档

虽然大家都知道文档的重要性，但许多项目中的文档要么已经过时，要么完全缺失。这类技术债务往往长期得不到处理，降低开发效率，也增加维护代码库或帮助新成员上手的难度。Copilot 擅长生成文档，因此很适合把这项工作分配给 Copilot cloud agent。它会在后台生成文档，你将在后续练习中审查结果。

1. 在新浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 在 **Title** 中输入 `Code lacks documentation`。
6. 在 **Description** 中输入：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求 (PR)，标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中暂时没有新 PR，请等待一两分钟并刷新浏览器。
12. 几分钟后，Copilot 应会在 PR 中创建待办列表。

> [!NOTE]
> Copilot 的待办列表可能需要几分钟才会显示。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定处理任务的方案。

13. 查看待办列表及其计划完成的任务。
14. 向下浏览 PR 时间线，确认 Copilot 已开始处理议题。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口，才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何探索代码、了解当前状态、制定方案并编写代码。

此过程可能需要几分钟。Copilot cloud agent 的主要优势之一是可以异步执行任务，让你去处理其他工作。现在利用这一点，再分配一项任务，然后转而为应用编写新功能。

## 在游戏详情页添加相关游戏部分

GitHub Copilot cloud agent 的一大优势是可以拆分工作：你处理一项任务，Copilot 同时处理另一项任务。即使设计团队需要的相关游戏部分并不复杂，也可以把时间留给其他工作。现在将此任务分配给 Copilot cloud agent。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择空白模板 **Blank issue**。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，你会在议题的第一条评论旁看到 👀，表示 Copilot 已开始处理。

![Copilot 用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求！Copilot cloud agent 的工作方式类似软件工程师，无需主动持续监视，完成后再审查即可。现在开始创建和使用自定义智能体。

## 总结与后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。你可以将议题分配给 cloud agent，让它异步处理技术债务、新功能，或将代码迁移到其他框架。

本课探索了以下内容：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

Copilot cloud agent 正在后台工作，现在可以开始创建和使用自定义智能体。[Copilot cloud agent 也可以使用 MCP 服务器][cloud-agent-mcp]，并且可以读取前面模块介绍的自定义指令。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
