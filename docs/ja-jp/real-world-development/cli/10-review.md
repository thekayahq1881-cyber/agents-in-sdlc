---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、その他のリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys で一連のワークフローに取り組み、GitHub Copilot CLI を使いました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、事前に登録された絞り込み issue を見つけました。
- 星評価を追加し、転送したブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- 絞り込み issue から始めて、Plan モードで方法を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーで絞り込み機能を試しました。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの確認を評価しました。
- 絞り込み機能の変更全体をレビューし、Agent Merge に絞り込み PR のマージを許可しました。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使えるスラッシュ コマンドを確認しました。

## リリースしたもの

このワークショップでは、更新済みの `main` からそれぞれ独自のブランチを作り、2 つの PR をマージしました。

1. **星評価:** 既存の `starRating` と未評価であることを示す表示を、ゲーム カードに追加します。
2. **絞り込みと品質ワークフロー:** 絞り込み機能を実装し、指示を更新して機能に適用し、`quality-checks` レポートをカスタマイズして、QA プロファイルと関連テストを追加します。

絞り込みの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔に進めるため、これらの作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザーの確認、MCP を通じた Copilot のブラウザー操作という、複数の方法でコードを確認しました。`quality-checks` スキルはユニット テスト、lint、型チェックを実行し、新しい形式でレポートしました。QA はこれらの結果を、要件とテスト範囲のレビューと合わせて PR の前に確認しました。

追加するテストは、実際に不足している範囲を補うものであるべきです。QA を実行した結果、新しいテストが不要と判断されることもあります。マージを許可する前に、コードと検証の根拠をレビューしてください。変更を加えた場合は、影響を受ける検証を再度行います。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業内容を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整してください。指示は標準を定め、スキルは繰り返し実行するタスクを説明し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。構築の前に方法を検討するときは **Plan**、範囲を絞った変更に参加しながら取り組むときは **Interactive**、範囲が明確なタスクを任せるときは **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。何を、なぜ、どのように作りたいかを明確に伝えることで、結果が大きく変わります。

## さらに確認する

主なワークフローを学びました。Copilot CLI には、次のような機能もあります。

- `/review` でコード レビュー エージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を相談し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを独立させます。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールも、使い続けることで上達します。実際のコード、趣味のコード、何年も作りたいと思っていた小さなアプリなどで試してみましょう。チームと学んだことを共有し、互いの経験から学んでください。そして、ドキュメントも引き続き確認しましょう。

ターミナルから Tailspin Toys を拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境も確認するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を参照してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI のコマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
