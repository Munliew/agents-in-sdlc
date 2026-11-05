---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "探索存储库指令、添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式人工智能 (AI) 时，上下文至关重要。如果任务需要采用特定方式完成，就要让 Copilot 获得相应指导。[指令文件][instruction-files]不仅说明你想要什么代码，也说明代码应如何组织。现在你已经构建了筛选功能，接下来将探索 Copilot 使用的指令、添加文档标准，并将其应用到代码中。

在本课中，你将：

- 探索存储库指令和按路径限定范围的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 观察指令文件对代码的影响。

## 场景

Tailspin Toys 团队制定了一套开发实践准则和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TypeScript 文档注释 (TSDoc) 或 JavaScript 文档注释 (JSDoc) 说明用途、参数和返回值；如果有可注入的 `db` 参数，也要说明。
- 可复用的 Astro 组件应说明其 `Props` 契约；相关代码更改时，注释也应保持同步。
- 保留现有格式和 lint 指南。

通过使用指令文件，可以让 Copilot 获得正确的信息，并按照这些实践要求完成任务。

## 指令文件

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和需求。你可以指定编码约定、库以及注释风格。既可以为整个存储库创建指令，也可以为特定文件类型创建指令，以提供任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md` 是一个存储库级指令文件，会随该存储库的**每个**请求发送给 Copilot。文件应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md` 为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他指令格式及其支持情况因开发环境而异。依赖某种格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

## 探索项目中的自定义指令文件

入门项目已包含一组指令文件。先探索现有文件，再修改内容，以便观察变化。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，留意项目简介和编码指导。此文件适用于该存储库中与 Copilot 的所有交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它通过 glob 指定指令适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，留意其中指向其他指令文件和项目现有文件的链接。这样可以将较大的指令集拆分成较小且可复用的文件，并提供项目示例供 Copilot 参考。

## 更新指令文件以符合团队指南

现有文件已经打下良好基础，但还缺少一项要求。现在修改核心 `copilot-instructions.md`，确保新生成的 TypeScript 遵循文档标准。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分（大约在第 35 行）。
2. 将以下内容添加为该部分的最后一项：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指南

Copilot CLI 会在对话启动时加载存储库指令。保存更改后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指南。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在存储库根目录、`game-filters-cli` 分支中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，选择筛选对话，并确认检出的是 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指南：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看已更改的 TypeScript 文件。留意新增的 TSDoc 注释，并确认其准确说明了代码。

## 总结与后续步骤

你已了解 Copilot 如何选取指令文件中的上下文，并将新的标准应用于功能。本课中，你：

- 探索了存储库指令和按路径限定范围的指令文件如何传递给智能体。
- 更新了指令文件，确保遵循编码标准。
- 观察了指令文件对代码的影响。

接下来[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保测试和 lint 检查能够一致地运行。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
