---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、基本を確認して、用意されたフィルタリング issue を探します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] はターミナルで動作するエージェント型コーディングアシスタントです。コードベースの調査、コードの生成、コマンドの実行、外部ツールとのやりとりをコマンドラインから行えます。タスクを任せたり、変更を依頼したりしながら、作業に集中できます。まずツールをインストールしましょう。使い慣れたツールでインストールできます。

このレッスンでは、次のことを行います。

- npm で GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub Model Context Protocol (MCP) サーバーを使って、フィルタリング issue を探します。

## シナリオ

チームは、増え続けるバックログに対応するため AI エージェントを使い始めています。Copilot CLI は、多くの開発者が日常的に使うターミナルでその機能を利用できるようにします。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで使える状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められた場合は、デバイスコードの手順に従って認証し、Copilot CLI を許可します。
3. Copilot CLI は次の確認を表示します。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して信頼します。

> [!NOTE]
> Codespace では GitHub セッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めてください。

## 基本を確認する

通常のシェルプロンプトで入力したコマンドは Codespace で実行されます。Copilot CLI を起動すると、自然な言葉でエージェントに依頼し、スラッシュコマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力してインストール済みバージョンのコマンドを確認し、<kbd>Esc</kbd> を押してヘルプを閉じます。
3. 次の質問で Copilot が動作することを確認します。

   ```plaintext
   このプロジェクトの重要なファイルを教えてください。
   ```

4. 回答を読み、Copilot がリポジトリを調べてから答えていることを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用可能であることを確認します。
6. 次のプロンプトでフィルタリング issue を探します。

   ```plaintext
   GitHub MCP を使って、このリポジトリから "Allow users to filter games by category and publisher" というタイトルの issue を探してください。URL と短い概要を教えてください。何も変更しないでください。
   ```

7. URL を開いて issue を読みます。最初の小さな変更の後に使用します。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチを使い、自動的には worktree を作成しません。変更ごとに機能ブランチを作成します。

## ワークショップ用のショートカットを使う

通常、Copilot CLI は許可された範囲外のツールを使う前に確認を求めます。このワークショップでは `--yolo` を付けて再起動します。Codespace 内で承認プロンプトを省略できるため、演習に集中できます。

> [!CAUTION]
> `--yolo` は `--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls` を含む完全な自動権限を有効にします。Codespace や VM などの分離された環境でのみ使い、日常の開発で既定の alias にしないでください。詳細は[ツールの利用を許可または拒否する][allow-all-warning]を参照してください。

このワークショップでは `--enable-all-github-mcp-tools` を付けて、後のレッスンで issue やプルリクエストを操作するための GitHub MCP の読み取り・書き込みツールを有効にします。Codespace からローカルコンピューターへのアクセスは制限されますが、認証済みの GitHub リソースは実際のものです。公開やマージの前に変更をレビューしてください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて簡単な質問をし、会話が動作することを確認してから `/exit` で終了します。

Copilot は会話を自動的に保存します。後のレッスンで指示を変更したりエージェントを追加したりした後、`copilot --resume` を使って同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- npm で GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーを使って、フィルタリング issue を探しました。

次は、ゲームカードに星評価を表示する[最初の対象を絞った変更][next-lesson]を行います。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
