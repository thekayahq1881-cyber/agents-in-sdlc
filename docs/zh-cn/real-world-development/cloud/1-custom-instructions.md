---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能访问这些信息。[说明文件][instruction-files]可以提供指导，让 Copilot 不仅了解要做什么，也了解应如何完成。

本练习将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明及路径范围说明文件传递给 Copilot；
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 练习路径不同，这里不会运行“前后对比”提示词。Copilot 云智能体会在 GitHub 议题上异步工作，因此难以实时并排展示其影响。后续练习中审查云智能体创建的拉取请求时，你会看到说明文件发挥的作用。

## 说明文件

### 场景

Tailspin Toys 和许多开发团队一样，有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加一段注释，说明该文件的用途。

通过使用说明文件，可以确保 Copilot 了解这些实践要求，并据此完成任务。

### 自定义说明

自定义说明可为 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。这项功能十分强大，可引导 Copilot 提供更贴合需求的建议和代码片段。你可以指定偏好的编码约定、库，甚至希望采用的注释方式。说明既可以适用于整个存储库，也可以针对特定文件类型。

说明文件有两种类型：

- `.github/copilot-instructions.md` 是一个单独的说明文件，Copilot 会在处理该存储库的**每个**请求时读取。它应包含项目级信息，即 Copilot 处理大多数聊天或 CLI 请求时都需要了解的上下文，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 文件可针对特定任务或文件类型创建。例如，可以为 TypeScript 或 Astro 编写说明，也可以为创建 UI 组件或单元测试等任务编写说明。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级说明文件和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot [还支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

完整讲解如何创建说明文件超出了本研讨会的范围，不过示例项目展示了具有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 限定为项目级指导，例如项目简介、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（例如单元测试、Astro 组件或数据层）或特定任务提供说明。
- 使用自然语言，确保指导清晰，并提供代码示例说明正确和错误的写法。

说明文件没有唯一的正确写法，正如 AI 也没有唯一的使用方式。通过试验，你会逐渐找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应配备完善的说明文件。浏览本项目中的文件时，你可能会发现有些文件专门针对多种任务，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同产品界面的操作方式有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应界面的课程会在相关处介绍这些功能。
>
> 需要模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义说明文件

花些时间阅读此存储库附带的说明文件：其中有一个核心文件 `copilot-instructions.md`，还有一组用于不同任务的 `*.instructions.md` 文件。请在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。它们适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并查看其中内容。你会看到适用于 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它设置相对于存储库根目录的 glob，用于指定适用文件。在此处，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会应用这些说明。
5. 留意专门用于在此项目中创建单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分成更小、可复用的文件，并为 Copilot 提供代码示例。文件中的路径相对于说明文件所在位置，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但尚未要求提供代码内文档。接下来，你将添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来要做的唯一编辑，是添加一条项目范围的规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。稍后练习中，云智能体处理你分配的议题时会遵循这条规则。

编辑前，先创建一个分支（云智能体会读取议题目标分支中的说明文件，但你仍应单独提交这些更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来更新其中记录的存储库编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，它应该位于第 27 行附近。注意该部分介绍了项目编码标准，但还没有关于代码内文档的规则，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 正如上一课所示，可以在存储库级文件（`.github/copilot-instructions.md`）中添加全局指导，也可以在 `*.instructions.md` 文件中添加特定语言、文件类型或任务的说明。刚才添加的文档注释规则属于项目级标准，因此适合放在存储库级文件中。

## 提交、推送并合并说明更新

云智能体会读取议题目标分支上的说明文件。下一课将把议题分配给 Copilot，目标分支是 `main`，因此必须先将这些说明更改合并到 `main`，云智能体才能读取。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并将其合并。下一课中，云智能体处理议题时就会读取这些说明。

> [!TIP]
> 如果希望直接在 `main` 上完成本研讨会，可以跳过创建分支的步骤并直接提交到 `main`。此处保留分支步骤，是为了让研讨会流程更接近真实项目的工作方式。

## 总结与后续步骤

你了解了 Copilot 如何读取本项目的说明文件，并向 `.github/copilot-instructions.md` 添加了一条存储库级标准。在接下来的练习中，你可以从云智能体生成的拉取请求中看到这条标准的实际效果。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
