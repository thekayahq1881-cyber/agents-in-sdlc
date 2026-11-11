---
title: "演習 5: GitHub Copilot の作業を反復的に改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善や機能の追加に関するいくつかのタスクで、GitHub Copilot を使ってきました。コードにドキュメントを追加し、デザイン チーム向けに関連ゲーム機能を構築し、ハイ コントラストとライト モードの切り替えなどのアクセシビリティ機能を実装するよう依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを伝え、作業を改善しましょう。レビューをチームに有効化する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot の提案を確認します。

### シナリオ

これまでに何度も説明したように、生成 AI を利用しても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。まず GitHub Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能をレビューしてから、チームのほかのメンバーにもレビューを有効にします。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期かつ監督者なしでタスクを実行するため、安全を保つためにいくつかの制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みアクセスは作業に使うブランチ**のみ**に限定されます。
- Cloud agent は GitHub Actions 内で実行され、作業用の独立した一時環境を作成します。
- GitHub Actions のワークフローを実行するには、人間による承認が必要です。
- MCP サーバーなど、[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent が作成した、コードにドキュメントを追加するプルリクエスト (PR) を確認します。github.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot は MCP サーバーへの外部呼び出しを含む外部リソースへのアクセスが既定で制限されているため、これは**想定どおり**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリを開きます。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** の表示があります。その場合は、Copilot の作業が終わるまで待ちます。数分かかることがあります。その間に休憩したり、学んだことを振り返ったりしてください。

4. プルリクエストの準備ができたら、**Files changed** タブを選択して変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc doc comment やその他のドキュメントを含む、更新されたコードを確認します。具体的な変更内容は状況によって異なります。

   内容を確認しながら、TSDoc doc comment、TypeScript の規約、コメント ヘッダーを探します。これらは演習 2 の最初に確認したカスタム指示によるものです。

6. 更新内容に問題がないことを確認したら、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す通知を確認します。
8. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローがキューに入ったことが表示されます。問題がなければ、単一の Astro アプリに対するプロジェクト チェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業するのは、一方通行ではありません。チーム メンバーに依頼するように、プルリクエストのコメントやコードのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は確定的ではないため、必ず成功する指示文は示せません。Copilot に依頼する更新の例を挙げます。

- 各コード ファイルの先頭に、ファイルの内容を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript ファイルと Astro ファイルに TSDoc doc comment を追加する。
- Astro アプリの構成を説明する README を作成する。

1. **@copilot** をメンションし、生成されたドキュメントへの変更を依頼するコメントを追加します。上記の例を使うか、コードベースに必要なドキュメントについて別の変更を依頼します。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択し、変更をレビューします。

納得できるまで Copilot に依頼を続けてください。完了したら、PR をドラフトからレビュー可能な状態に変更して、main ブランチにマージできます。

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue に対して、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す通知を確認します。
7. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄にワークフローがキューに入ったことが表示されます。問題がなければ、単一の Astro アプリに対するプロジェクト チェックが成功します。完了まで数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を実装しました。ほかの作業と同様に、Copilot cloud agent に更新を依頼しながら反復的に改善できます。たとえば、表示する関連ゲームの数を調整したり、コメント ヘッダーや TSDoc doc comment を追加したりできます (この issue は、カスタム指示を更新する**前**に割り当てたことを思い出してください)。**Conversation** タブにコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントを使って実装した機能を確認します。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 の mission control で追加を依頼したライト モードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストに **[WIP]** の表示があります。その場合は、Copilot の作業が完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - モードを切り替える UI コンポーネント
   - ユーザー設定を保持するための local storage の使用
   - ハイ コントラスト モードとライト モードの CSS またはスタイルの変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モード切り替えを管理する JavaScript または TypeScript コード

6. 更新内容を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す通知を確認します。
8. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローがキューに入ったことが表示されます。問題がなければ、単一の Astro アプリに対するプロジェクト チェックが成功します。完了まで数分かかることがあります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   その後、アプリケーションを起動して、ブラウザーでハイ コントラスト モードとライト モードを切り替えます。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティ カスタム エージェントが、アクセシビリティのベスト プラクティスに沿った機能実装をどのように支援したかを確認しましょう。アクセシビリティに関する懸念や改善点があれば、前の PR と同じようにコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 作業をさらに委任する

Cloud agent は、実際のバックログの項目を任せて別の作業に集中するときに役立ちます。習慣にするため、リポジトリでいくつか issue を作成して Copilot に割り当ててみましょう。次のようなタスクを試せます。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセス helper に入力検証とエラー処理を追加する。

## まとめ

Cloud agent の環境を完了しました。各レッスンで、次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し**、後で cloud agent の出力に反映される様子を確認できるようにしました。
- **issue を Copilot cloud agent に割り当て**、環境のセットアップ、計画、非同期の作業を確認しました。
- アクセシビリティ用のカスタム エージェントを作成して使い、ハイ コントラストとライト モードの切り替えを追加しました。
- Copilot agents ページを mission control として使い、作業中にアクセシビリティ セッションを監視して調整しました。
- cloud agent のプルリクエストをレビューして反復的に改善し、変更を依頼するために **@copilot** をメンションし、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent の環境を完了しました。引き続き学習する場合は、ほかの環境で次の内容を試せます。

- 🖥️ **[VS Code 環境](../../vscode/)** — IDE で直接 Copilot Chat のエージェント モードと MCP 統合を試します。
- 💻 **[CLI 環境](../../cli/)** — Plan モードと Autopilot モード、エージェント スキル、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` 連携を使い、レビュー済みの変更をターミナルから 2 つ届けます。

自分のリポジトリでは、次のことを試してください。

- リファクタリングやテスト カバレッジの issue を cloud agent に割り当てる。
- 別の分野向けのカスタム エージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
