---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、絞り込み機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合は、Copilot がそのガイダンスを参照できるようにします。[Instruction file][instruction-files]には、作成したいコードだけでなく、その構成方法も記述できます。絞り込み機能を作成したので、Copilot が使った指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示と、パスを指定して適用する instruction file がエージェントにどのように伝わるかを確認します。
- コーディング標準が守られるように instruction file を更新します。
- instruction file がコードに与える影響を確認します。

## シナリオ

多くの優れた開発チームと同じように、Tailspin Toys にも開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントではコードを言い換えるのではなく、意図や自明ではない判断を説明します。
- `db/` と `src/lib/` にある export された関数では、`db` 引数を注入できる場合はその点も含め、目的、パラメーター、戻り値を TSDoc/JSDoc で記述します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を文書化し、関連するコードの変更に合わせてコメントを最新の状態に保ちます。
- 既存の書式設定と lint のガイダンスを維持します。

Instruction file を使うと、強調したプラクティスに沿ってタスクを進めるために必要な情報を Copilot に伝えられます。

## Instruction file

カスタム指示を使うと、コンテキストや設定を Copilot に伝え、コーディング スタイルや要件を理解しやすくできます。適切な提案やコード スニペットを得るために、好みのコーディング規約、ライブラリ、追加したいコメントの種類などを指定できます。リポジトリ全体に適用する指示も、特定のファイル種別に適用するタスク レベルの指示も作成できます。

Instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送られる単一の instruction file です。ほとんどのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスクのガイドラインを記載します。

> [!NOTE]
> ほかの形式の指示とそのサポート状況は、環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示を確認する

スターター プロジェクトには、作業を始めるための instruction file がすでに用意されています。変更を加える前に、どのような指示があるかを確認しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡潔な説明とコーディングに関するガイダンスを確認します。このリポジトリで Copilot を使うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドには、指示の適用対象を決める glob が設定されています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの instruction file やプロジェクト内の既存ファイルへのリンクを確認します。大きな指示を小さく再利用しやすい単位に分け、Copilot が参照する例を示せます。

## チームのガイダンスに合わせて instruction file を更新する

既存ファイルは出発点として十分ですが、まだ不足があります。中心となる `copilot-instructions.md` を編集し、新しく生成される TypeScript に TSDoc コメントを追加するようにします。

1. `.github/copilot-instructions.md` で **Code formatting guidance** という見出しのセクションを探します。35 行目付近にあります。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新した指示を適用し、機能のコンテキストを保つために、絞り込みの会話を終了して再開します。

1. 絞り込みの会話を続けている場合は `/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートに移動し、`game-filters-cli` ブランチで保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   指示が表示されたら、絞り込みの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に、更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが生成され、コードの内容を正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が instruction file からコンテキストを取得する方法を確認し、機能に新しい標準を適用しました。具体的には、次のことを行いました。

- リポジトリの指示と、パスを指定して適用する instruction file がエージェントにどのように伝わるかを確認しました。
- コーディング標準が守られるように instruction file を更新しました。
- instruction file がコードに与える影響を確認しました。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
