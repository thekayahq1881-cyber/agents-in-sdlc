---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルを使って自動チェックを実行し、Playwright MCP を使ってブラウザーで絞り込み機能を確認しました。次は、明確に定義した QA プロセスを持つカスタム エージェントに、これらの機能をまとめます。

このレッスンでは、次のことを行います。

- カスタム エージェントが指示、スキル、MCP ツールとどのように連携するかを確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、絞り込み issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys は、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト範囲、ブラウザーでの動作を一貫してレビューしたいと考えています。カスタム エージェントを使うと、QA プロセスを調整し、再利用できるレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する、Copilot の専門的な役割です。プロファイルには、エージェントの目的、指示、利用できるツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を説明します。`quality-checks` スキルは繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、要件を評価して結果を報告するために、Copilot がそれらの機能をどう使うかを示します。これらを置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用可能な QA プロファイルの作成を Copilot に依頼します。プロファイルでは、QA が実施するチェックと、守るべき境界の両方を定義します。

1. Codespace に戻り、絞り込みの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、新しいカスタム エージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA ワークフローと境界を反映したか確認します。検証だけを依頼したときに、エージェントが機能を変更してしまうのを防ぐためです。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は任意ですが、指定するとエージェントの表示名がわかりやすくなります。
3. プロファイルの指示を読み、QA が要件を起点とし、リポジトリの指示に従い、`quality-checks` スキルを実行して、Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告すること、実装コードを変更する前に確認を求めること、変更をコミットしたりプルリクエストを開いたりしないことを確認します。
5. これらの役割や境界がプロファイルに含まれていない場合は、続行する前に既定のエージェントに修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後、絞り込みの会話を再開し、QA を選択します。そうすると、すでにコンテキストに含まれている issue と計画の決定を使えます。

1. 次のプロンプトを入力してエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェントの一覧に表示されない場合があります。上記のコマンドを入力すると有効になります。

2. 次のプロンプトで、QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 作業が終わったら、レポートを読みます。

## まとめと次のステップ

再利用可能な専門的な役割をワークフローに加え、エージェントの作業をレビューしました。このレッスンでは、次のことを行いました。

- カスタム エージェントが指示、スキル、MCP ツールとどのように連携するかを確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、絞り込み issue に照らして結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次は、[機能 PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
