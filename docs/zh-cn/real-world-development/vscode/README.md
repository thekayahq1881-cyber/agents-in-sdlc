---
slug: zh-cn/real-world-development/vscode
title: "Visual Studio Code"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code 中的 **[GitHub Copilot Chat](https://code.visualstudio.com/docs/copilot/chat/copilot-chat)** 将 GitHub Copilot 带入你日常使用的代码编辑器。在 Visual Studio Code（以及 GitHub Codespaces）中，你可以使用 Copilot Chat 智能体模式、通过 MCP 连接外部工具，并借助自定义智能体；无需离开 IDE，Copilot 就能查看你的文件、终端和问题。

你将先添加自定义说明并观察 Copilot 如何遵循说明，然后使用智能体模式跨 UI、数据层和测试构建筛选功能。接下来，连接 Playwright MCP 服务器，让 Copilot 操作浏览器测试功能，然后创建拉取请求。最后，你将审查并使用用于无障碍工作的自定义智能体，然后在编辑器中监控、引导并迭代 Copilot 所做的更改。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 Codespace |
| [1. 自定义说明][ex1] | 上下文 | 在 VS Code 中添加并验证自定义说明 |
| [2. 智能体模式][ex2] | 代码生成 | 使用智能体模式构建筛选功能 |
| [3. 使用 Playwright 的 MCP][ex3] | 外部工具 | 使用 Playwright MCP 服务器在浏览器中测试功能 |
| [4. 自定义智能体][ex4] | 专用智能体 | 审查和使用自定义智能体 |
| [5. 管理智能体][ex5] | 监控 | 监控并引导智能体会话 |
| [6. 迭代][ex6] | 审查 | 在本地审查 Copilot 的工作并选择后续步骤 |
| [可选：集成 Foundry][foundry-toolkit] | AI 智能体 | 使用 VS Code 和 Foundry Toolkit 准备模型、部署智能体并将其连接到网站 |

## 先决条件

参加本研讨会前，请确保具备以下条件：

- [ ] 拥有 GitHub 账户，并已启用 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划
- [ ] 可以访问 GitHub Codespaces

> [!TIP]
> 没有付费计划？经过验证的学生可以通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本研讨会使用的智能体、MCP、代码审查和 Copilot CLI 功能，可用于完成所有练习路径。

[callout-student-plan-education]: https://github.com/education/students

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-agent-mode/
[ex3]: 3-mcp/
[ex4]: 4-custom-agents/
[ex5]: 5-managing-agents/
[ex6]: 6-iterating/
[foundry-toolkit]: 7-foundry-toolkit/
