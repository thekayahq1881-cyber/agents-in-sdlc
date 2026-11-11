---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "浏览存储库说明、添加文档标准，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，就要确保 Copilot 能访问相关指导。[说明文件][instruction-files]不仅描述你希望生成什么代码，也说明代码应如何组织。现在你已构建筛选功能，接下来将查看 Copilot 使用的说明，添加一条文档标准，并将其应用于代码。

本课将：

- 了解存储库说明和路径范围说明文件如何传递给智能体；
- 更新说明文件，确保遵循编码标准；
- 了解说明文件对代码的影响。

## 场景

Tailspin Toys 和许多开发团队一样，有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明其用途、参数和返回值（包括其中可注入的 `db` 参数）。
- 可复用的 Astro 组件应说明其 `Props` 接口；相关代码更改时，注释也应保持更新。
- 应保留现有格式设置和 lint 指南。

通过使用说明文件，可以确保 Copilot 了解这些实践要求，并据此完成任务。

## 说明文件

自定义说明可为 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。它可以引导 Copilot 提供更贴合需求的建议和代码片段，也可以指定偏好的编码约定、库以及注释风格。说明既可以适用于整个存储库，也可以针对特定文件类型。

说明文件有两种类型：

- `.github/copilot-instructions.md` 是一个单独的说明文件，Copilot 会在处理该存储库的**每个**请求时读取。它应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md` 文件可针对特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他说明格式及其支持情况会因环境而异。依赖特定格式前，请查看[自定义说明支持参考][custom-instructions-support]。

## 浏览本项目中的自定义说明文件

入门项目中已经包含一组说明文件。先浏览现有文件，再进行更改，以便了解其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，留意项目简介和编码指导。Copilot 在此存储库中的每次交互都会应用这些说明。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到适用于 Astro 文件、Drizzle 数据层、测试等内容的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它设置一个 glob，用于指定适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，留意其中指向其他说明文件和项目现有文件的链接。这样可以将较大的说明集拆分成更小、可复用的文件，并为 Copilot 提供示例。

## 更新说明文件以符合团队指导

现有文件已经提供了良好的起点，但仍有不足。接下来修改核心文件 `copilot-instructions.md`，要求为新生成的 TypeScript 添加 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，该部分应位于第 35 行附近。
2. 在该部分末尾添加以下项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指导

Copilot CLI 会在对话启动时加载存储库说明。保存更新后的说明后，退出并恢复筛选功能对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选功能对话仍在运行，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选功能对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，检查更改的 TypeScript 文件。留意新增的 TSDoc 注释，并确认其准确说明了代码。

## 总结与后续步骤

你了解了 Copilot 如何读取说明文件，并将新标准应用于功能。本课中，你：

- 了解了存储库说明和路径范围说明文件如何传递给智能体；
- 更新了说明文件，确保遵循编码标准；
- 了解了说明文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保以一致的方式运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
