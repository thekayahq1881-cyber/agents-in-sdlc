---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの 2 つの演習では、ユーザー エクスペリエンスの改善や機能追加に関する 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクの進行状況を監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。たとえば GitHub.com の[エージェント ページ][agents-page]を使うと、オープンなプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。実行内容を調べたり、進行中のセッションに追加の指示を与えたりできます。

このレッスンでは、次のことを行います。

- エージェント ページを開き、cloud agent のタスクを監視します。
- 進行中のセッションに指示を与え、追加機能を依頼します。

## シナリオ

エージェントにハイ コントラスト モードを作成させた後、チームはライト モードも追加したいと考えました。サイトのスタイルと切り替え機能をすでに変更しているため、ライト モードも含めるのが自然です。エージェントに作業を追加で依頼し、ハイ コントラストに加えてライト モードも実装してもらいます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン ペインと左ペインの両方に表示されるタスクの一覧を確認します。次のような、Copilot に割り当てたタスクが表示されます。
   - コードベースのドキュメントを更新する
   - ゲームを変更するための API を生成する
   - Web サイトにハイ コントラスト モードを追加する
3. 実行中のタスクを 1 つ選択し、Copilot が実行した作業を確認します。次のような内容が表示されます。
   - リポジトリからコードをチェックアウトする
   - Copilot の作業環境を作成する
   - MCP サーバーを設定する
   - 割り当てられたタスクを完了するための各種手順を実行する

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認して、作業をさらに監視できます。

## Cloud agent に追加の指示を与える

現在実行中のタスクを確認できたので、エージェントがハイ コントラスト モードに取り組んでいる間に、ライト モードの切り替えも追加するよう依頼します。

1. ハイ コントラスト モードを追加しているセッションを選択します。作業のタイトルは Copilot が選んだ名前や現在の状態によって異なります。

   ![mission control に表示されるアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![エージェント ページで、作業中の Copilot セッションへの追加指示ダイアログを選択した画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に取り入れる様子を確認します。

## Copilot に作業を続けてもらう

前回と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で進めていた手順が終わると、新しい依頼を作業に取り入れます。

今回も数分かかります。その間に休憩し、ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent のタスクを監視し、ガイダンスを与える中心的な場所である Copilot agents ページについて学びました。この mission control を使うと、進行中および完了したタスクの確認、実行中の作業のレビュー、進行中のタスクへの追加指示を行えます。

このレッスンでは、次のことを行いました。

- mission control とエージェント ページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成したプルリクエストを反復的に改善する][next-lesson]方法を確認しましょう。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復的に改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
