---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、結果のレポート形式をカスタマイズして、フィルタリング機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、開発には多くの作業があります。これまではコードを手動で検証し、指示ファイルを使ってプロジェクトの標準に従うようにしてきました。テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

こうした作業には **agent skill** が適しています。スキルを使うと、Copilot がテストなどの操作を適切に実行する方法を理解できます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果のレポート形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、単体テスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

Agent skill は、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースをまとめ、必要なときにエージェントが読み込むものです。基本的には、スキル名のフォルダーと、その中にある `SKILL.md` という Markdown ファイルで構成されます。この Markdown ファイルには、スキルの内容と呼び出すタイミングを定義する `name` と `description` の frontmatter、および手順が記述されます。スキルのフォルダーには、スクリプトや補助リソース用のサブフォルダーを追加することもできます。

> [!NOTE]
> スキルに追加のファイルやフォルダーは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトにある既存のコマンドを使うため、`SKILL.md` のみで構成されています。

スキルは、チームで共有して再利用できるようにプロジェクトの `.github/skills` フォルダーに置くか、ユーザーの `~/.copilot/skills` フォルダーに置くことができます。

## スキルを確認する

Tailspin Toys チームが単体テスト、lint、型チェックの実行に用意した `quality-checks` スキルの内容を確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. ファイルの先頭にある `name` と `description` を読みます。`description` は Copilot がスキルを呼び出すタイミングの判断に役立ちます。
3. 指示を読み、テストと lint の手順をどのように案内しているか確認します。
4. **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

まずスキルを実行し、単体テスト、lint、型チェックの結果を確認します。

1. Interactive モードでフィルタリングの会話に戻ります。
2. 次のプロンプトを入力します。

   ```plaintext
   `quality-checks` スキルを実行して、単体テスト、lint、型チェックを行ってください。
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行したチェックとその成否、ツールが報告した内容がわかるようにレポートを改善します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. 次のセクションをファイルの末尾に追加します。

   ```markdown
   ## 結果の出力形式

   完了したら、実行した各コマンドと、成功、失敗、またはブロックされたかを報告します。テスト件数、所要時間、エラー、警告、その他の指標は、ツールが報告した場合に限り記載します。失敗やブロッカーがある場合は次に行う対応を示し、スキップまたは未完了のチェックを成功として報告しないでください。
   ```

3. ファイルが自動的に保存されたことを確認します。

## 更新したスキルを実行する

Copilot CLI は、会話を再起動せずに編集したスキルを再読み込みできます。同じプロンプトをもう一度実行して、レポートの違いを確認します。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 変更前と同じプロンプトを入力します。

   ```plaintext
   `quality-checks` スキルを実行して、単体テスト、lint、型チェックを行ってください。
   ```

3. 最後に表示されるレポートを確認し、最初の結果と比較します。

## まとめと次のステップ

既存の agent skill をカスタマイズして使いました。このレッスンでは、次のことを行いました。

- `quality-checks` スキルを確認しました。
- 結果のレポート形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、フィルタリング機能のプルリクエストに含まれます。次は [Playwright MCP サーバー][next-lesson]を使って、Copilot がサイトを直接操作できるようにします。

## スキルのその他の例

次のコミュニティ提供の例は参考資料です。追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
