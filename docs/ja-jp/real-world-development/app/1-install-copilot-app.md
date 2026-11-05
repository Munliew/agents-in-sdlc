---
title: "レッスン 1 - GitHub Copilot app をインストールする"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースとクイックチャットを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェント主導の開発に使うデスクトップアプリです。GitHub Copilot CLI を基盤として GitHub とネイティブに統合されているため、リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。複数のエージェントを個別の分離されたワークスペースで並行して動かし、作業を指示したり、繰り返し作業を自動化したりできます。Node.js のインストールとプロジェクトの準備ができたので、次はアプリをインストールしてリポジトリを接続します。

このレッスンでは、次のことを行います。

- GitHub Copilot app をインストールしてサインインします。
- GitHub リポジトリからプロジェクトをアプリに追加します。
- テンプレートから作成したバックログを含め、ワークスペースを確認します。
- クイックチャットを試し、アプリについて質問します。

## シナリオ

チームは、増え続けるバックログに対応するために AI エージェントを導入しています。Copilot app では、issue の選択、エージェントの実行、変更のレビュー、プルリクエストのマージを一か所から指示できます。このレッスンでは、アプリをインストールして接続し、プロジェクトについての会話を始めます。

> [!NOTE]
> 対象の Copilot プランが必要です。Copilot Free、Copilot Student、またはいずれかの有料プラン (Pro、Pro+、Business、Enterprise) を利用してください。Copilot Business または Copilot Enterprise では、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。

## GitHub Copilot app をインストールして構成する

GitHub Copilot app は Windows、macOS、Linux で利用できます。アプリをインストールして認証し、Tailspin Toys リポジトリを追加します。

1. ブラウザーで [GitHub Copilot app のランディングページ][download-app]を開きます。
2. 使用しているプラットフォーム向けのアプリをダウンロードし、ページの手順に従ってインストールします。
3. インストール後にアプリを開きます。
4. **Sign in to GitHub** を選択し、表示される手順に従って認証します。データ所在地を設定した GitHub Enterprise Cloud を使う場合は、**Use GitHub Enterprise** を選択し、求められたら `*.ghe.com` ホスト名を入力します。GitHub Enterprise Server はサポートされていません。
5. オンボーディングでリポジトリの選択が表示されたら、テンプレートから作成した Tailspin Toys リポジトリを選択します。リポジトリ選択をスキップし、オンボーディング後に追加することもできます。
6. テーマを選択するよう求められたら選び、オンボーディングを完了します。
7. サイドバーの **Sessions** でリポジトリを確認します。すでに一覧にあれば再追加は不要です。
8. リポジトリが接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選択します。前提条件のレッスンで控えた所有者名とリポジトリ名を指定します。
9. リポジトリがサイドバーに表示されることを確認します。このレイアウトでは、`tailspin-toys` などのリポジトリフォルダーが **Sessions** の下にまとまり、その下に個々のセッションが表示されます。

## ワークスペースを確認する

プロジェクトを接続したら、サイドバーの各領域を確認します。

- **New** - Copilot との新しいチャットセッションを開始します。
- **Pull requests** - アプリの GitHub 統合からプルリクエストを参照して絞り込み、CI 状態を確認し、レビューします。
- **Issues** - issue を参照して絞り込み、issue からセッションを開始します。
- **Automations** - スケジュールまたはオンデマンドで実行するエージェントタスクを保存します。やることリストや定期的な保守などを任せられます。まとめで次のステップとして紹介しますが、ワークショップ中の追加演習ではありません。
- **Customize** - MCP サーバー、プラグイン、スキルなどを追加します。Playwright MCP の設定で使います。
- **Chats** - 独自のブランチやワークスペースを必要としない、質問やブレインストーミングのための軽量な会話です。最初のチャットを開始すると表示されます。表示されていない場合は、**Sessions** の横にある **+** を選択して最初のチャットを始めます。
- **Sessions** - リポジトリのフォルダーと、そのエージェントセッションをまとめます。リポジトリ名 (例: `tailspin-toys`) を選択するとリポジトリのビューが開き、その下のセッションを選ぶと会話に戻ります。セッションごとに分離されたワークスペースを使うため、変更を競合させずに複数を同時に実行できます。最初のセッションは次のレッスンで開始します。

ワークショップを進めながら、ワークスペースをさらに確認していきます。

> [!TIP]
> 操作方法が分からない場合や、何かが可能か知りたい場合は、Copilot に質問して案内を受けてください。

### 用意されたバックログを確認する

テンプレートからリポジトリを作成したときに追加されたバックログを確認します。

1. サイドバーの **Sessions** で、個別のセッションではなく Tailspin Toys のリポジトリフォルダーを選択します。リポジトリ上部のナビゲーションで **Issues** を選択します。
2. issue 番号ではなく、次のタイトルを探します。
   - Allow users to filter games by category and publisher
   - Update our repository coding standards
3. issue を選択して詳細を読みます。issue からエージェントセッションを開始できます。最初の小さな変更を終えた後、フィルタリング issue から作業を始めます。

> [!NOTE]
> **Issues** と **Pull requests** の一覧には、Copilot app に追加したリポジトリの項目だけが自動的に表示されます。ほかのリポジトリの作業項目を見るには、そのリポジトリもアプリに追加してください。

## クイックチャットを試す

アプリ自体について質問すると、アプリに慣れることができます。**quick chat** では、ブランチや worktree を作らずに質問やブレインストーミングができます。その場限りの質問に適しており、セッションは必要ありません。

1. サイドバーの **Chats** の横にある **+** を選択して、新しいチャットを開きます。**Chats** がまだ表示されていない場合は、**Sessions** の横にある **+** を選択して最初のチャットを開始します。開始後に **Chats** が表示されます。
2. アプリのセッションの動作を尋ねます。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。各セッションが分離された git worktree で実行されるため、変更が競合せずに複数のエージェントを並行実行できることが分かります。会話は続けることも、新しいチャットを始めることもできます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続して、ワークスペースを確認しました。次のことを行いました。

- アプリをインストールして GitHub にサインインしました。
- GitHub リポジトリからプロジェクトを追加しました。
- ワークスペースを確認し、**Issues** で用意されたバックログを見つけました。
- クイックチャットで、その場限りの質問をしました。

次は、[最初のエージェントセッションを開始し][next-lesson]、ゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の概要][getting-started]
- [GitHub Copilot app でのエージェントセッションの操作][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
