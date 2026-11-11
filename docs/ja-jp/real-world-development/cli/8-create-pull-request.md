---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "絞り込み機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

絞り込みの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、すべて 1 つのブランチに保存されています。まとめてレビューして、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は自分でマージしました。今回は **Agent Merge** にマージの作業を任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは進行を簡単にするため、絞り込み機能と品質確認の作業を 1 つの会話とブランチで行い、1 つの PR にまとめました。

このレッスンでは、次のことを行います。

- Agent Merge とは何か、マージまでのライフサイクルをどのように自動化するかを学びます。
- 機能全体の変更と検証の根拠を確認します。
- 絞り込み機能の PR を作成します。
- レビューの後に Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

絞り込み機能のワークフローでは、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、マージを許可する権限は開発者が保持したまま、PR に関する残りの作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、プルリクエストをマージするまでに必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題 (失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要に応じた rebase など) に取り組みます。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその役割を任せることもできます。Agent Merge を有効にする前に、作業をレビューし、準備ができていることを確認してください。

## Agent Merge で PR を管理する

コードの作成が終わったので、まとめてレビューし、PR を作成して、残りの作業を Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視します。すべて成功すると、マージを実行します。
7. Copilot が「PR #14 was squash-merged successfully.」のようなメッセージを表示したことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリの設定、権限の不足を回避しません。ブロックされた場合は、報告された理由を確認し、リポジトリで許可されている場合はレビュー済みのマージを手動で完了します。

## まとめと次のステップ

コードの生成、テスト、検証、プルリクエストのプロセスなど、開発プロセスの複数の部分を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge とは何か、マージまでのライフサイクルをどのように自動化するかを学びました。
- 機能全体の変更と検証の根拠を確認しました。
- 絞り込み機能の PR を作成しました。
- レビューの後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に使える[便利な Copilot CLI のスラッシュ コマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI のコマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
