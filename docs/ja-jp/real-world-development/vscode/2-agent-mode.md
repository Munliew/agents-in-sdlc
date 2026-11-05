---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

小さなアプリケーションの変更でも、複数のファイルを更新し、テストを実行するなどの作業が必要になることがあります。開発者は必要なファイルを特定して変更し、テストを実行し、デバッグし、更新し忘れたファイルを見つけて修正する、といった作業を繰り返します。

このようなときに役立つのが Copilot Agent Mode です。

Copilot Agent Mode は、統合開発環境 (IDE) 内でより自律的に作業します。既存プロジェクトの構造を調べ、必要なファイルを更新し、テストなどのタスクを実行して、見つかった問題を自動的に修正します。Agent Mode を使って、サイトに新機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる体験を提供します。agent mode は IDE 内で動作し、短いフィードバックサイクルで対話できます。cloud agent はチームメンバーのように非同期で作業し、issue とプルリクエストを通じて連携します。

この演習では、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数の変更を調整する方法。
- GitHub Copilot Agent Mode でユーザーインターフェイス (UI) とデータレイヤーにまたがる機能を実装する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリとパブリッシャーで絞り込めるようにします。前の演習では publishers ヘルパーを追加しました。次は Copilot Agent Mode で、残りのデータレイヤー、UI、テストを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを開いて現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示します。単一の Astro アプリケーションで、ビルド時に静的なハイパーテキストマークアップ言語 (HTML) を生成します。ページは frontmatter 内で Drizzle ORM を使ってローカル SQLite データベースを直接検索します。別のバックエンドのアプリケーション プログラミング インターフェイス (API) やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブな動作には標準のドキュメントオブジェクトモデル (DOM) API を使う小規模な Astro `<script>` を使用します。

### Web サイトを起動する

単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。次の手順で GitHub Codespace から起動します。

1. Codespace に戻ります。前の演習を終えて `main` に戻っていることを確認します。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. `main` を更新し、フィルタリング作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を選択してサイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動転送されます。これは Codespace の Web サーバーに接続するプライベートトンネルです。

### Web サイトを確認する

サイトを起動したら、現在の機能を確認します。

- **ホームページ**: ボードゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なり、必要な場合は手順内で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加します。テンプレートからプロジェクトを作成したときに issue のバックログも自動作成されています。GitHub Copilot に内容を表示させます。

1. Chat ビューのエージェントの一覧で **Agent** を選択します。Agent は自律的に計画し、複数ファイルを変更し、ターミナルコマンドとツールを実行します。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップは特定のモデルを推奨するものではありません。作成時に Claude Sonnet 4.5 を使ったため、手順にも記載しています。提案の一貫性を高めることが目的ですが、大規模言語モデル (LLM) は確率的に動作するため、生成結果が説明と異なる場合があります。これは正常な動作です。

> [!NOTE]
> 確率的な動作のため、Copilot は別の Model Context Protocol (MCP) コマンドを使う場合がありますが、タスク自体は完了できるはずです。

3. 次のプロンプトを Copilot に送信して、リポジトリの issue を表示させます。

   ```plaintext
   GitHub リポジトリのバックログを表示してください。利用者にとって役立つものから優先順位を付けるのを手伝ってください。
   ```

4. issue を一覧表示するコマンドの実行を許可するには、**Continue** を選択します。
5. 生成された issue の一覧を確認します。

Copilot が利用者に役立つと考える順に issue を優先したことを確認します。

## 指示ファイルを確認する

コード生成を始める前に、Copilot にコンテキストを与える指示ファイルを確認します。この演習では、Web サイトに機能を追加する方法を説明する [ユーザーインターフェイス (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の進め方に関する指針を確認します。
   - アーキテクチャの概要
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle などの指示ファイルへのリンク

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクトファイルを参照できます。パスは指示ファイルからの相対パスです。複雑な指示を再利用しやすい小さなファイルに分け、例やテンプレートを参照できます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも 3 つの変更が必要です。

- データレイヤー (`src/lib/`) にフィルタリングロジックを追加または調整します。
- フィルタリングの動作を確認するテストを追加または更新します。
- ゲーム一覧ページを更新して、フィルター UI を追加します。

コードベースにマージする前に、テストも実行して成功を確認します。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. 現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** を選んで指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントの一覧で **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログの関連 issue に基づいて機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   関連する GitHub issue の要件に基づいて、パブリッシャーとカテゴリで絞り込めるようサイトを更新してください。前の演習で追加した publishers ヘルパーを維持し、必要に応じて調整しながら、残りのデータレイヤー、UI、テストを完成させてください。完了前にすべてのテストが成功することを確認してください。サーバーはすでに起動しているため、起動する必要はありません。
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを探す様子を確認します。データレイヤー、UI、テストを特定した後、ファイルの変更とテスト実行を始めます。

   ![Copilot がプロジェクトファイルを調べている様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行うため、タスクやコードベースによっては数分かかります。作業中、エディターに **Keep** と **Undo** が表示されることがあります。作業中に選択する必要はありません。完了すると、変更ごとに **Keep** または **Undo** を選べます。

7. Copilot から求められたら、テストを実行するため **Continue** を選択します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログのスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 処理中に一時停止したり、テストが失敗したりする場合があります。Copilot はタスクが完了し、エラーが見つからなくなるまでコード生成とテストを繰り返します。

   ![Copilot Agent Mode の完了した Chat セッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot や他の AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを表示しているブラウザーに戻り、新機能を試します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って、Tailspin Toys の Web サイトに新機能を追加しました。この演習では、次のことを行いました。

- プロジェクトを調べ、関連ファイルを特定し、複数の変更を調整しました。
- GitHub Copilot Agent Mode で UI とデータレイヤーにまたがる機能を実装しました。
- コードベースにマージする前に、生成された変更とテストをレビューしました。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナス演習 - ページネーションを実装する

ゲームの一覧が増えるとページネーションが必要です。この演習で学んだことを使い、Copilot にページネーションの実装を依頼します。次の点を検討してください。

- 既存の指示ファイルを含め、プロジェクトのベストプラクティスに従います。
- ページサイズを利用者が選べるようにするか、固定にするかを決めます。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- Copilot に変更とコンテキストを伝え、反復が必要な場合があります。Copilot を使うときの通常の進め方です。

## リソース

- [Copilot の ask、edit、agent mode: 機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
