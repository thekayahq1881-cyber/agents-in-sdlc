---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストを実行するなどの作業が必要です。開発者は、必要なファイルを見つけ、変更を行い、テストを実行し、デバッグし、更新し忘れたファイルを見つけ、さらに変更を行う、といった作業を繰り返します。

このようなときに Copilot Agent Mode を使います。

Copilot Agent Mode は、IDE 内でより自律的に作業するように設計されています。開発者のように既存のプロジェクト構成を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使って、サイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる用途向けに作られています。エージェント モードは IDE 内で動作するため、すばやいフィードバックと対話が可能です。Cloud agent はチーム メンバーのように非同期で作業し、issue やプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調べ、関係するファイルを特定し、複数の変更を連携して行う方法
- GitHub Copilot Agent Mode が UI とデータ レイヤーにまたがる新機能を実装する方法
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリや発行者で絞り込めるようにします。前の演習では publishers helper を追加しました。次は、Copilot Agent Mode を使って残りのデータ レイヤー、UI、テストの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、現在の機能を把握するために Tailspin Toys の Web サイトを確認しましょう。

この Web サイトは、開発者をテーマにしたボード ゲームのクラウドファンディング プラットフォームです。ゲームの一覧や詳細を表示できます。単一の Astro アプリで、ビルド時にページを静的 HTML としてレンダリングします。ページは Drizzle ORM を通じて frontmatter からローカルの SQLite データベースを直接クエリします。独立したバックエンド API やクライアント側の UI フレームワークはありません。再利用可能なデータアクセス helper は `src/lib/` に置き、インタラクティブな動作には標準の DOM API を使った小規模で範囲を限定した Astro `<script>` を使用します。

### Web サイトを起動する

Web サイトを簡単に実行できるように、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace では、次の手順で実行します。

1. Codespace に戻ります。前の演習の後、`main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 絞り込み機能の作業用に、新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次の例のように URL を示すメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` のブラウザー ウィンドウを開くダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して、Web サイトを開きます。

> [!NOTE]
> Codespace でローカルホストの URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは Web サーバーをホストする Codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。主な機能は次のとおりです。

- **ホーム ページ**: ボード ゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行者、カテゴリなどの情報を掲載した詳細ページが表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。各手順で必要な場合に説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトを作成すると、issue のバックログも自動的に作成されています。GitHub Copilot に項目を表示してもらいましょう。

1. Chat ビューのエージェント選択メニューで **Agent** を選択します。**Agent** エージェントは、自律的に変更の計画と実装を行い、ファイルをまたぐ更新、ターミナル コマンドの実行、ツールの呼び出しを行います。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨するものではありません。ワークショップの作成には Claude Sonnet 4.5 を使ったため、ここでも同じモデルを指定しています。これにより、コードの提案に一貫性を持たせ、よりよい体験を提供できることを期待しています。ただし、LLM は確率的に動作するため、ワークショップの例とは異なる提案が表示されることがあります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot は別の MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot は、ユーザーに最も役立つと考える項目を優先順位付けしてくれます。

## Instruction file を確認する

エージェントにコードを生成させる前に、作業のコンテキストを Copilot に伝える instruction file を確認しましょう。Web サイトに機能を追加する際の考え方を説明する [user interface (UI)][ui-instructions] ファイルを使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の進め方に関する全体的なガイダンスを確認します。次の内容が含まれています。
   - アーキテクチャの概要
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle など、ファイル種別ごとの指示へのリンク

> [!TIP]
> Instruction file から、ほかの instruction file やプロジェクト内のファイルを参照できます。パスは instruction file の場所からの相対パスです。指示を再利用しやすく小さな単位に分け、例やテンプレートを示せます。

## 絞り込み機能を実装する

絞り込み機能を完成させるには、少なくとも次の 3 つの変更が必要です。

- データ レイヤー (`src/lib/`) に残りの絞り込みロジックを追加または調整します。
- 絞り込みの動作を確認するテストを追加または更新します。
- ゲーム一覧ページに絞り込み UI を追加します。

コードベースにマージする前に、テストを実行して成功させる必要もあります。Copilot Agent Mode にこれらの作業を依頼できます。機能を追加しましょう。

1. 現在の Copilot の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、instruction file を追加します。

   ![UI instruction file を選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント選択メニューで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログの関連 issue に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、目的の機能に関係するファイルを探す様子を確認します。データ レイヤー helper、UI、テストを見つけた後、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトのファイルを調べている画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行など、複数のタスクを行います。タスクやコードベースの複雑さによっては数分かかります。作業中、コード エディターに **Keep** と **Undo** ボタンが表示されることがあります。Copilot の作業が終わると、すべての変更に対して **Keep** または **Undo** を選択できるようになります。作業中に選択する必要はありません。

7. Copilot から求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat ペインでテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はコード生成とテストを繰り返し、タスクを完了してエラーがないと判断するまで作業を続けます。

   ![Copilot Agent Mode のチャット セッションの完了画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いたブラウザーに戻り、新しい機能を試します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って、Tailspin Toys の Web サイトに機能を追加する方法を学びました。次のことを確認しました。

- GitHub Copilot Agent Mode が UI とデータ レイヤーにまたがる新機能を実装できること
- Copilot Agent Mode がプロジェクトを調べ、関係するファイルを特定し、複数の変更を連携して行えること
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューすること

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### おまけの演習 — ページ分割を実装する

ゲーム一覧が増えるにつれて、ページ分割が必要になります。この演習で学んだことを使い、Copilot にサイトのページ分割機能の実装を依頼してみましょう。次の点を検討してください。

- 既存の instruction file を使うなど、既存のベスト プラクティスに従います。
- ページ分割の方法を決めます。ユーザーがページ サイズを選べるようにするか、値を固定するかを検討します。
- 望む実装に必要なガイダンスを、プロンプトで Copilot に伝えます。
- Copilot に変更やコンテキストを伝えながら、何度かやり取りする必要があるかもしれません。Copilot を使う通常のワークフローです。

## リソース

- [Copilot の Ask、Edit、Agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
