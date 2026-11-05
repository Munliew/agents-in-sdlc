---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

多くの組織では、解決されていないセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能要望など、技術的負債に直面しています。GitHub Copilot の cloud agent は、コードの更新や機能追加などを自律的に実行します。作業が終わると、開発者がレビューできるドラフトのプルリクエスト (PR) を作成します。時間のかかる作業を任せることで、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent で次のことを行います。

- コード生成環境をカスタマイズします。
- 操作を安全に実行する方法を確認します。
- issue の範囲を明確にする重要性を学びます。
- issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを作成した開発者がドキュメントを残しておらず、アプリケーションにはほとんど記載がありません。まず、エクスポートされたすべての関数に TypeScript Documentation (TSDoc) のドキュメントコメントを追加します。

さらに、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のゲームを表示すれば、利用者は引き続きゲームを探せます。まずはユーザー体験 (UX) の受け入れテストに使える簡単な実装を希望しています。この作業は現在のブロッカーですが、優先度がより高い別の issue があります。

どちらも優先順位が下がりやすいタスクです。Copilot cloud agent に割り当てると、開発者がほかの作業をしている間に非同期で進められ、後から結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドで作業します。[GitHub issue を Copilot に割り当てる][assign-issue]方法などでタスクを依頼できます。割り当て後、Copilot は進行状況を追跡するドラフト PR を作成し、環境を準備して、タスクに取り組みます。作業中も完了後もセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。単純に見える操作にも複雑さが伴うため、[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に定めてください。AI ペアプログラマーとして扱い、段階的に作業し、学び、試し、調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot の動作への影響を確認しました。カスタム指示を実際に試したことがあれば `.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、cloud agent に作業を割り当てる前に、このリポジトリの指示ファイルを読み取り専用で確認しておきます。

リポジトリの GitHub Web UI または起動済みの Codespace で、次のファイルを確認します。

1. `.github/copilot-instructions.md` を開き、**Code standards** セクション、特に TypeScript 規約と TSDoc コメントに関する指針を確認します。
2. `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` frontmatter が `**/*.test.ts` に適用される指示を定めていることを確認します。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てたら、作成された PR に TSDoc コメント、TypeScript 規約、ファイルヘッダーが反映されているか確認してください。後の演習で PR をレビューするときにも確認します。

## Copilot cloud agent の開発環境を設定する

コード作成には、通常、適切な環境と初期化スクリプトが必要です。Copilot にタスクを割り当てる場合も、ソフトウェアエンジニアのように作業するための環境が必要です。

cloud agent は GitHub Actions を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に設定した[セットアップワークフロー][setup-workflow]を作成すると、必要な開発ツールや依存関係を使えるようにできます。このワークショップでは、環境があらかじめ設定されています。単一の Astro アプリ用に、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションとシード済みの SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限の権限を設定します。Copilot は自身の操作用トークンを受け取ります。
    permissions:
      # セットアップ手順でリポジトリをクローンする場合 (依存関係のインストールなど) は contents: read が必要です。クローンしない場合、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリは Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテスト用にローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローファイルと同様ですが、重要な点があります。

- `copilot-setup-steps` という単一のジョブがあり、Copilot が PR の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うため、Copilot の実行を待たずにリポジトリの **Actions** タブから手動で実行し、セットアップワークフローを検証できます。

## ドキュメントを追加する

多くのプロジェクトでは、ドキュメントが古いか不足しています。対応されないままの技術的負債は、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメント作成を支援できるため、このタスクを cloud agent に割り当てます。後の演習で結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    組織では、役立つ場合に関数とメソッドへ TSDoc のドキュメントコメントを付けるよう定めています。しかし、最近の変更はこの規約に従っていません。動作を明確にする既存コードにドキュメントコメントを追加してください。
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成された PR を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前です。表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後に、Copilot が作成した todo リストを確認します。

> [!NOTE]
> Copilot が PR に todo リストを表示するまで数分かかる場合があります。前述のワークフローで環境を準備し、プロジェクトを分析して、作業の方法を検討しています。

13. todo リストを読み、Copilot が実行するタスクを確認します。
14. PR のタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを確認します。
15. **View session** ボタンを選択します。

  ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示を更新するため、ウィンドウの再読み込みが必要な場合があります。

16. ライブセッションをスクロールして Copilot の作業を確認します。コードを調査して状態を把握し、計画を考えて、コードを作成する様子を確認できます。

非同期で作業させることが cloud agent の大きな利点です。エージェントに次のタスクも割り当て、その間にアプリケーションの機能を追加するコードを書きます。

## ゲーム詳細ページに関連ゲームを追加する

cloud agent に作業を分けて任せれば、開発者は別のタスクに集中できます。デザインチーム向けの関連ゲーム表示に長い時間がかからないとしても、その時間をほかの作業に使えます。次の issue を cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. 空のテンプレートとして **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    ゲーム詳細ページに関連ゲームを表示し、利用者がほかのゲームを見つけやすくしたいと考えています。デザインチームは UX を検討し、受け入れテストを行います。要件は次のとおりです。

    - `src/lib/` に、指定したゲームを除き、同じカテゴリに属する他のゲームを返すデータアクセスヘルパーを追加する
    - ヘルパーを使う「Related games」セクションをゲーム詳細ページに表示する
    - 関連ゲームがない場合に対応する
    - 新しいヘルパーのユニットテストを作成する
    - PR 作成前にすべてのテストが成功することを確認する
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

issue の最初のコメントに 👀 が表示され、Copilot が作業中であることが分かります。

![issue で目の絵文字を使って作業中を示す Copilot](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組み始めました。ソフトウェアエンジニアのように非同期で作業するため、常に監視する必要はありません。完了後に結果をレビューします。次はカスタムエージェントを作成して使います。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents] を確認しました。issue を割り当てると、Copilot は非同期に作業します。技術的負債の解消、新機能の作成、フレームワーク間のコード移行などを任せられます。

このレッスンでは、次のことを行いました。

- コード生成環境をカスタマイズしました。
- 操作を安全に実行する方法を確認しました。
- issue の範囲を明確にする重要性を学びました。
- issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業する間、次はカスタムエージェントを作成して使います。[Copilot cloud agent は Model Context Protocol (MCP) サーバーも利用でき][cloud-agent-mcp]、前のモジュールで扱ったカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
