---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot が知っておくべき背景情報がある場合は、その情報を参照できるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。主な方法の 1 つが[Instruction file][instruction-files]です。コード生成のガイダンスを伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示と、パスを指定して適用する instruction file によって、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどのように伝わるかを確認します。
- *現在の*指示を使って、絞り込み機能の最初のデータ部分 (publishers helper) を生成します。
- `.github/copilot-instructions.md` に、リポジトリ全体に適用する新しい標準を追加します。
- 同じプロンプトをもう一度実行し、生成されたコードに新しい標準が反映される様子を確認します。
- 指示の更新と絞り込みの最初の変更を `main` にコミットし、次の演習で Copilot が更新したガイダンスを利用できるようにします。

> [!CAUTION]
> 生成されたコードが、設定した標準から外れる場合があります。Copilot は非決定的に動作します。この演習の目的は、指示の更新後に動作の傾向が変わることを確認することであり、出力を 1 文字ずつ一致させることではありません。

## Instruction file

### シナリオ

多くの優れた開発チームと同じように、Tailspin Toys にも開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データ レイヤーには、必ずユニット テストを用意します。
- UI はダーク モードに対応し、モダンな印象にします。
- コード内のドキュメントは TSDoc の doc comment として記述します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

Instruction file を使うと、強調したプラクティスに沿ってタスクを進めるために必要な情報を Copilot に伝えられます。

### カスタム指示

カスタム指示を使うと、コンテキストや設定を Copilot に伝え、コーディング スタイルや要件を理解しやすくできます。適切な提案やコード スニペットを得るために、好みのコーディング規約、ライブラリ、追加したいコメントの種類などを指定できます。リポジトリ全体に適用する指示も、特定のファイル種別に適用するタスク レベルの指示も作成できます。

Instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送られる単一の instruction file です。使用している技術スタック、開発中のものの概要、ベスト プラクティスなど、ほとんどのチャットや CLI のリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けに作成できます。TypeScript や Astro などの言語別ガイドライン、UI コンポーネントの作成やユニット テストの追加といったタスクのガイダンスを記載できます。

> [!NOTE]
> IDE で作業する場合、instruction file は Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コードを生成するときにリポジトリ レベルの instruction file と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> また、Copilot は [ほかの標準形式の instruction file][custom-instructions-support]もサポートしています。AGENTS.md や CLAUDE.md も利用できます。

### Instruction file を管理するためのベスト プラクティス

Instruction file の作成方法を詳しく説明することは、このワークショップの範囲外です。ただし、サンプル プロジェクトには代表的な方法が含まれています。要点は次のとおりです。

