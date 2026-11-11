---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并通过浏览器体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码不只是写代码。我们还要处理数据、外部服务，并让 Copilot 能使用其他自动化工具。这就是 MCP 服务器的作用。MCP 服务器可以扩展 Copilot CLI 的内置功能，为它提供更多工具和服务。

本课将：

- 了解什么是 Model Context Protocol (MCP)，以及 Copilot CLI 如何使用它；
- 如果尚未配置 Playwright MCP 服务器，则添加该服务器；
- 让智能体操作浏览器，体验筛选功能。

## 场景

单元测试和端到端测试固然重要，但要验证 UI 更新，就必须实际与 UI 交互。你希望 Copilot 能像用户一样使用正在开发的网站，进一步自动化更改流程，并更有把握地确认更新符合预期。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 为 AI 智能体提供了与外部工具和服务通信的方式。借助 MCP，AI 智能体可以实时访问最新信息，并代表你执行操作。

AI 智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当智能体与外部工具和服务之间的桥梁，每个 MCP 服务器都代表一组智能体可以访问的工具和资源。

以下是两个常见的 MCP 服务器：

- **[GitHub MCP Server][github-mcp]**：提供管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 提供了一个 [MCP 注册表][mcp-registry]，方便用户发现并参与生态系统建设。

> [!CAUTION]
> 请像对待项目的其他依赖项一样对待 MCP 服务器。使用前先检查源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请添加它，让 Copilot 可以像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已配置 Playwright MCP，请跳过这些设置步骤，直接继续下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器的对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下内容：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux），保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 请 Copilot 通过 Playwright 体验功能

之前你已手动确认功能表现正常。现在使用 Copilot 中可用的 Playwright MCP 服务器，重复此操作！

1. 使用以下提示词，让 Copilot 通过 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常无需明确要求 Copilot 使用 MCP 服务器，因为它会自行判断。但既然你知道要使用哪个工具，明确指出也有好处！这样可以提高结果的一致性，还能节省一些 token。

2. 观察 Copilot 如何列出在浏览器中确认功能正常的各个步骤。
3. 阅读报告并确认所有行为符合预期。

Copilot 会启动服务器，使用 Playwright 与网站交互，停止服务器，然后提供报告。

## 总结与后续步骤

恭喜你通过 Copilot CLI 中的 Playwright MCP 服务器，在真实浏览器中体验了功能！本课中，你：

- 了解了 Model Context Protocol (MCP) 以及 Copilot CLI 如何使用它；
- 如果尚未配置 Playwright MCP 服务器，则添加该服务器；
- 让智能体操作浏览器，体验筛选功能。

接下来，[创建 QA 自定义智能体][next-lesson]，将技能和浏览器工具整合到专用角色中。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
