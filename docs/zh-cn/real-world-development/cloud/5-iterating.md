---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你与 GitHub Copilot 一起完成了多项旨在改善用户体验和添加功能的任务：请 Copilot 为代码添加文档、为设计团队构建相关游戏功能，并使用无障碍自定义智能体实现高对比度和浅色模式切换。现在来检查代码更改，必要时向 Copilot 提供反馈以改进结果。

### 场景

加入生成式人工智能 (AI) 并不会改变软件设计和开发运维 (DevOps) 的基本原则。我们始终需要审查生成的代码，并遵循常规 DevOps 流程。现在先审查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能提出的更改，然后再邀请团队其他成员参与审查。

## 安全与 GitHub Copilot cloud agent

Copilot cloud agent 会异步且无人监督地执行任务，因此设置了一些安全限制：

- Copilot 只能读取存储库，并且**只能**写入用于保存其代码的分支。
- Cloud agent 在 GitHub Actions 中运行，并创建独立、临时的工作环境。
- 任何 GitHub Actions 工作流都必须由人工批准后才能运行。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括模型上下文协议 (MCP) 服务器。

## 审查生成的文档

先查看 GitHub Copilot cloud agent 创建的第一个拉取请求 (PR)，其中包含代码文档。你将使用 GitHub.com 的标准 PR 界面。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。你可以[自定义或禁用 Copilot cloud agent 防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 打开 PR 列表。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题中会包含 **[WIP]**。此时请等待 Copilot 完成任务，可能需要几分钟。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 浏览更新后的代码，其中应包含新创建的 TypeScript 文档注释 (TSDoc) 和其他文档。具体更改可能不同。

   检查 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自你在练习 2 开始时查看的自定义指令文件。

6. 审查完毕且确认更改无误后，返回 **Conversation** 选项卡并向下滚动。
7. 检查是否有工作流等待批准的提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会出现在 PR 的检查区域中。如果一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 更改

在拉取请求中与 Copilot 协作并非单向流程。你可以像提及团队成员一样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会看到这些评论，并启动新的会话来处理请求。由于结果具有非确定性，无法指定固定文本。可以考虑要求 Copilot：

- 在每个代码文件顶部添加简短说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 在评论中请求 Copilot 修改生成的文档，并像提及其他用户一样标记 **@copilot**。可以使用上面的建议，也可以提出其他文档改进请求。
2. 选择 **View Session**，观察 Copilot 如何启动新会话并执行更新。
3. 选择 **Back to pull request** 返回 PR。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中应出现新的提交。
5. 选择 **Files changed** 选项卡并审查更改。

你可以持续迭代，直到满意为止。完成后，可以将 PR 从草稿转换为准备审查状态，然后合并到 main 分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在返回 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查完毕且确认更改无误后，返回 **Conversation** 选项卡并向下滚动。
6. 检查是否有工作流等待批准的提示。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流会出现在 PR 的检查区域中。如果一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：**可以在 Codespace 中切换到该分支，手动测试相关游戏功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已构建相关游戏功能！与之前一样，你可以继续请求 Copilot cloud agent 更新。例如，可以要求调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住，分配此任务时尚未更新自定义指令）。与之前一样，可以在 **Conversation** 选项卡中添加评论，Copilot 会看到评论并启动新会话。

## 审查无障碍功能

最后来审查使用无障碍自定义智能体实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中在任务控制中心提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题中会包含 **[WIP]**。此时请等待 Copilot 完成，可能需要几分钟。

4. 选择 **Files changed** 选项卡并审查生成的代码。
5. 检查实现，重点关注：
   - 切换显示模式的用户界面 (UI) 组件。
   - 使用本地存储保存用户偏好。
   - 高对比度和浅色模式的层叠样式表 (CSS) 或样式更改。
   - 无障碍属性（无障碍富互联网应用程序 (ARIA) 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 审查完毕且确认更改无误后，返回 **Conversation** 选项卡并向下滚动。
7. 检查是否有工作流等待批准的提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会出现在 PR 的检查区域中。如果一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到该分支，手动测试无障碍功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   然后启动应用，在浏览器中测试高对比度和浅色模式切换，并确认重新加载页面后设置仍然有效。

注意，无障碍自定义智能体帮助 Copilot 根据无障碍最佳实践实现功能。如果发现无障碍问题或改进点，可以像处理前面几个 PR 一样，在评论中标记 **@copilot** 请求更新。

## 可选练习：继续委派任务

当你可以把真实待办事项交给 cloud agent，并转而处理其他工作时，它最能发挥作用。可以在自己的存储库中创建更多议题并分配给 Copilot，逐渐养成这种工作习惯。例如：

- 在游戏详情页创建支持者意向表单。
- 为游戏列表页实现分页。
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 开发环境课程。本系列练习中，你：

- **检查了存储库附带的自定义指令文件**，以便稍后查看这些指令对 cloud agent 输出的影响。
- **将议题分配给 Copilot cloud agent**，并观察它异步设置环境、制定计划和执行任务。
- **创建并使用了无障碍自定义智能体**，添加高对比度和浅色模式切换。
- **将 Copilot agents 页面用作任务控制中心**，监视并引导无障碍会话。
- **审查并迭代改进了 cloud agent 的拉取请求**，通过标记 `@copilot` 请求更改并批准工作流。

## 回顾与后续步骤

Cloud agent 开发环境课程已完成。其他开发环境课程可以帮助你通过不同界面探索相关场景：

- 🖥️ **[VS Code 开发环境课程](../../vscode/)**：直接在 IDE 中探索 Copilot Chat agent mode 和 MCP 集成。
- 💻 **[CLI 开发环境课程](../../cli/)**：在终端中交付两项经过审查的更改，使用 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` cloud agent 桥接。

在自己的存储库中，还可以尝试以下后续任务：

- 将重构或测试覆盖率议题分配给 cloud agent。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
