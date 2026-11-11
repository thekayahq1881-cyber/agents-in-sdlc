---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、絞り込み機能の検証に使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことだけが開発ではありません。手動でコードを検証し、instruction file を使って標準に従うようにしました。では、テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

このようなタスクには、**エージェント スキル**が適しています。スキルは、こうした操作を正しく実行する方法を Copilot が理解するのに役立ちます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果のレポート形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys は、ユニット テスト、lint、型チェックに `quality-checks` スキルを使っています。チームでは、結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェント スキルは、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースをまとめたもので、エージェントが必要に応じて読み込みます。基本的には、スキル名と同じ名前のフォルダーに `SKILL.md` という Markdown ファイルを置きます。Markdown には、スキルの名前と説明を定義する frontmatter、スキルの概要、呼び出すべき状況のガイダンスを記載します。スキルのフォルダーには、呼び出されたときに使用するスクリプトやほかのリソースを格納するサブフォルダーを追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトの既存コマンドを使うため、`SKILL.md` だけで構成されています。

スキルはプロジェクトの `.github/skills` フォルダーに置くと、チームで共有して再利用できるリポジトリの資産になります。また、ユーザーのスキル フォルダー `~/.copilot/skills` に置くこともできます。

## スキルを確認する

Tailspin Toys チームがユニット テスト、lint、型チェックの実行に使う `quality-checks` スキルを確認しましょう。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 冒頭にある `name` と `description` を読みます。説明によって、Copilot はスキルを呼び出す状況を判断します。
3. 指示を読み、テストと lint の作業にどのようにガイダンスを与えているかを確認します。
4. このスキルに、**Results output formatting** セクションがまだないことを確認します。

## 変更する前にスキルを実行する

Copilot CLI から直接、または自然な言葉で依頼して、スキルを実行できます。Copilot に 3 種類のチェックを実行してもらいましょう。

1. Interactive モードで絞り込みの会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

どのチェックを実行したか、成功したか、ツールが実際に報告した内容を把握できる、より良いレポートが必要です。スキルを更新してその形式でレポートを作成しましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を加えたので、実際に試してみましょう。Copilot CLI は会話を再起動せずに、編集されたスキルを再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどとまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして使いました。このレッスンでは、次のことを行いました。

- ユニット テスト、lint、型チェック用の `quality-checks` スキルを確認しました。
- 結果のレポート形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、機能 PR で絞り込み機能と一緒に扱います。次は、[Playwright MCP サーバーを使って Copilot がサイトを直接操作できるようにします][next-lesson]。

## スキルの例

以下のコミュニティの例は参考資料です。追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図と同梱のエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
