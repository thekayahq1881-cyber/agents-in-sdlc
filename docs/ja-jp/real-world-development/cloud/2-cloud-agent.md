---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシ コード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加といったタスクを自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せて開発を加速し、より重要なことに集中できます。

Copilot cloud agent を使って、次のことを確認します。

- コード生成環境をカスタマイズする方法
- 操作を安全に行う方法
- 範囲を明確に定めた issue の重要性
- issue を Copilot に割り当てる方法

## シナリオ

Tailspin Toys には、対応したい技術的負債があります。サイトの初期バージョンを作った請負業者が、ドキュメントを適切に整備しませんでした。実際に確認すると、ドキュメントがまったくありません。まず、アプリケーションのすべての export された関数に TSDoc の doc comment を追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する関連ゲームを表示し、閲覧を続けられるようにします。まだ仕上げる必要はなく、UX の受け入れテストに使えるものを求めています。この作業は現在ブロッカーですが、より優先度の高い issue がほかにあります。

どちらも、優先順位が下がりやすく、Copilot cloud agent に割り当てるのに適したタスクです。cloud agent に非同期で作業させる間に別の作業を進め、後から成果をレビューして要件どおりか確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)など、いくつかの方法があります。割り当てると、Copilot は進行状況を追跡するドラフト プルリクエストを作成し、環境をセットアップして、タスクに取りかかります。作業中でも完了後でも、Copilot のセッションを詳しく確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を適切に定めた指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な作業でも、詳しく調べると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペア プログラマーとして扱い、段階的に作業しながら学び、試し、必要に応じて調整します。生成 AI を使っても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と、それが Copilot のガイダンスにどう使われるかを確認しました。カスタム指示の演習をすでに行った場合は、`.github/copilot-instructions.md` が機能する様子を見ています。まだの場合は、ここで簡単に確認しましょう。Copilot cloud agent に作業を依頼する前に、このリポジトリに含まれる instruction file を読み取り専用で確認しておくと、後で指示の効果を見つけやすくなります。

リポジトリの GitHub Web UI か、起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc doc comment に関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter を確認します。この指示は `**/*.test.ts` に一致するファイルに適用されます。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てたら、作成されたプルリクエストに TSDoc doc comment、TypeScript の規約、コメント ヘッダーが反映されているか確認してください。これらの指示は、後のレッスンで PR をレビューするときにも取り上げます。

## Copilot cloud agent の開発環境を設定する

コードの作成には、誰が担当する場合でも、通常、適切な環境と初期設定が必要です。タスクを Copilot に割り当てる場合も同じで、Copilot は SWE と同じように作業します。

Cloud agent は、作業環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定した[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に環境をカスタマイズし、必要な開発ツールや依存関係を用意できます。ラボを進めやすくし、この機会に学べるよう、環境は事前に設定されています。単一の Astro アプリ用に Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションとシードが済んだ SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Copilot には操作用の独自トークンが渡されます。セットアップ手順に必要な最小限の権限を設定してください。
    permissions:
      # 依存関係のインストールなどでセットアップ手順中にリポジトリをクローンする場合は、contents: read 権限が必要です。クローンしない場合は、手順の完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ — Node.js (アプリ全体は Astro + Drizzle/libSQL)
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

      # ビルドとテストでデータを使えるように、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフロー ファイルと同じ形式ですが、次の点に注意してください。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずに、ワークフローが正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、ほとんどのプロジェクトでは情報が古いか、ドキュメントが不足しています。この種の技術的負債は対応されないままになりやすく、生産性を落とし、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメントの作成を得意とするため、Copilot cloud agent に割り当てるのに適した issue です。バックグラウンドで必要なドキュメントを作成し、後の演習で成果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が todo リストを作成します。

> [!NOTE]
> Copilot が PR に todo リストを表示するまで、数分かかることがあります。Copilot は先ほどのワークフローを実行して環境を作成し、プロジェクトを分析して、問題への最適な取り組み方を検討しています。

13. todo リストを確認し、Copilot が実行するタスクを把握します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッション表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示が見えるように、ブラウザーを更新する必要がある場合があります。

16. ライブ セッションをスクロールし、Copilot の作業を確認します。コードを調べて状態を理解し、計画を考えて判断し、コードを作成する様子がわかります。

作業には数分かかるでしょう。Copilot cloud agent の主な利点は、非同期で作業できるため、別のタスクに集中できることです。この利点を活かすため、cloud agent にもう 1 つのタスクを割り当てた後、アプリケーションへの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームのセクションを追加する

前述のとおり、GitHub Copilot cloud agent の利点の 1 つは、作業を分担できることです。デザイン チーム向けに関連ゲームのセクションを作成するのに時間がかからないとしても、その時間をほかの作業に使えます。このタスクも Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

ほどなくして、issue の最初のコメントに 👀 が表示され、Copilot が作業を始めたことがわかります。

![issue で Copilot が作業中であることを目の絵文字で示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent は SWE と同様に作業するため、完了まで積極的に監視する必要はありません。完了後にレビューできます。その間に、カスタム エージェントの作成と使用を確認しましょう。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に issue を割り当て、非同期で作業させることができます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などに活用できます。

このレッスンでは、次のことを確認しました。

- コード生成環境をカスタマイズする方法
- 操作を安全に行う方法
- 範囲を明確に定めた issue の重要性
- issue を Copilot に割り当てる方法

Copilot がバックグラウンドで作業している間に、カスタム エージェントの作成と使用に取り組みましょう。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も使います。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
