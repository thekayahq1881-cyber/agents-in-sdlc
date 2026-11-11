---
title: "レッスン 2 - 星評価を追加する: 小さな改善"
description: "Copilot CLI でゲーム カードに小さな変更を加え、転送したブラウザーでレビューして、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次は、プロジェクトに最初の変更を加えましょう。小さな変更から始めます。ゲーム データには星評価がすでにありますが、ホーム ページのゲーム カードには表示されていません。エージェントに評価を表示させ、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで、目的を絞った Copilot の会話を開始します。
- エージェントにプロジェクトの小さな変更を依頼します。
- `/diff` で変更をレビューします。
- 転送したブラウザーでアプリを実行し、変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。しかし、ホーム ページのゲーム カードにはタイトル、カテゴリ、発行者、説明しか表示されていません。準備運動として、各カードに既存の評価を表示するようエージェントに依頼します。これは小規模で独立した変更なので、最初のセッションに適しています。

## 会話の構成

**会話**では、タスクに取り組むために Copilot CLI を使います。Copilot app と異なり、通常の CLI の会話では、専用の worktree を作成せず、ターミナルで現在チェックアウトされているリポジトリと Git ブランチを使います。保存した会話は後で再開できます。ファイルとブランチは通常の Git の状態としてディスク上に残ります。

会話では、次の 3 つを確認できます。プロンプトとエージェントの応答、ファイルを調べて編集する際のエージェントのツール使用状況、そして `/diff` で確認できる変更です。

## 会話を開始して変更を依頼する

機能の実装に向けて、新しい会話を開始します。

1. Codespace に戻ります。
2. まだターミナルを開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. まだ起動していない場合は、次のコマンドを実行して Copilot を起動します。

   ```bash
   copilot --yolo
   ```

4. スラッシュ コマンド `/new` を入力して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトを使って変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲーム詳細の表示に使われているファイルを特定し、必要なコードを作成します。Copilot CLI で新しい機能を追加できました。

## 差分をレビューする

小さな変更でも、AI が生成した変更はマージする前にレビューします。Copilot CLI で変更を確認しましょう。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. ゲーム カードに評価がある場合は数値が、`starRating` が `null` の場合は `No rating yet` が表示されることを確認します。
3. 両方の状態がテストされていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> Copilot をはじめとする生成 AI ツールは確率的に動作し、決定論的ではありません。そのため、実際に生成されるコードは異なる場合があります。特定の実装と一致することではなく、動作をレビューしてください。

## 変更を確認する

コードを読むだけで動作すると判断するのではなく、Copilot に Web サイトを起動してもらい、Codespaces が転送したブラウザーで更新後のユーザー インターフェイス (UI) を確認します。

1. 次のプロンプトで、Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が使用可能になったら、**Open in Browser** を選択します。
3. ゲーム カードに 5 段階評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

機能が完成したので、コードをプロジェクトにマージするためのプルリクエスト (PR) を作成します。

1. 既定のエージェントに、変更のコミットを依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は既存のコミットをプッシュして PR を作成し、PR の URL を表示します。
3. Copilot CLI に表示された URL を <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して、PR を開きます。
4. 変更されたファイルとチェック結果をレビューします。
5. 準備ができたら **Merge pull request** を選択し、マージを確認します。
6. Codespace に戻り、`/exit` を入力して Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って、最初の変更をリリースできました。具体的には、次のことを行いました。

- 機能ブランチで目的を絞った Copilot の会話を開始しました。
- ゲーム カードに小さな変更を加えるようエージェントに依頼しました。
- `/diff` で変更をレビューしました。
- 転送したブラウザーでアプリを実行し、星評価を確認しました。
- 最初のプルリクエストを作成してマージしました。

次は、[絞り込み issue から始めて Plan モードと Autopilot モードを使い][next-lesson]、より大きな機能を作成します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI のコマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
