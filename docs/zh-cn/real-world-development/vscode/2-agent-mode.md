---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。作为开发者，你的工作流程通常包括：查找所有相关文件、进行更改、运行测试、调试、找出遗漏的文件、再次修改……类似步骤还有很多。

Copilot 智能体模式正是为此而设计的。

Copilot 智能体模式可以在集成开发环境 (IDE) 中更自主地工作。它的行为类似开发者：先探索现有项目结构，进行必要更新，运行测试等任务，并自动修复发现的问题。接下来看看如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体的名称相似，但它们针对两种不同的体验。智能体模式在 IDE 中执行任务，便于快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍如何：

- 使用 Copilot 智能体模式探索项目、查找相关文件并协同修改；
- 使用 GitHub Copilot 智能体模式跨用户界面 (UI) 和数据层实现新功能；
- 在将代码合并到代码库之前，审查 GitHub Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望允许用户按类别和出版社筛选游戏。上一练习中已经添加了出版社辅助函数，现在将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

开始修改之前，先浏览 Tailspin Toys 网站，了解它目前的功能。

该网站是一个带有开发者主题的桌游众筹平台。用户可以浏览游戏列表并查看游戏详情。网站是一个单体 Astro 应用，会在构建时将页面渲染为静态超文本标记语言 (HTML)。页面通过 Drizzle 对象关系映射 (ORM) 在 frontmatter 中直接查询本地 SQLite 数据库；没有独立的后端应用程序编程接口 (API) 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则通过使用标准文档对象模型 (DOM) API 的小型、局部 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，便于启动单体 Astro 开发服务器。按照以下步骤，可以在 GitHub Codespace 中运行该任务：

1. 返回 Codespace。上一练习结束后，你应该已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新的终端窗口。
3. 创建并切换到用于筛选工作的新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，终端中会显示类似以下内容的提示：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端里的 localhost 链接时，系统会自动将其重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace 的私有隧道，网站服务器正在其中运行。

### 浏览网站

网站运行后，可以浏览其功能。主要功能包括：

- **主页**：显示桌游列表，包括游戏标题、图片和说明。
- **游戏详情页**：选择游戏后会进入详情页，查看游戏标题、说明、出版社和类别等信息。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，以启动一个干净的对话。不同练习所需的模式和模型可能不同；每个步骤会在相关处说明。

网站的初始实现已具备基本功能，但我们还希望添加新能力。先查看待办事项。在你根据模板设置项目时，系统已自动创建了一组议题；请让 GitHub Copilot 列出这些事项。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![显示 Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某一种模型。编写本研讨会时使用的是 Claude Sonnet 4.5，因此在说明中列出了该模型。我们希望这样能让代码建议相对一致，确保练习体验良好。但由于大型语言模型 (LLM) 的输出具有概率性，你可能会发现生成的建议与研讨会中描述的内容不同。这完全正常，也是预期情况。

> [!NOTE]
> 由于 LLM 输出具有概率性，Copilot 可能会使用不同的模型上下文协议 (Model Context Protocol，MCP) 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示词，询问待办事项中的议题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

留意 Copilot 甚至会根据它认为对用户最有用的事项为列表排序。

## 查看说明文件

让智能体开始生成代码之前，先查看用于向 Copilot 提供工作上下文的说明文件。接下来将使用[用户界面 (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) 说明文件，其中介绍了为网站添加功能时应采用的方法。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意文件中添加功能时的总体指导，包括：
   - 架构概述；
   - 组件设计、可测试性和无障碍原则；
   - 指向各类文件专用说明的链接，包括：
     - Astro；
     - Tailwind CSS（层叠样式表）；
     - Drizzle。

> [!TIP]
> 说明文件既可以引用其他说明文件，也可以引用项目中的文件。路径相对于说明文件所在位置。这样便于复用、将较大的说明拆分成更小且易于管理的部分，并为 Copilot 提供代码示例。

## 实现筛选功能

要完成筛选功能，至少需要对应用进行以下三项更新：

- 在数据层 (`src/lib/`) 添加或完善剩余的筛选逻辑；
- 添加或更新筛选行为的测试；
- 更新游戏列表页，添加筛选 UI。

此外，在将更改合并到代码库之前，测试必须全部通过。Copilot 智能体模式可以完成这些工作！现在开始添加此功能。

1. 可以继续使用当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选择了 **Agent**。

   ![显示 Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示词，让 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目，查找与所需功能相关的文件。你应该会看到它查找数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> 你会看到 Copilot 执行多项任务，例如探索项目、修改文件和运行测试。根据任务复杂度和代码库规模，此过程可能需要几分钟。在此期间，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示 **Keep** 或 **Undo** 按钮；无需在工作进行期间选择这些按钮。

7. 按 Copilot 的提示选择 **Continue**，运行测试。

   ![Copilot Chat 窗格中询问用户是否允许运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会出现暂停，也可能看到测试暂时失败。这没有关系！Copilot 会在生成代码和运行测试之间反复处理，直到任务完成且未发现错误。

   ![Copilot 智能体模式完整聊天会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查看是否存在潜在问题。

> [!CAUTION]
> 请记住，始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认一切正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。你了解了如何：

- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能；
- 使用 Copilot 智能体模式探索项目、查找相关文件并协同修改；
- 在将代码合并到代码库之前，审查 Copilot 智能体模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 奖励探索练习——实现分页

随着游戏列表不断增长，就需要实现分页。运用本练习中学到的技能，提示 Copilot 更新网站以实现分页。编写代码时，可以考虑：

- 遵循现有最佳实践，包括使用已有的说明文件；
- 考虑如何实现分页，例如允许用户选择每页大小，还是将其固定；
- 编写提示词时，为 Copilot 提供实现所需的指导；
- 可能需要通过提供上下文并要求 GitHub Copilot 修改来反复迭代。这是与 Copilot 协作时的正常流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能与适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
