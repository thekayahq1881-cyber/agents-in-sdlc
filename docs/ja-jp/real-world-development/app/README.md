---
slug: ja-jp/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** は Copilot CLI を基盤とするデスクトップ アプリケーションです。エージェント主導の開発に必要な機能を、1 つの専用ワークスペースにまとめています。並列エージェント セッション、切り替え可能なセッション モード、共有キャンバス、GitHub issue とプルリクエストのネイティブな管理機能を備えています。さらに **Agent Merge** を使うと、プルリクエストのリベース、レビュー フィードバック、継続的インテグレーション (CI) の修正、マージを管理できます。

このワークショップでは、Tailspin Toys で一連のワークフローを体験します。

1. プロジェクトを準備し、アプリをインストールしてリポジトリを接続し、ワークスペースと事前に登録されたバックログを確認します。
2. 星評価の小さな変更を加え、ブラウザーでレビューして、最初のプルリクエスト (PR) を手動でマージします。
3. 絞り込み issue から始めて **Plan** モードでアプローチを決め、**Autopilot** モードで実装し、**Interactive** モードでレビューします。
4. リポジトリの指示を更新し、絞り込み機能の作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーで絞り込み機能を確認します。
7. 品質保証 (QA) カスタム エージェントを作成して、要件、テスト範囲、検証の証拠をレビューします。
8. 絞り込み機能の変更全体をレビューし、2 つ目の PR に Agent Merge を使います。
9. キャンバスを利用できるか確認し、Database Explorer を使って、リポジトリに保存するトリアージ キャンバスを作成してテストします。必要な機能を利用できない場合は、ブロッカーを記録し、読み取り専用の代替方法を使うか、まとめに進みます。

ワークショップでは、星評価と、指示の更新、スキルの更新、QA プロファイル、テストを含む絞り込み機能の 2 つの PR を作成します。どちらも更新済みの `main` から開始します。絞り込みと品質に関するワークフローは、1 つのセッション、worktree、ブランチを共有するため、各ツールを学びながら作業を積み重ねられます。最後のキャンバス演習では同じセッションを使い、PR ワークフローを繰り返さずに、共有サーフェスの作成とテストに集中します。

## レッスン

| レッスン | トピック | 説明 |
|--------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | Node.js をインストールして Tailspin Toys プロジェクトの自分用コピーを作成します |
| [1. Copilot app のインストール][ex1] | セットアップ | アプリをインストールしてプロジェクトを接続し、ワークスペースを確認します |
| [2. 星評価を追加する: 小さな改善][ex2] | 最初の変更 | 既存の評価と未評価の場合の表示を追加し、PR 1 をマージします |
| [3. エージェント モード: Plan と Autopilot][ex3] | エージェント モード | issue から機能を計画し、Autopilot で構築して、Interactive でレビューします |
| [4. カスタム指示で Copilot をガイドする][ex4] | コンテキスト | 指示を確認して更新し、絞り込み機能に適用します |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し可能なチェック | 既存のスキルを確認し、レポート形式を変更して実行します |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | Customize で MCP を構成し、絞り込みの動作を確認します |
| [7. QA エージェントを作成して使う][ex7] | 要件とテスト範囲 | 専門プロファイルを作成して選択し、最終検証の証拠を集めます |
| [8. 機能 PR を作成してマージする][ex8] | レビューとマージ | 絞り込み、指示、スキル、QA プロファイル、テストをレビューし、2 つ目の PR に Agent Merge を使います |
| [9. キャンバスを確認して作成する][ex9] | コラボレーション | Database Explorer を使ってから、リポジトリに保存するトリアージ キャンバスを作成してテストします |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、成果物、その他のリソースを確認します |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランのいずれかがある GitHub アカウント
- [ ] **macOS、Linux、Windows** のいずれかを実行するコンピューター
- [ ] コンピューターに[インストールされた Git][install-git]

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、どの環境でもすべての演習を完了できます。

> [!NOTE]
> Copilot app は Codespace ではなく自分のコンピューターで動作します。[前提条件の演習][ex0]では、アプリをインストールする前に Node.js をインストールし、プロジェクトの自分用コピーを作成します。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使う場合、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。これは Copilot CLI ポリシーとは別の設定です。

## はじめに

**[前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
