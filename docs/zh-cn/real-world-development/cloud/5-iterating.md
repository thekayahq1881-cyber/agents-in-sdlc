---
title: "练习 5：迭代改进 GitHub Copilot 的工作成果"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作成果

在本实验中，你与 GitHub Copilot 协作完成了多个旨在改善用户体验和添加功能的任务。你让 Copilot 为代码添加文档、构建供设计团队迭代改进的相关游戏功能，以及实现包括高对比度和浅色模式切换开关在内的无障碍功能。现在来查看代码更改；如有需要，还可以向 Copilot 提供反馈，进一步改进成果。

### 场景

如前所述，加入生成式人工智能 (AI) 并不会改变软件设计和开发与运维 (DevOps) 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。接下来，在为团队其他成员开启审查之前，先查看 GitHub Copilot 对文档、相关游戏功能和无障碍功能的建议。

## 安全与 GitHub Copilot 云智能体

由于 Copilot 云智能体会在没有监督的情况下异步执行任务，因此设置了一些安全限制，以确保操作安全。这些限制包括：

- Copilot 对存储库只有读取权限，对其用于编写代码的分支才有写入权限。
- 云智能体在 GitHub Actions 中运行，并创建独立的临时环境来工作。
- 任何 GitHub Actions 工作流都必须经人工批准后才能运行。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，模型上下文协议 (Model Context Protocol，MCP) 服务器也包括在内。

## 审查生成的文档

首先查看 GitHub Copilot 云智能体创建的第一个拉取请求 (PR)，其中包含为代码添加文档的工作。你将在 GitHub.com 上通过标准 PR 界面完成此操作。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot 云智能体的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 打开拉取请求列表。
3. 打开标题类似于 **Add missing documentation** 或更完整描述的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟；你可以稍作休息，或回顾目前学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并查看更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新添加的代码，包括 TSDoc 文档注释和其他文档。具体更改可能会有所不同。

    查看更改时，留意 TSDoc 文档注释、TypeScript 约定和注释头。这些内容来自练习 2 开始时查看的自定义说明文件。

6. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流指示器。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. 你应该会看到工作流在拉取请求的检查部分排队运行。如果一切正常，这款单体 Astro 应用的项目检查就会通过。完成此过程可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作处理拉取请求并非单向交流。你可以像提及团队其他成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会读取这些评论，并启动新会话来处理请求。由于结果具有非确定性，我们无法提供固定的提示词。你可以要求 Copilot 更新的内容包括：

- 在每个代码文件顶部添加简短说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建介绍 Astro 应用结构的 README。

1. 添加评论，请求修改生成的文档，并像提及其他用户一样标记 **@copilot**。可以使用上面的任一建议，也可以提出其他希望 Copilot 在代码库中处理的文档要求。
2. 选择 **View Session**，观察 Copilot 执行工作。留意 Copilot 如何启动新会话来完成更新。
3. 选择 **Back to pull request**，返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应该会出现新的提交。
5. 选择 **Files changed** 选项卡并审查更改。

如果还想继续迭代，可以重复上述操作，直到满意为止。满意后，可以将 PR 从草稿转换为准备审查状态，然后合并到 main 分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在回到 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Show related games on the game details page** 或更完整描述的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流指示器。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
8. 你应该会看到工作流在拉取请求的检查部分排队运行。如果一切正常，这款单体 Astro 应用的项目检查就会通过。完成此过程可能需要几分钟。
9. **可选：**也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 和终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已经构建了相关游戏功能！与之前一样，你可以通过迭代向 Copilot 云智能体请求更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义说明！）。和之前一样，可以在 **Conversation** 选项卡中添加评论来提出请求；Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查使用自定义无障碍智能体实现的无障碍功能。此 PR 应同时包含你在练习 3 中分配的高对比度模式，以及在练习 4 的任务控制中心中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Add high contrast mode to website** 或更完整描述的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]** 标记。此时请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 查看实现，特别留意：
   - 用于切换模式的用户界面 (UI) 组件；
   - 用于保存用户偏好的本地存储；
   - 高对比度和浅色模式的层叠样式表 (CSS) 或样式更改；
   - 无障碍属性（无障碍富互联网应用 (ARIA) 标签、键盘导航等）；
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流指示器。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. 你应该会看到工作流在拉取请求的检查部分排队运行。如果一切正常，这款单体 Astro 应用的项目检查就会通过。完成此过程可能需要几分钟。
10. **可选：**可以切换到 Codespace 中的此分支，手动测试无障碍功能。打开 Codespace 和终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换开关，并确认设置在重新加载页面后仍然保留。

请注意，自定义无障碍智能体如何引导 Copilot 按照无障碍最佳实践实现这些功能。如果发现无障碍方面的问题或改进点，可以像处理之前的 PR 一样，在评论中标记 **@copilot** 并请求更新。

## 可选练习——继续委派任务

云智能体最适合处理真实待办事项，让你能把注意力转移到其他工作。可以在自己的存储库中创建更多议题并分配给 Copilot，以培养这种协作方式。可以尝试：

- 在游戏详情页创建支持者意向表单。
- 在游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体路径的练习。在这些课程中，你：

- **检查了此存储库附带的自定义说明文件**，以便稍后查看这些说明对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，并观察它如何设置环境、制定计划和异步执行任务。
- **创建并使用自定义智能体**实现无障碍功能，添加高对比度和浅色模式切换开关。
- **使用 Copilot 智能体页面作为任务控制中心**，监控并在任务执行期间引导无障碍会话。
- **审查并迭代改进了云智能体创建的拉取请求**，通过标记 `@copilot` 请求更改并批准工作流。

## 审查与后续步骤

你已完成云智能体路径的练习。如果想继续探索，其他路径可以补充你在这里实践的内容：

- 🖥️ **[VS Code 路径](../../vscode/)**——在 IDE 中直接探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 路径](../../cli/)**——在终端中交付两项经过审查的更改，使用 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

在自己的存储库中，可以尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

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
