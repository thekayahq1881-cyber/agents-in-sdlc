---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

エージェント モードを使って、絞り込み機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot に新しいツールを追加する仕組みを学びます。
- ワークスペースに Playwright MCP サーバーを追加します。
- Copilot に MCP サーバーを使わせ、ブラウザーで絞り込み機能を手動でテストします。
- ブランチを公開して、絞り込みのプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェント モードは、エディターの外にアクセスできると、さらに強力になります。Copilot がそのために使う仕組みが Model Context Protocol (MCP) です。エージェントが外部のツールやサービスと通信するための標準です。

![エージェント モードがコンテキスト、LLM、ツール (MCP サーバーや VS Code 拡張機能が提供するツールを含む) と連携する仕組みを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP によって、AI エージェントはリアルタイムに外部のツールやサービスと通信し、リソースを使って最新情報にアクセスしたり、ツールを使って依頼に応じた操作を実行したりできます。

ツールやリソースには、AI エージェントと外部のツールやサービスを仲介する MCP サーバーを通じてアクセスします。MCP サーバーは、AI エージェントと外部ツール (既存の API や npm パッケージなどのローカル ツール) 間の通信を管理します。各 MCP サーバーは、AI エージェントが利用できるツールとリソースのセットを表します。

よく使われる MCP サーバーの例を挙げます。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。AI エージェントは、新しいリポジトリの作成、既存リポジトリの更新、issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。AI エージェントは Web ページへの移動、フォームへの入力、ボタンの選択などを行えます。

さまざまなツールやリソースにアクセスできる MCP サーバーがほかにもあります。GitHub は、エコシステム内の MCP サーバーを見つけやすくし、貢献を促すために [MCP registry](https://github.com/mcp) をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはほかのプロジェクト依存関係と同じように扱ってください。使用する前にソース コードを慎重に確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` サーバーが構成されていることを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

`github` の設定はプロジェクト テンプレートに含まれています。そのため、前の演習で Copilot はバックログの issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot が最初にツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が操作できるブラウザーを利用できます。これを使って機能をテストします。

1. `.vscode/mcp.json` で `github` の隣に `playwright` の項目を追加し、ファイルを次のようにします。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモートの `github` サーバーと異なり、`playwright` は**ローカル** サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、Playwright は表示ウィンドウなしでブラウザーを実行します。デスクトップ環境がない Codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンド テストに Playwright をすでに使っているため、通常、ブラウザーもインストール済みです。後で Copilot からブラウザーが見つからないと報告された場合は、`npx playwright install chromium` を実行するよう依頼して、もう一度試してください。

## サーバーを起動して信頼する

VS Code は、MCP サーバーを**必要に応じて**起動します。Copilot が初めてサーバーのツールを必要としたときに VS Code が起動し、サーバーを信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` の設定の上に **Start** 操作が表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認された場合は、設定を確認し、サーバーを起動できるように信頼します。

手動での起動は任意です。省略した場合は、次の手順で Copilot が初めてツールを必要としたときに VS Code がサーバーを起動し、信頼するか確認します。

## 絞り込み機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを選択して確認する代わりに、エージェントにページを開かせ、移動し、フィルターを適用して、その結果を読み上げてもらえます。会話を離れずに機能が期待どおりに動作するか確認できる、最も手早い方法です。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。つまり、エージェントはボタン、リンク、リスト項目など、ラベル付きの構造化要素をもとに推論します。支援技術と同じようにページを確認できるため、簡単な機能テストがアクセシビリティの簡易チェックにもなります。

サーバーを接続してアプリを起動したら、次のプロンプトを Copilot に送信し、絞り込み機能を確認します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して、見つかった内容を報告します。issue の受け入れ条件と照らし合わせて要約を読みます。問題がありそうな場合は、プルリクエストを開く前に、追加で質問するか、コードを修正するよう依頼します。

> [!NOTE]
> このテストには、アプリが `http://localhost:4321` で起動している必要があります。開発サーバーを停止した場合は、プロンプトを送る前にもう一度起動してください。Copilot が Playwright MCP サーバーを初めて使うときに、ブラウザーをダウンロードする場合があります。ブラウザーが見つからないと報告されたら、`npx playwright install chromium` を実行するよう依頼し、もう一度試してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたので、チームがレビューできるようにプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace で **Source Control** パネルに移動し、Copilot が変更したファイルを確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![Copilot が変更したファイルを表示する Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンド ライン インターフェイス (CLI) など、プルリクエストを作成する方法はいくつかあります。GitHub Copilot を使っているので、PR の作成も依頼しましょう。Copilot は関連する issue を見つけ、issue と関連付けて PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. GitHub ツールを使えるように、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、情報の取得や操作を Copilot に許可します。GitHub ツールを初めて使うときは、GitHub への認証を求められることがあります。画面の案内に従って許可してください。
5. Copilot が issue を検索し、該当するものを見つけて PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択してプルリクエストをレビューします。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot に新しいツールを追加する仕組みを学びました。
- ワークスペースに Playwright MCP サーバーを追加しました。
- MCP サーバーを使って、リリース前の絞り込み機能をブラウザーで手動テストしました。
- ブランチを公開し、絞り込み機能のプルリクエストを作成しました。

機能のテストに MCP サーバーを使いましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次は、アクセシビリティのレビューなど、目的を絞ったタスクを効率化するために[カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
