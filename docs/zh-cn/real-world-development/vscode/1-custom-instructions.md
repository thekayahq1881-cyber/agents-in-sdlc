---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式人工智能 (AI) 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能访问这些信息。可以通过多种方式向 Copilot 提供特定上下文，其中一种重要方式是使用[说明文件][instruction-files]，为代码生成提供指导。

本练习将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明及路径范围说明文件传递给 Copilot；
- 在当前说明文件生效时生成用于筛选功能的第一部分数据层代码（出版社辅助函数）；
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准；
- 重新运行相同提示词，观察生成的代码如何采用新标准；
- 将说明更新和筛选功能的数据层基础提交到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不完全符合你设置的标准。Copilot 的输出具有非确定性。本练习的重点是观察更新说明后行为的*趋势变化*，而不是要求输出逐字一致。

## 说明文件

### 场景

和任何优秀的开发团队一样，Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- 用户界面 (UI) 应采用深色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加一段注释，说明该文件的用途。

通过使用说明文件，可以确保 Copilot 了解这些实践要求，并据此完成任务。

### 自定义说明

自定义说明可为 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。这项功能十分强大，可引导 Copilot 提供更贴合需求的建议和代码片段。你可以指定偏好的编码约定、库，甚至希望采用的注释方式。说明既可以适用于整个存储库，也可以针对特定类型的文件。

说明文件有两种类型：

- `.github/copilot-instructions.md` 是一个单独的说明文件，Copilot 会在处理该存储库的**每个**请求时读取。它应包含项目级信息，即 Copilot 处理大多数聊天或命令行界面 (CLI) 请求时都需要了解的上下文，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 文件可针对特定任务或文件类型创建。例如，可以为 TypeScript 或 Astro 编写说明，也可以为创建 UI 组件或单元测试等任务编写说明。

> [!NOTE]
> 在集成开发环境 (IDE) 中，说明文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
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
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在与 Copilot 进行交互时，这些内容都适用。
3. 打开 `.github/instructions` 文件夹并查看其中内容。你会看到适用于 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它设置相对于存储库根目录的 glob，用于指定适用文件。在此处，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会应用这些说明。
5. 留意专门用于在此项目中创建单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分成更小、可复用的文件，并为 Copilot 提供代码示例。文件中的路径相对于说明文件所在位置，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但尚未要求提供代码内文档。接下来，你将添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

接下来按照最佳实践，为本次更改创建分支。

1. 返回上一练习中使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在终端中打开新窗口。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，以启动一个干净的对话。不同练习所需的模式和模型可能不同；每个步骤会在相关处说明。

要查看自定义说明的影响，先在当前说明文件生效时发送提示词。稍后更新说明后，再发送相同提示词进行比较。

1. 关闭之前练习中打开的编辑器标签页，让 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应该放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 应用文件更改。

   ![显示 Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示词：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 留意生成的辅助函数是一个带类型的函数，它以 `db` 客户端作为第一个参数，并返回带类型的出版社数组；这来自 `.github/instructions/drizzle.instructions.md` 中的数据层约定（该文件适用于 `src/lib/*.ts`）。
7. 留意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> 由于 Copilot 的输出具有概率性，它有可能在没有明确要求的情况下添加文档注释。如果出现这种情况也没关系；本练习关注的是更新说明后行为是否更一致。

## 添加新的存储库标准

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

## 重新运行提示词并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清除对话内容。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送与之前**相同的提示词**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意生成的文件现在是否以类似以下内容的注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的出版社数据访问辅助函数。
    * 提供从数据库检索出版社信息的函数。
    */
   ```

5. 留意生成的函数现在是否包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * 返回包含所有出版社 ID 和名称的列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 一个 Promise，解析为出版社对象数组。
    */
   ```

你刚才引导 Copilot 遵循新的项目标准，并将其应用到真实代码中；下一练习会在此基础上继续开展工作。

## 提交说明并推送分支

说明文件与其他项目文件一样，都应通过版本控制进行管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），打开新的终端窗口。
2. 在终端中运行以下命令，确认说明更新和辅助函数更改都已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交说明更新，以及 Copilot 为辅助函数基础所做的每项更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他相关更新（例如测试），也请添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送完成后，应创建拉取请求 (PR)，并关联待办事项中的文档标准议题。你可以手动操作，也可以使用已连接到项目的 GitHub 工具，让 Copilot 代为处理。向 Copilot 提出请求，让它查找议题、创建用于关闭该议题的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已为工作区连接 GitHub 工具，因此无需额外设置。Copilot 首次使用其中某个工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。后续练习会介绍此连接的工作方式——模型上下文协议 (Model Context Protocol，MCP)。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认在智能体选择器中选择了 **Agent**。
3. 请 Copilot 查找与更新说明文件有关的议题，并从当前分支创建 PR，同时说明 PR 还包含即将用于筛选工作的出版社辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 开始查找议题并创建 PR。
5. 如果系统提示 **Allow** Copilot 代表你执行 GitHub 操作，请审查命令并酌情选择 **Allow**。
6. 创建 PR 后，请 Copilot 合并 PR 并将分支切回 main：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果系统提示 **Allow** Copilot 代表你执行 GitHub 操作和运行 shell 命令，请审查命令并酌情选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结与后续步骤

你了解了 Copilot 如何读取本项目的说明文件，并使用 VS Code 中的 Copilot Chat 完成以下任务：

- 发送代码生成提示词，观察 Copilot 根据*现有*说明生成的内容；
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准；
- 重新发送相同提示词，观察生成的代码如何采用新标准；
- 将说明更新和辅助函数基础提交到 `main`，供下一练习接续使用。

接下来，在[智能体模式][next-lesson]中运用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
