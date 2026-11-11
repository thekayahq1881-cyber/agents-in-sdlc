---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローにある特定のタスクや分野に合わせて、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、エージェントを定義します。焦点を絞った指示、ベスト プラクティス、コーディング パターン、分野固有の知識を指定すると、Copilot がその種類の作業をより効果的に進められます。チームの専門知識を再利用できるエージェントとしてまとめることもできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディング プラクティスに従うセキュリティ エージェント、一貫したテスト パターンを守るテスト エージェントなどです。

カスタム エージェントは、プロジェクト内の `.github/agents` フォルダーか、グローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、そのエージェントの動作、専門知識、指示を記述する Markdown プロンプトが必要です。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には、役割が重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。わかりやすく区別するなら、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、作業の一部としてスキルやほかのエージェントを調整するように設計されています。このラボのアクセシビリティ カスタム エージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業で、プルリクエスト ワークフロー スキルやテストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI を使うほかの場面と同じように、環境やシナリオに合わせてテストと調整を繰り返してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、カスタム エージェントを使って次のことを確認します。

- カスタム エージェントの定義方法
- タスクをカスタム エージェントに割り当てる方法

## シナリオ

Tailspin Toys は、見え方や視覚的な好みにかかわらず、すべての人がクラウドファンディング プラットフォームを利用できるように取り組んでいます。最近、暗いテーマではテキストと背景色のコントラストが不十分で、読みにくいというフィードバックがありました。このアクセシビリティ上の課題に対応するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、この機能をできるだけ早く実装します。そこで、カスタム エージェントを使って機能を作成します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。内容を確認し、Copilot に与えるガイダンスを把握しましょう。

Codespace に戻り、アクセシビリティ カスタム エージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドがある YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成するときの主な役割
   - アクセシビリティのベスト プラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題や間違い

## issue を作成して割り当てる

エージェントの mission control は、環境内のエージェントと作業する中心的な場所です。Copilot cloud agent にタスクを割り当て、進行状況を監視し、作業中に指示を変更したり追加したりできます。まず、ハイ コントラスト モードの作成タスクを Copilot に割り当てます。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

   ![カスタム エージェントと Accessibility が選択された Copilot cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に特化した GitHub Copilot の[カスタム エージェント][custom-agents]について学びました。カスタム エージェントを使うと、チームの専門知識や標準を再利用可能な形にまとめ、Copilot が特定の種類の作業を効果的に進めるためのガイダンスを提供できます。

このレッスンでは、次のことを確認しました。

- カスタム エージェントの定義方法
- タスクをカスタム エージェントに割り当てる方法

Copilot がハイ コントラスト モードを実装している間に、[mission control からエージェント セッションを監視して調整する][next-lesson]方法を確認しましょう。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使用する準備][org-custom-agents]
- [Enterprise でカスタム エージェントを使用する準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