- `copilot-instructions.md` には、開発中のものの説明、プロジェクト構成、全体に適用するコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` は、ファイル種別 (ユニット テスト、Astro コンポーネント、データ レイヤー) や特定のタスクに関する具体的な指示に使います。
- 自然な言葉で明確に説明します。望ましいコードと望ましくないコードの例を示します。

Instruction file の作り方に唯一の正解がないのと同じように、AI の使い方にも唯一の正解はありません。試しながら、プロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な instruction file を用意しましょう。このプロジェクトのファイルを確認すると、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの指示が見つかります。
>
> Copilot に instruction file を生成してもらうこともできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、利用する環境ごとに方法が異なります。必要に応じて、各環境のレッスンで説明します。
>
> テンプレートや出発点を探していますか。[awesome-copilot][awesome-copilot] リポジトリには、instruction file、カスタム エージェント、その他のリソースが集められています。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示を確認する

このリポジトリに含まれる instruction file を確認しましょう。中心となる `copilot-instructions.md` が 1 つあり、さまざまなタスク向けの `*.instructions.md` ファイルもあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明や、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** にある **GitHub Actions Workflows** のガイダンスにも注目してください。これらの指示は Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ レイヤー、テストなどに関する指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドには、指示の適用対象を決める glob パターンがリポジトリのルートからの相対パスで指定されています。この例では、`**/*.test.ts` に一致するファイルなど、TypeScript のテスト ファイルが対象です。
5. このプロジェクトのユニット テスト作成に関する指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、一番下までスクロールします。`unit-tests.instructions.md` などの別の instruction file や、プロジェクト内の既存ファイルへのリンクを確認します。大きな指示を小さく再利用しやすい単位に分け、Copilot がコードを生成するときに参照する例を示せます。(記載されているパスはリポジトリのルートではなく、instruction file からの相対パスです。)

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc の doc comment とファイル コメント ヘッダーのルールを追加します。

## 変更用のブランチを作成する

ここではベスト プラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する*前に* Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。各手順で必要な場合に説明します。

カスタム指示の効果を見るため、まず現在の instruction file を使ってプロンプトを送信します。その後、指示を更新して同じプロンプトを再送信し、違いを確認します。

1. 前の演習のエディター タブをすべて閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、helper の配置場所を Copilot に知らせます。
3. Chat ビューのエージェント選択メニューで **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを確認してコードを更新します。helper とテストなど、複数のファイルに変更が及ぶことがあります。
6. 提案された helper が型付き関数として作成され、最初の引数に `db` クライアントを受け取り、型付きの出版社配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約によるものです。
7. 提案されたコードに TSDoc doc comment とファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc comment を追加することがあります。その場合も問題ありません。重要なのは、指示を更新した後に動作の一貫性が高まることです。

## リポジトリ全体に適用する標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を伝えるファイルです。コード提案の質を高めるため、リポジトリのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを探します。27 行目付近にあります。コーディング標準は記載されていますが、コード内ドキュメントのルールがありません。そのため、生成された helper には doc comment がありませんでした。
3. 既存の標準のすぐ下に次の Markdown 行を追加し、ファイル コメント ヘッダーと TSDoc doc comment を追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、instruction file は全体的なガイダンスを記載するリポジトリ レベルの `.github/copilot-instructions.md` と、特定の言語、ファイル種別、タスク向けの `*.instructions.md` のどちらにも作成できます。今回追加した doc comment のルールのように、プロジェクト全体に適用する標準にはリポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` をもう一度開き、Copilot が適切なファイルに注目するようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 今度は、提案されたファイルの先頭に次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しいプロジェクト標準に従うよう促し、次の演習で使う実際のコードに適用できました。

## 指示とブランチをコミットしてプッシュする

Instruction file もリポジトリのほかのファイルと同じように、ソース管理で管理します。変更をコミットしてリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示と helper の変更が含まれることを確認します。

   ```bash
   git status
   ```

3. ターミナルで次のコマンドを実行し、指示の更新と helper の基盤に関する、Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加ファイルが表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルで次のコマンドを実行し、ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の issue に関連付けたプルリクエストを作成します。手動で行うこともできますが、プロジェクトに接続済みの GitHub ツールを使って Copilot に依頼できます。Copilot に issue を検索させ、issue をクローズする PR を作成してマージします。

> [!NOTE]
> この演習では、Copilot に GitHub 上の操作を依頼します。プロジェクト テンプレートでは GitHub ツールがワークスペースにあらかじめ接続されています。Copilot が初めてツールを使うときに、VS Code で GitHub へのサインインを求められる場合があります。画面の案内に従って許可してください。この接続 (Model Context Protocol) については、後の演習で学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の更新と、新しい publishers helper の基盤の両方を PR に記載するようにします。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探して PR を作成します。
5. Copilot が代わりに GitHub 操作を実行する許可を求めたら、コマンドを確認し、問題がなければ **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージしてローカル ブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が代わりに GitHub 操作やシェル コマンドを実行する許可を求めたら、内容を確認し、問題がなければ **Allow** を選択します。

GitHub Copilot の支援で、プルリクエストを作成してマージできました。

## まとめと次のステップ

Copilot がこのプロジェクトの instruction file からコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示でコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` に、リポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映される様子を確認しました。
- 指示と helper の基盤を `main` にコミットし、次の演習で利用できるようにしました。

次は、エージェント モードで[指示を活用し][next-lesson]、Copilot にコードベース全体にわたる新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う instruction file][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタム指示を書く 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
