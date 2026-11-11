---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot が知っておくべき背景情報がある場合は、その情報を参照できるようにします。[Instruction file][instruction-files]を使うと、Copilot に依頼する内容だけでなく、その進め方も伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示と、パスを指定して適用する instruction file によって、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどのように伝わるかを確認します。
- `.github/copilot-instructions.md` に、リポジトリ全体に適用する新しい標準を追加します。

> [!NOTE]
> VS Code や CLI の環境とは異なり、ここではプロンプトの実行前後を比較しません。Copilot cloud agent は GitHub issue に対して非同期で動作するため、影響をリアルタイムに並べて示すのは困難です。cloud agent が作成したプルリクエストをこの環境の後半でレビューするときに、instruction file の効果を確認できます。

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

このリポジトリに含まれる instruction file を確認します。中心となる `copilot-instructions.md` が 1 つあり、さまざまなタスク向けの `*.instructions.md` ファイルもあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明や、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** にある **GitHub Actions Workflows** のガイダンスにも注目してください。これらの指示は Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ レイヤー、テストなどに関する指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドには、指示の適用対象を決める glob パターンがリポジトリのルートからの相対パスで指定されています。この例では、`**/*.test.ts` に一致するファイルなど、TypeScript のテスト ファイルが対象です。
5. このプロジェクトのユニット テスト作成に関する指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、一番下までスクロールします。`unit-tests.instructions.md` などの別の instruction file や、プロジェクト内の既存ファイルへのリンクを確認します。大きな指示を小さく再利用しやすい単位に分け、Copilot がコードを生成するときに参照する例を示せます。(記載されているパスはリポジトリのルートではなく、instruction file からの相対パスです。)

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc の doc comment とファイル コメント ヘッダーのルールを追加します。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクト レベルの情報を伝えるファイルです。コード提案の質を高めるため、リポジトリのコーディング標準を記載します。

このレッスンで行う編集は 1 つだけです。コード内のドキュメントを TSDoc の doc comment とファイル レベルのコメント ヘッダーで記述する、プロジェクト全体のルールを追加します。cloud agent は、この環境の後半で割り当てる issue に取り組むときに、このルールを読み取ります。

編集する前に、作業用のブランチを作成します。cloud agent は issue の対象ブランチから指示を読み取りますが、変更を整理してコミットします。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` を開き、次の手順に従います。

1. **Code formatting requirements** セクションを探します。27 行目付近にあるはずです。コーディング標準は記載されていますが、コード内ドキュメントのルールがありません。そのため、生成された helper には doc comment がありませんでした。
2. 既存の標準のすぐ下に次の Markdown 行を追加し、ファイル コメント ヘッダーと TSDoc doc comment を追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

3. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、Instruction file は全体的なガイダンスを記載するリポジトリ レベルの `.github/copilot-instructions.md` と、特定の言語、ファイル種別、タスク向けの `*.instructions.md` のどちらにも作成できます。今回追加した doc comment のルールのように、プロジェクト全体に適用する標準にはリポジトリ レベルのファイルが適しています。

## 指示の変更をコミット、プッシュ、マージする

Cloud agent は、issue の対象ブランチにある instruction file を読み取ります。次の演習で issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が指示を使うには、変更を `main` に反映する必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを作成し、マージします。次の演習で cloud agent が作業するときに、この指示を読み取れるようになります。

> [!TIP]
> このワークショップで `main` ブランチに直接作業する場合は、ブランチ作成を省略して `main` に直接コミットできます。ここでは、実際のプロジェクトで行う方法に近づけるためにブランチを使います。

## まとめと次のステップ

このプロジェクトで Copilot が instruction file からコンテキストを取得する方法を確認し、リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。この環境の残りのレッスンで cloud agent が作成するプルリクエストを確認すると、この標準が反映されていることがわかります。

次は、[Copilot cloud agent に最初の issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う instruction file][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタム指示を書く 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
