---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または構成し、ブラウザーで絞り込み機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに説明したように、コードを書くことだけが開発ではありません。データや外部サービスを使ったり、Copilot が利用できる自動化機能を追加したりすることも必要です。そこで MCP サーバーを使います。MCP サーバーを使うと、CLI に組み込まれている機能を超えて、Copilot にさまざまなツールやサービスを提供できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使うかを確認します。
- まだ使用できない場合は Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、絞り込み機能を確認します。

## シナリオ

ユニット テストとエンドツーエンド テストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。ユーザーと同じように Web サイトを使ってもらうことで、Copilot に変更作業をさらに自動化させ、期待どおりに動くという確信を高めたいと考えています。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信する仕組みです。MCP を使うと、AI エージェントはリアルタイムに外部のツールやサービスと通信し、最新の情報にアクセスしたり、依頼に応じて操作を実行したりできます。

ツールやリソースには、AI エージェントと外部のツールやサービスを仲介する MCP サーバーを通じてアクセスします。各 MCP サーバーは、AI エージェントが利用できるツールとリソースのセットを表します。

よく使われる MCP サーバーの例を挙げます。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内の MCP サーバーを見つけやすくし、貢献を促すために [MCP registry][mcp-registry] をホストしています。

> [!CAUTION]
> MCP サーバーは、ほかのプロジェクト依存関係と同じように扱ってください。使用する前にソース コードを確認し、発行元を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は追加し、Copilot がユーザーと同じように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに構成されている場合は、セットアップ手順を省略して次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーを追加するダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類が **STDIO** であることを確認し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を確認するよう Copilot に依頼する

これまでは、機能が期待どおりに動作することを手動で確認しました。今度は、Copilot が利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、自動的に MCP サーバーを見つけるため、プロンプトで指定する必要はありません。ただし、使用するサーバーがわかっている場合は、明示しても問題ありません。より一貫した結果が得られ、トークンも少し節約できます。

2. Copilot がブラウザーで機能を確認する手順を一覧表示し、実行する様子を見ます。
3. レポートを読み、すべてが期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright を使って Web サイトを操作し、サーバーを停止してから、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認できました。ここまでに、次のことを行いました。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使うかを学びました。
- まだ使用できない場合に Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、絞り込み機能を確認しました。

次は、スキルとブラウザー ツールを組み合わせ、[QA カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
