---
title: "演習 3 - Playwright Model Context Protocol (MCP) サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

agent mode でフィルタリング機能を構築しました。プルリクエストを作成する前に、ブラウザーで動作を確認します。アプリを手動で操作する代わりに、**Playwright MCP サーバー**を接続して、Copilot に実際のブラウザーを操作させます。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを提供する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にブラウザーでフィルタリング機能を手動テストさせます。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

agent mode は、エディターの外部にあるツールへ接続すると、さらに強力になります。Model Context Protocol (MCP) は、Copilot が外部ツールやサービスと通信するための標準的な方法です。

![agent mode の内部動作と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部ツールやサービスと通信できます。リソースを使って最新情報にアクセスし、ツールを使って開発者に代わって操作できます。

これらのツールやリソースには、AI エージェントと外部ツール・サービスの橋渡しをする MCP サーバーを通じてアクセスします。MCP サーバーは既存 API や npm パッケージなどのローカルツールとの通信を管理します。各サーバーは、エージェントが利用できるツールとリソースのセットを提供します。

よく利用される MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリ、issue、プルリクエストを管理する API にアクセスできます。新しいリポジトリの作成や既存リポジトリの更新などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を提供します。Web ページへの移動、フォームの入力、ボタンの選択などを実行できます。

ほかにもさまざまなツールやリソースに接続する MCP サーバーがあります。GitHub は、エコシステム内での発見や貢献を促すために [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーも他の依存関係と同様に扱ってください。利用前にソースコードを確認し、発行元を検証して、セキュリティ上の影響を検討します。信頼できる MCP サーバーだけを使い、機密情報や操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクトテンプレートに含まれています。そのため、前の演習で Copilot がバックログの issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub 認証を行います。

## Playwright MCP サーバーを追加する

次に 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が操作できるブラウザーを利用できるため、機能をテストできます。

1. `.vscode/mcp.json` の `github` と並べて `playwright` エントリを追加し、ファイルを次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーと異なり、`playwright` はローカルサーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` は、画面を表示せずにブラウザーを実行します。デスクトップ環境がない Codespace ではこの設定が必要です。

> [!NOTE]
> Tailspin Toys ではエンドツーエンドテストに Playwright を使うため、通常はブラウザーもインストール済みです。ブラウザーが見つからないと Copilot が報告した場合は、`npx playwright install chromium` を実行して再試行してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot がサーバーのツールを初めて使うときにサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** が表示されたら選択し、サーバーを起動して構成を確認します。
2. VS Code からサーバーを信頼するか尋ねられたら、構成を確認し、起動を許可します。

手動での起動は省略できます。その場合、次の手順で Copilot が初めてツールを使うときに VS Code が起動を促します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。開発者がアプリを操作する代わりに、エージェントがページを開き、移動して、フィルターを適用し、結果を確認して報告します。会話を離れずに機能を検証できます。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を利用します。ボタン、リンク、リスト項目など、ラベル付きの構造化された要素をエージェントが読み取ります。これは支援技術と同様の情報に基づくため、簡単な機能確認をアクセシビリティの基本確認にも活用できます。

サーバーが接続され、アプリが起動していることを確認したら、次のプロンプトで Copilot にブラウザー確認を依頼します。

1. 次のプロンプトを送信します。

   ```text
   Playwright MCP サーバーを使って、実行中の http://localhost:4321 にブラウザーでアクセスし、新しいゲームのフィルタリング機能を確認してください。

   1. ゲームページに移動し、一覧にあるゲームの数を確認します。
   2. カテゴリフィルターを適用し、そのカテゴリのゲームだけに一覧が更新されることを確認します。
   3. フィルターを解除し、パブリッシャーフィルターを適用して、そのパブリッシャーのゲームだけに一覧が更新されることを確認します。
   4. カテゴリとパブリッシャーのフィルターを組み合わせ、両方の条件が結果に反映されることを確認します。

   各手順で確認したことを報告し、想定どおりに動作しない点があれば指摘してください。
   ```

2. Copilot が Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告することを確認します。
3. 検証報告を issue の受け入れ条件に照らし合わせます。問題がある場合は追加質問をするか、PR 作成前にコード修正を依頼します。

> [!NOTE]
> テストの実行中はアプリが `http://localhost:4321` で起動している必要があります。次の手順で必要な場合の対処を行います。
>
> 1. 開発サーバーを停止している場合は、プロンプトを送信する前に再起動します。
> 2. Copilot がブラウザーを見つけられないと報告した場合は、`npx playwright install chromium` を実行し、もう一度試すよう依頼します。Copilot が初めて Playwright MCP サーバーを使うときは、ブラウザーをダウンロードする場合があります。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能の動作を確認できたら、チームのレビュー用にプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルを開き、Copilot が変更したファイルを確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![変更されたファイルを表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択し、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドライン インターフェイス (CLI) など、複数の方法で PR を作成できます。ここでは GitHub Copilot に関連する issue を検索させ、PR を作成して関連付けます。

1. Copilot Chat パネルを開き、**New Chat** を選択して新しいセッションを開始します。
2. エージェントの一覧で **Agent** が選択され、Copilot が GitHub ツールを使えることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   リポジトリでカテゴリとパブリッシャーによるフィルタリングに関する issue を探してください。現在のブランチから新しいプルリクエストを作成し、正しい issue に関連付けてください。
   ```

4. Copilot が情報を収集して操作を行うとき、必要に応じて **Continue** を選択して許可します。GitHub ツールを初めて使うときは GitHub 認証を求められる場合があります。表示される手順に従います。
5. Copilot が issue を検索し、適切なものを見つけて PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択して PR を確認します。まだマージしないでください。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを提供する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- リリース前にブラウザーで機能を手動テストしました。
- ブランチを公開し、フィルタリング作業の PR を作成しました。

機能のテストに MCP サーバーを使いましたが、MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティレビューなど、対象を絞ったタスクを効率化する[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
