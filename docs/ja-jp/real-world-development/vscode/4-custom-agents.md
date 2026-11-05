---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルでエージェントを定義し、特定の作業を効果的に行うための指示、ベストプラクティス、コーディングパターン、専門知識を指定します。チームの知識を再利用できるエージェントにまとめることもできます。たとえば、[Web Content Accessibility Guidelines (WCAG)][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディングに従うセキュリティエージェント、一貫したテストパターンを保つテストエージェントなどです。

カスタムエージェントは、プロジェクト内の `.github/agents` またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、その後にエージェントの動作、専門知識、指示を記載する Markdown プロンプトが必要です。

### カスタムエージェントと agent skills の違い

カスタムエージェントと [agent skills][agent-skills-concept]には一部共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。分かりやすく言うと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントには独自のコンテキストウィンドウがあり、作業の一部としてスキルや他のエージェントを調整します。この演習では、アクセシビリティカスタムエージェントがアクセシビリティガイドラインに照らしてサイトをレビュー・更新します。その際、プルリクエストワークフローのスキルやテスト実行・管理用のスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI を使う他の作業と同様に、利用する環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者がクラウドファンディングプラットフォームを使えるようにしたいと考えています。現在のダークテーマは、テキストと背景色のコントラストが不十分で、読みにくいと感じる利用者がいるというフィードバックが寄せられました。デザインチームは、利用者が切り替えられるハイコントラストモードを求めています。

アクセシビリティは重要なため、カスタムエージェントを使って機能をすばやく生成します。

この演習では、次のことを行います。

- 既存のアクセシビリティカスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに作成されています。Copilot をどのように導くか、その内容を確認しましょう。

1. Codespace に戻り、ターミナルを開きます。
2. フィルタリング PR とは分けてアクセシビリティ作業を行うため、`main` から新しいブランチを作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

3. `.github/agents/accessibility.md` を開きます。
4. `name` と `description` フィールドがある YAML frontmatter を確認します。

> [!CAUTION]
> `name` と `description` の frontmatter は、カスタムエージェントに必須です。

5. 続くセクションを確認します。
   - アクセシブルな Web サイトのコード生成に関する主要な責務
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題と避けるべきミス

## Copilot Chat でカスタムエージェントを使う

VS Code では、`.github/agents` に定義したカスタムエージェントが Copilot Chat ビュー下部のエージェント一覧に表示されます。エージェントを選ぶと、そのエージェント固有の指示とツールを使うチャットセッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なり、必要な場合は手順内で説明します。

1. Chat ビューのエージェントの一覧で **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. チャットビュー下部のエージェント一覧を開きます。現在のエージェントが表示されます。既定では **default** です。
3. 利用可能なエージェント一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティエージェントに送信します。

   ```plaintext
   サイトにハイコントラストモードを追加してください。利用者が切り替えられるトグルを設け、ブラウザーの local storage を使って設定をページ再読み込み後も保持してください。
   ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクトファイルに適用します。各編集はファイルパスと差分とともにチャットに表示されます。
6. エージェントが変更したファイルに応じて、**Files changed** の表示が更新されることを確認します。

> [!NOTE]
> 処理には数分かかる場合があります。Copilot は実際のリポジトリを変更し、Astro コンポーネント、CSS、関連テストなどを更新します。

次の演習では、進行中の作業を確認し、必要に応じてエージェントに指示します。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]は、特定のタスクや分野に合わせた AI アシスタントです。チームの知識や規約を再利用できるエージェントにまとめ、Copilot がより効果的に作業できるようにします。

この演習では、次のことを行いました。

- 既存のアクセシビリティ用カスタムエージェントを確認しました。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装しました。

次は、エージェントの作業を[監視して方向を示します][next-lesson]。変更を確認し、同じセッションにライトモードのトグルを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントを監視・管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
