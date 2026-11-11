---
title: "レッスン 0 - 前提条件"
description: "GitHub Copilot app のレッスンに向けて、Node.js をインストールし、Tailspin Toys プロジェクトの自分用コピーをテンプレートから作成します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

GitHub Copilot app は、Copilot と GitHub を一元的に扱うデスクトップ アプリです。issue やプルリクエストにすばやくアクセスでき、GitHub Copilot を使った開発もできます。このワークショップでは、自分のコンピューターで GitHub Copilot app を使い、Astro で構築された Tailspin Toys アプリを更新します。始める前に、Node.js がローカルにインストールされていることを確認し、次に Copilot app をインストールします。

このレッスンでは、次のことを行います。

- Tailspin Toys プロジェクトのテストを自分のコンピューターで実行できるように Node.js をインストールします。
- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成します。

## Node.js をインストールする

いくつかのレッスンでは、エージェントに機能を実装させ、Tailspin Toys のテスト スイートをローカルで実行します。そのためには **[Node.js][nodejs]** が必要です。プロジェクトに必要なランタイムは Node.js だけです。現在の **LTS** リリースをインストールしてください。

どのプラットフォームでも、公式インストーラーを使うのが最も簡単です。

1. Windows Terminal、macOS のターミナル、または普段使っているターミナルを開きます。
2. 次のコマンドを実行し、インストールされている Node.js のバージョンを確認します。

   ```shell
   node --version
   npm --version
   ```

3. どちらのコマンドも成功し、Node.js がプロジェクトの README と `package.json` に記載された要件を満たしている場合は、次のセクションに進めます。

> [!TIP]
> これらの手順は、Node.js がインストールされていない場合や、更新が必要な場合にのみ実行してください。

4. [Node.js のダウンロード ページ][node-download]を開きます。
5. 使用しているオペレーティング システム向けの **LTS** ビルドをダウンロードします。
6. インストーラーを実行し、既定の設定を選択します。Windows では **Add to PATH** を選択したままにします。
7. インストール後、新しいターミナル ウィンドウを開きます。
8. 新しいターミナルで次のコマンドを実行し、インストールを確認します。

   ```bash
   node --version
   npm --version
   ```

9. インストールされた Node.js と npm のバージョンが表示されます。

> [!IMPORTANT]
> Copilot app のエージェントも `node --version` と `npm --version` を実行できる必要があります。普段使うターミナルで成功しても、アプリから利用できるとは限りません。Node.js のインストール後や PATH を変更した後は、アプリを完全に終了してから再起動してください。セッションでどちらかのコマンドを実行できない場合は、続行する前に[セッション環境のトラブルシューティング手順][session-environment]を行ってください。

> [!IMPORTANT]
> E2E チェックには、各 worktree にプロジェクトの依存関係と Playwright Chromium も必要です。worktree の準備では Tailspin Toys リポジトリの README に従い、インストールの要求は内容を確認してから承認してください。

## ラボ用リポジトリを設定する

Tailspin Toys プロジェクトの自分用コピーを使います。[テンプレート リポジトリ][template-repository]から作成してください。新しいリポジトリにはラボに必要なファイルがすべて含まれます。アプリをインストールするときに、このリポジトリを接続します。

1. 新しいブラウザー ウィンドウで、このラボの GitHub リポジトリ `https://github.com/github-samples/tailspin-toys` を開きます。
2. ラボ用リポジトリのページで **Use this template** ボタンを選択し、次に **Create a new repository** を選択して、自分用のコピーを作成します。

   ![ドロップダウンで Create a new repository が選択された Use this template ボタン](../../../_images/app-0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの案内に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成できます。

   ![github-samples/tailspin-toys がテンプレートに設定され、リポジトリ名が入力された Create a new repository フォーム](../../../_images/app-0-create-repository.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。ラボの後半で参照します。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub issue のバックログも自動的に作成されます。ワークショップ全体を通してこの issue を使うため、自分で起票する必要はありません。

ワークショップ テンプレートの新しいコピーを使ってください。リポジトリの指示、アプリケーション コード、テスト、`quality-checks` スキル、既存のキャンバス拡張機能が含まれています。ワークショップではスキルをカスタマイズし、QA エージェントを作成します。以前作成したコピーを使う場合は、必要なファイルがあるかどうか、ファシリテーターに確認してください。

## まとめと次のステップ

準備ができました。このレッスンでは、次のことを行いました。

- Tailspin Toys プロジェクトをコンピューター上でビルドおよびテストできるように Node.js をインストールしました。
- テンプレートから Tailspin Toys リポジトリの自分用コピーを作成しました。

次は、[GitHub Copilot app をインストールし][next-lesson]、作成したリポジトリを接続して、ワークスペースを確認します。

## リソース

- [Node.js をダウンロードする][node-download]
- [テンプレートからリポジトリを作成する][template-repository]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#issue-からセッションを開始する
