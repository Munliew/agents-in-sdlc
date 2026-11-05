---
title: "练习 4 - 监视和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你让 Copilot cloud agent 处理了三个旨在改善用户体验和添加功能的任务。Cloud agent 虽然可以异步、自主地工作，但仍然需要监视任务进度。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，你可以查看所有已创建开放拉取请求 (PR) 的智能体任务，检查智能体执行的操作，也可以引导正在进行的会话。

在本课中，你将：

- 浏览智能体页面，监视 cloud agent 的任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到也应该添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，顺便实现浅色模式很合理。现在引导智能体在添加高对比度模式的同时也实现浅色模式。

## 查看 Copilot cloud agent 任务

先查看分配给 Copilot cloud agent 的任务状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主窗格和左窗格中的任务列表。列表中应包含你分配给 Copilot 的任务，例如：
   - 更新代码库文档。
   - 生成用于修改游戏的应用程序编程接口 (API)。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，检查 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 为 Copilot 创建工作环境。
   - 设置模型上下文协议 (MCP) 服务器。
   - 执行完成分配任务所需的其他步骤。

> [!NOTE]
> 显示的具体步骤会因 Copilot 当前的工作状态和处理方式而异。

4. 留意右侧的拉取请求窗格，可以在此查看 PR 和已更改的文件，以便进一步监视。

## 引导 cloud agent

现在已经查看正在执行的任务，可以请求 Copilot 在实现高对比度模式时也添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。会话标题取决于 Copilot 使用的名称和当前工作状态。

   ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示设置已完成并开始执行任务。看到类似下方的消息时，就表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Copilot agents 页面中的 cloud agent 任务截图，其中突出显示了 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示，并将其纳入工作流程。

## 让 Copilot 继续工作

Copilot 会继续处理更新后的任务！它会先完成发送消息时正在执行的步骤，然后将新请求纳入工作流程。

与之前一样，这可能需要几分钟。此时可以暂停一下，回顾迄今为止学到和探索的内容。

## 总结与后续步骤

本课探索了 Copilot agents 页面，它是监视和引导 GitHub Copilot cloud agent 任务的中心位置。你可以通过这个任务控制中心跟踪正在进行和已完成的任务、审查工作，也可以重定向正在运行的任务以调整范围或补充指导。

本课探索了以下内容：

- 使用任务控制中心和智能体页面监视 cloud agent 任务。
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能，接下来开始[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
