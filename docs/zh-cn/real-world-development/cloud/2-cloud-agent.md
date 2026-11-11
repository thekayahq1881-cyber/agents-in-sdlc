---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都面临技术债务问题，例如未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码、添加功能等任务。完成工作后，它会生成草稿拉取请求 (PR)，供开发者审查。这样可以把繁琐任务交给智能体，加快开发进程，让开发者专注于更重要的工作。

本课将使用 Copilot 云智能体了解以下内容：

- 如何自定义代码生成环境；
- 如何确保操作安全地执行；
- 明确界定议题范围的重要性；
- 如何将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初受雇开发网站的承包商没有留下完善的文档，事实上，应用完全缺少文档。首先，他们希望为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现体验。他们希望每款游戏的详情页都显示相关游戏，也就是同一类别中的其他游戏，让用户可以继续浏览。他们暂时不需要完善的实现，只需要一个可用于用户体验验收测试的版本。这项工作目前是阻碍因素，但还有优先级更高的议题。

这两项工作都可能很快被降级为低优先级任务，因此很适合分配给 Copilot 云智能体。云智能体可以异步处理这些任务，让开发者先专注于其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像人类开发者一样在后台执行任务。就像与人类开发者协作一样，你可以通过多种方式交办工作，包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求 (PR) 来跟踪进度、设置环境并开始处理任务。你可以在 Copilot 会话进行期间或完成后查看详情。解决方案准备好供你审查时，Copilot 会在拉取请求中标记你。

## 明确界定任务说明的重要性

GitHub Copilot 并非魔法——它不能代替思考。即使看似简单的操作，深入分析后也会发现其复杂性，因此[为 Copilot 云智能体明确界定任务范围][cloud-agent-best-practices]非常重要。应将它视为人工智能 (AI) 结对编程助手：分阶段工作、学习、试验并根据情况调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义说明

前面的练习介绍了自定义说明及其如何为 Copilot 提供指导。如果你已经完成自定义说明练习，就会看到 `.github/copilot-instructions.md` 的实际作用；如果还没有，现在可以快速查看。将工作分配给 Copilot 云智能体前，先只读查看此存储库中已有的说明文件，以便稍后观察它们的效果。

在 GitHub 网页界面中打开以下文件；如果已有正在运行的 codespace，也可以在那里打开：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，特别留意 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它会将这些说明限定用于 `**/*.test.ts` 文件。

下一部分将把 `Code lacks documentation` 议题分配给云智能体。留意随后生成的拉取请求是否包含 TSDoc 文档注释、TypeScript 约定和注释头；这些要求来自刚才查看的说明文件。稍后的练习还会再次审查拉取请求中的这些内容。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保一切就绪。将任务分配给 Copilot 时也是如此，因为它会以类似软件工程师 (SWE) 的方式工作。

云智能体使用 [GitHub Actions][github-actions] 环境执行任务。你可以创建一个[专用设置工作流][setup-workflow]，并将其配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，使其在智能体开始工作前运行。这能让它使用所需的开发工具和依赖项。为方便本实验流程并提供学习机会，我们已预先配置好此工作流，确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及这款单体 Astro 应用中已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为你的步骤设置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，步骤完成后 Copilot 会自动为你克隆。
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

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流文件类似，但有几个值得注意的地方：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，此作业会在 GitHub Actions 中运行。
- 请留意 `workflow_dispatch` 触发器。它允许你从存储库的 **Actions** 选项卡手动运行工作流。这很适合用于测试工作流是否成功运行，不必等待 Copilot 启动它。

## 添加文档

大家都知道文档的重要性，但大多数项目的信息要么过时，要么完全缺失。这类技术债务常常得不到处理，会降低工作效率，也让维护代码库或引入新开发者变得更加困难。Copilot 擅长创建文档，因此很适合让 Copilot 云智能体处理这类议题。它会在后台生成所需文档。之后的练习中，你会回来审查它的工作。

1. 在新的浏览器标签页中，打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 议题分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求 (PR)，其标题类似于 `[WIP]: Code lacks documentation`。如果列表中没有出现新的 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办事项列表。它正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方案。

13. 查看列表及其计划完成的任务。
14. 向下滚动拉取请求时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到已更新的指示器。

16. 查看实时会话，了解 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停思考并决定合适方案，以及创建代码。

此过程可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让我们能专注于其他工作。接下来会利用这一点：先再分配一项任务给 Copilot 云智能体，然后转而编写代码，为应用添加功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot 云智能体的一大优势是可以分工：你专注于一组任务，智能体同时处理另一组任务。为设计团队添加相关游戏部分未必需要很长时间，但这段时间仍可用于处理其他工作。现在就把它分配给 Copilot 云智能体吧！

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为：`Show related games on the game details page`
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

不久后，议题的第一条评论中会出现一组 👀，表示 Copilot 已开始处理！

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在认真处理第二项请求！Copilot 云智能体的工作方式与软件工程师类似，因此你不需要持续监控，而是在完成后进行审查即可。现在可以转而创建和使用自定义智能体了。

## 总结与后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。使用云智能体，可以将议题分配给 Copilot 进行异步处理。可以让 Copilot 处理技术债务、创建新功能，或协助将代码从一种框架迁移到另一种框架。

本课介绍了以下概念：

- 如何自定义代码生成环境；
- 如何确保操作安全地执行；
- 明确界定议题范围的重要性；
- 如何将议题分配给 Copilot。

云智能体正在后台工作，现在可以转而创建和使用自定义智能体。[Copilot 云智能体也可以使用模型上下文协议 (Model Context Protocol，MCP) 服务器][cloud-agent-mcp]，并且可以读取自定义说明；前面的模块已经介绍过自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

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
