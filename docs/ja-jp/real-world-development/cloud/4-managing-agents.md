---
title: "演習 4 - エージェントを監視・管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の 2 つの演習では、利用者体験の改善と機能追加を目的に、Copilot cloud agent に 3 つのタスクを割り当てました。cloud agent は非同期かつ自律的に作業しますが、進行中のタスクを監視することも重要です。

タスクの管理には、GitHub.com の[エージェントページ][agents-page]など、さまざまなツールを利用できます。Mission control では、開いているプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行内容を調べ、進行中のセッションに指示を送って作業を導くこともできます。

このレッスンでは、次のことを行います。

- エージェントページで cloud agent のタスクを監視します。
- 実行中のセッションに指示を送り、機能を追加します。

## シナリオ

チームはハイコントラストモードを追加する作業を Copilot に依頼した後、ライトモードも必要だと気付きました。サイトのスタイルと切り替え機能を更新しているため、同時に追加するのが適切です。ハイコントラストに加えてライトモードも実装するよう、エージェントの作業に指示します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン領域と左側の領域に表示されたタスクを確認します。次のような割り当て済みタスクが表示されます。
   - コードベースのドキュメント更新
   - ゲーム変更用 API の生成
   - Web サイトへのハイコントラストモード追加
3. 実行中のタスクを選択し、Copilot が行った操作を確認します。
   - リポジトリのチェックアウト
   - Copilot の作業環境の作成
   - MCP サーバーの設定
   - タスク完了に向けた各種操作

> [!NOTE]
> 表示される手順は、Copilot の作業状況や実装方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) の領域も確認します。PR と変更されたファイルを確認して進捗を把握できます。

## cloud agent に作業の指示を送る

現在実行中のタスクを確認できました。次に、ハイコントラストモードの作業にライトモードのトグルも加えるよう Copilot に依頼します。

1. ハイコントラストモードを追加しているセッションを選択します。タイトルは Copilot が付けた名前や作業状況によって異なります。

    ![Mission control でアクセシビリティのセッションを表示](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで数分待ちます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    ハイコントラストモードを作成する際に、ライトモードも追加してください。利用者が表示モードを選べるよう、ライトモード用のスイッチも設けてください。
    ```

    ![エージェントページで、作業中の cloud agent に指示するダイアログを強調表示したスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が依頼を認識し、作業計画に加えることを確認します。

## Copilot の作業を続ける

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中の手順を終えた後、新しい依頼を計画に取り込みます。

今回も完了まで数分かかります。その間に、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot agents ページを使い、GitHub Copilot cloud agent のタスクを監視して指示する方法を確認しました。Mission control では、進行中と完了済みのタスクを追跡し、作業内容を確認し、進行中のタスクに方向を示せます。

このレッスンでは、次のことを行いました。

- Mission control とエージェントページで cloud agent のタスクを監視しました。
- 進行中のセッションに指示し、機能を追加するよう依頼しました。

Copilot がアクセシビリティ機能の作業を続ける間、次は Copilot が作成した[プルリクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
