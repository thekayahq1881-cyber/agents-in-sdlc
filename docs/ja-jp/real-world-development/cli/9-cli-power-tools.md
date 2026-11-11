---
title: "レッスン 9 - GitHub Copilot CLI の便利なスラッシュ コマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、任意のクラウドへの委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

一般的な CLI ツールと同じように、GitHub Copilot CLI にも多くのスラッシュ コマンドが用意されています。これらのコマンドを使うと、高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。ほかの便利なコマンドも確認しましょう。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` を使って、利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認します。

## シナリオ

CLI の主なワークフローを終えました。次は、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組むと、モデルのコンテキスト ウィンドウの上限に近づくことがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。スラッシュ コマンドを使って、コンテキストを自分で確認したり圧縮したりすることもできます。

1. Codespace に戻り、まだ起動していない場合はリポジトリのルートから Copilot CLI を起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどう配分されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. 再度 `/context` を入力し、結果を比較します。会話がすでに短い場合は、大きな違いが見られないことがあります。

> [!NOTE]
> コンテキスト ウィンドウが埋まると、Copilot CLI は自動的にコンテキストを圧縮します。実行するタイミングを選びたい場合は `/compact` を使います。関係のない別のタスクに切り替えるために新しい会話を始める場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルを一覧表示し、使用するモデルを選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力して学んだことを共有すると、AI ツールの活用方法を互いに改善できます。`/share` コマンドでは、セッションを Markdown または HTML ファイルにエクスポートしたり、共有可能なリンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みのバージョンにある `/share` のオプションを確認します。
2. セッションを共有する場合は、目的に合った保存先を選択します。たとえば `/share file` はローカルの Markdown ファイルにエクスポートします。
3. エクスポートした内容を誰かに送ったり公開したりする前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有するつもりのないリポジトリや会話の内容は公開しないでください。

## 任意: 作業を拡大する、または委任する

このワークショップの主要な内容は完了しました。Copilot CLI には、大規模なタスクに役立つコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割し、並行して実行できます。
- `/worktree` は、別のタスク用に独立した Git worktree を作成できます。
- `/delegate` は、非同期で作業し、プルリクエストを作成することがある Copilot cloud agent にタスクを送信できます。

追加の worktree やリモート作業が発生するため、これらのコマンドは任意です。試すときは、範囲を明確にした新しいタスクから始め、通常のワークフローに沿って結果をレビューしてください。非同期エージェントの作業について詳しく学ぶ場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使うと、構成の変更、セッションの共有、内部動作の確認ができます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` を使って、利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認しました。

Copilot CLI には、まだ多くのスラッシュ コマンドや機能があります。最後に、[学んだ内容を振り返り][next-lesson]、さらに学ぶための次のステップを確認しましょう。

## リソース

- [Copilot CLI のコマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
