---
title: "演習 5 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントを管理する][previous-lesson] |
|:--|

このワークショップでは、GitHub Copilot と複数のタスクに取り組み、利用者体験を改善して機能を追加してきました。コードへのドキュメント追加、デザインチーム向けの関連ゲーム機能、ハイコントラストとライトモードを含むアクセシビリティ機能を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って改善しましょう。

## 作業をレビューする

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成コードは常にレビューし、通常の DevOps プロセスに従います。チーム全体にレビューを有効にする前に、Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期かつ監視なしで作業するため、安全を保つためのセキュリティ制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みは作業用のブランチに限られます。
- cloud agent は GitHub Actions 内に一時的な専用環境を作成して実行します。
- GitHub Actions ワークフローの実行には、人間による承認が必要です。
- [外部リソースへのアクセスは既定で制限されます](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーも対象です。

## 生成されたドキュメントをレビューする

まず、Copilot cloud agent が作成した最初の PR、コードへのドキュメント追加を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に、GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot は MCP サーバーなどの外部リソースへのアクセスが既定で制限されるため、これは想定内です。必要であれば、[Copilot cloud agent のファイアウォールを構成または無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が付きます。Copilot が完了するまで待ってください。数分かかることがあります。その間に休憩したり、学んだことを振り返ったりできます。

4. 準備ができたら **Files changed** タブを選び、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc のドキュメントコメントなどが追加されたコードを確認します。具体的な変更内容は状況によって異なります。TSDoc、TypeScript 規約、ファイルヘッダーに注目してください。これらは演習 2 の開始時に確認したカスタム指示ファイルに基づいています。
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を探します。
8. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR の checks セクションにワークフローが登録されたことを確認します。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チームメンバーと同様に、PR やコードのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、修正のための新しいセッションを開始します。結果は確率的に変わるため、依頼する文面を指定することはできません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、ファイルの役割を説明するコメントヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. **@copilot** をユーザーと同じようにメンションして、生成されたドキュメントの変更を依頼します。上記の例、またはコードベースのドキュメントに関する別の提案を使います。
2. **View Session** を選択し、Copilot が変更する様子を確認します。Copilot は更新用の新しいセッションを開始します。
3. **Back to pull request** を選択して PR に戻ることもできます。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、新しいコミットが PR に追加されます。
5. **Files changed** タブで変更をレビューします。

満足するまで改善を繰り返せます。問題がなければ、ドラフト PR をレビュー可能な状態にし、`main` ブランチにマージします。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue のために Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示を探します。
7. 承認待ちのワークフローがある場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. PR の checks セクションにワークフローが登録されたことを確認します。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動テストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に `<branch-name>` を置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot は関連ゲーム機能を作成しました。前と同様に、Copilot cloud agent に更新を依頼して反復できます。たとえば、表示する関連ゲームの数を調整する、ファイルヘッダーや TSDoc コメントを追加するよう依頼できます。ただし、この issue を割り当てた時点ではカスタム指示が更新前だったことに注意してください。PR の **Conversation** タブにコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティカスタムエージェントが実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 の Mission control で追加を依頼したライトモードが含まれるはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が付きます。作業が完了するまで待ってください。数分かかることがあります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点を確認します。
   - モード切り替え用のトグル UI コンポーネント
   - 利用者の設定を保存する local storage の利用
   - ハイコントラストとライトモードの CSS またはスタイルの変更
   - ARIA ラベルやキーボード操作などのアクセシビリティ属性
   - モード切り替えを管理する JavaScript/TypeScript コード
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を探します。
8. 承認待ちのワークフローがある場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR の checks セクションにワークフローが登録されたことを確認します。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動テストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名に `<branch-name>` を置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリを起動し、ブラウザーでハイコントラストとライトモードのトグルを試します。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティカスタムエージェントは、アクセシビリティのベストプラクティスに従って機能を実装するよう Copilot を導きました。アクセシビリティ上の問題や改善点があれば、前の PR と同様に **@copilot** をメンションして更新を依頼できます。

## 任意の演習 - 追加の作業を委任する

cloud agent に実際のバックログ項目を割り当て、自分は別の作業に移ることで、委任の習慣を身に付けられます。リポジトリに issue を追加して Copilot に割り当ててみましょう。

- ゲーム詳細ページに支援者向けの関心フォームを作る。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent 環境でのワークショップを完了しました。これまでのレッスンでは、次のことを行いました。

- **リポジトリのカスタム指示ファイルを確認し**、cloud agent の出力への影響を確認しました。
- **issue を Copilot cloud agent に割り当て**、環境の準備、計画、非同期の実行を確認しました。
- **アクセシビリティ用のカスタムエージェントを作成して使い**、ハイコントラストとライトモードのトグルを追加しました。
- **Copilot agents ページを Mission control として使い**、アクセシビリティセッションを監視して作業中に指示しました。
- **cloud agent の PR をレビューして反復し**、`@copilot` に変更を依頼してワークフローを承認しました。

## レビューと次のステップ

Cloud agent 環境のワークショップを完了しました。他の環境のワークショップでは、今回の内容をさらに実践できます。

- 🖥️ **[VS Code 環境][vscode]** — IDE で Copilot Chat の agent mode と MCP 統合を試します。
- 💻 **[CLI 環境][cli]** — Plan と Autopilot モード、agent skills、カスタム QA エージェント、Playwright MCP、Agent Merge、任意の `/delegate` による cloud agent への連携を使い、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリでは、次の案を試せます。

- リファクタリングやテスト範囲の拡充に関する issue を cloud agent に割り当てます。
- 別の分野に特化したカスタムエージェントを作成します。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントを管理する][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
