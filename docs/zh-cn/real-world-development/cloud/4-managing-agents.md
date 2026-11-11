---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几课中，你已让 Copilot 云智能体处理了三个侧重于改善用户体验和添加功能的任务。云智能体虽然可以异步、自主地工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制中心，你可以查看所有具有未关闭拉取请求 (PR) 的智能体任务，了解执行的操作，甚至可以引导正在进行的会话。

本课将：

- 浏览智能体页面，监控云智能体任务；
- 引导正在进行的会话，请求添加其他功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到也可以趁此机会添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，因此把此功能一并纳入很合理。现在，你要引导智能体，确保它在实现高对比度模式的同时也添加浅色模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。列表中应包含你分配给 Copilot 的任务，例如：
    - 更新代码文档；
    - 生成用于修改游戏的 API；
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从存储库检出代码；
    - 为 Copilot 创建工作环境；
    - 设置 MCP 服务器；
    - 执行完成任务所需的各项操作。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 的工作状态和采用的方案而变化。

4. 同时留意右侧显示的拉取请求 (PR) 窗格。在此处可以查看 PR 和已更改的文件，以便进一步监控。

## 引导云智能体

查看正在运行的任务后，可以在 Copilot 实现高对比度模式时要求它也添加浅色模式。

1. 选择用于添加高对比度模式的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![智能体控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 等待几分钟，直到会话显示已完成设置并开始工作。出现类似下方的消息时，就表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示词：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面上的云智能体任务截图，其中突出显示了“Steer active session while Copilot is working”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示词。
5. 留意 Copilot 如何确认收到提示词，并将其纳入工作流程。

## 等待 Copilot 完成工作

与之前一样，Copilot 会开始处理更新后的任务！完成当前步骤后，它会把你的新要求纳入工作流程。

这同样需要几分钟，正好可以暂停一下，回顾到目前为止学到和探索的内容。

## 总结与后续步骤

本课介绍了 Copilot 智能体页面，它是监控和引导 GitHub Copilot 云智能体任务的中心控制台。你可以通过该控制台跟踪所有正在进行和已完成的任务、审查处理过程，还可以重定向正在运行的任务以调整范围或提供更多指导。

本课介绍了以下概念：

- 使用智能体控制中心和智能体页面监控云智能体任务；
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能。接下来，[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
