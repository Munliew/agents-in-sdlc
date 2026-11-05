---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときはコンテキストが重要です。タスクを特定の方法で実行したい場合や、Copilot に背景情報を伝えたい場合は、その情報を参照できるようにします。[指示ファイル][instruction-files]を使うと、コード生成に関する指針を Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス指定の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント規約が Copilot に届く方法を確認します。
- 現在の指示を適用した状態で、フィルタリング用データの最初の部分 (publishers ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい規約を追加します。
- 同じプロンプトを再送信して、生成コードが新しい規約に従う様子を確認します。
- 次の演習で Copilot が更新済みの指針を使えるよう、指示の更新とフィルタリングの基盤を `main` にコミットします。

> [!CAUTION]
> 生成コードが設定した規約から外れる場合があります。Copilot は確率的に動作します。この演習では、出力の文字単位の一致ではなく、指示を更新した後の傾向の変化を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には開発手法に関するガイドラインがあります。

- データレイヤーには必ずユニットテストを用意します。
- ユーザーインターフェイス (UI) はダークモードで、現代的な見た目にします。
- コードには TSDoc のドキュメントコメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントを追加します。

指示ファイルを使い、Copilot がこれらの開発方針に沿って作業できるようにします。

### カスタム指示

カスタム指示は、コーディングスタイルや要件のコンテキストを Copilot に伝え、より適切な提案やコード生成を促します。好みの規約、ライブラリ、コードコメントの形式などを指定できます。リポジトリ全体や、特定の種類のファイルに対して指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。多くのチャットや CLI リクエストに関係する技術スタック、開発内容の概要、ベストプラクティスなどを記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に指針を設定します。TypeScript や Astro などの言語、UI コンポーネントやユニットテストの作成などを対象にできます。

> [!NOTE]
> 統合開発環境 (IDE) では、指示ファイルは Copilot Chat のコード生成に使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルをコード生成時に使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの規格の指示ファイルにも対応しています。[対応する指示ファイル][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

スタータープロジェクトの例を見ながら、指示ファイルの構成を確認します。

- `copilot-instructions.md` には、プロジェクトの説明、構造、全体的なコーディング規約など、プロジェクト全体に関する指針を記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種類や特定のタスクに関する指示を記載します。
- 自然な言葉を使い、明確な指針とコードの良い例・悪い例を示します。

指示ファイルの作成方法は 1 つではありません。プロジェクトで試しながら、適切な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、指示ファイルを充実させることをお勧めします。このプロジェクトには [UI 更新用][ui-instructions]や [Astro 用][astro-instructions]など、さまざまなタスク向けの指示があります。
>
> Copilot に指示ファイルを生成させることもできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、各環境で操作が異なります。必要な場合は、該当する環境のレッスンで説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

エディターまたは GitHub Web UI で、このリポジトリに含まれる指示ファイルを読みます。中心となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** の指針も、Copilot とのすべてのやりとりに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データレイヤー、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。リポジトリルートからの glob で適用対象を指定します。ここでは、`**/*.test.ts` に一致する TypeScript のテストファイルなどが対象です。
5. プロジェクトのユニットテスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の指示ファイル (例: `unit-tests.instructions.md`) やプロジェクトファイルへのリンクを確認します。指示を再利用できる小さなファイルに分け、Copilot がコード生成時に参照する例を示せます。ここに記載するパスは、リポジトリのルートではなく指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約が記載されていますが、コード内のドキュメントはまだ求めていません。次の手順で TSDoc コメントとファイルヘッダーの規約を追加します。

## 変更用ブランチを作成する

変更を作業用ブランチで管理します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なり、必要な場合は手順内で説明します。

現在の指示を使ってプロンプトを送り、カスタム指示の効果を確認します。後で指示を更新した後、同じプロンプトを再送信して違いを見ます。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. ヘルパーの配置先として `src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェントの一覧で **Agent** を選び、ファイルの変更を適用できるようにします。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   src/lib/publishers.ts を作成または更新し、すべてのパブリッシャーについて名前と ID の一覧を返すデータアクセスヘルパーを追加してください。ファイルの変更を適用してください。
   ```

5. Copilot はプロジェクトを調べ、ヘルパーファイルとテストなどにコードを適用します。
6. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取り、型付きのパブリッシャー配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に基づいています。
7. 提案されたコードには、TSDoc コメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加することがあります。その場合も問題ありません。重要なのは指示を更新した後に動作が一貫して改善することです。

## リポジトリ全体の新しい規約を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクト全体の情報を伝えるファイルです。コード提案を改善するため、コーディング規約を追加します。

1. `.github/copilot-instructions.md` を開き直します。
2. **Code formatting requirements** セクション (27 行目付近) を探します。現在の規約は記載されていますが、インラインドキュメントに関する規則がまだないことを確認します。
3. 既存の規約の直後に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体の指示には `.github/copilot-instructions.md` を、特定の言語、ファイルの種類、タスクには `*.instructions.md` を使えます。今回追加したドキュメントコメントの規約は、プロジェクト全体に適用するため、リポジトリレベルのファイルに適しています。

## 同じプロンプトを再送信して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選んで会話を新しくします。
2. `src/lib/publishers.ts` を選択して、Copilot が正しいファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   src/lib/publishers.ts を作成または更新し、すべてのパブリッシャーについて名前と ID の一覧を返すデータアクセスヘルパーを追加してください。ファイルの変更を適用してください。
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 生成された関数に、次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しいプロジェクト規約を適用し、次の演習で使うコードに反映させました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもほかの資産と同じようにソース管理します。変更をコミットしてリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基盤作成で Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント規約の issue に関連付けた PR を作ります。GitHub ツールがプロジェクトに接続されているため、Copilot に issue を検索させ、PR を作成してマージできます。

> [!NOTE]
> Copilot に GitHub 操作を依頼するのは今回が初めてです。プロジェクトテンプレートでは、GitHub ツールがワークスペースにあらかじめ設定されています。Copilot が初めてツールを使うとき、VS Code で GitHub へのサインインを求められる場合があります。表示される手順に従ってください。この接続方法 (Model Context Protocol) は後の演習で扱います。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントの一覧で **Agent** が選択されていることを確認します。
3. 指示ファイルの更新と、今後のフィルタリング作業に使う publishers ヘルパーの基盤作成の両方を説明し、現在のブランチから PR を作成するよう Copilot に依頼します。

   ```plaintext
   指示ファイルの更新に関する issue を探してください。現在のブランチから新しい PR を作成し、その issue を解決することを明記してください。また、今後のフィルタリング作業に向けて publishers ヘルパーの基盤も追加したことを説明してください。
   ```

4. Copilot が issue を探し、PR を作成する様子を確認します。
5. GitHub 操作の実行許可を求められたら、コマンドを確認して、適切な操作だけに **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージと `main` への切り替えを依頼します。

   ```plaintext
   PR を main にマージしてください。その後、ローカルで main に戻り、最新コードを取得してください。
   ```

7. GitHub 操作やシェルコマンドの実行許可を求められたら、内容を確認してから適切に **Allow** を選択します。

GitHub Copilot の支援で PR を作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示でコード生成プロンプトを送信し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の規約を追加しました。
- 同じプロンプトを再送信し、生成コードが新しい規約に従う様子を確認しました。
- 次の演習で使えるよう、指示とヘルパーの基盤を `main` にコミットしました。

次は Copilot がコードベース全体に機能を追加する際に、[agent mode で指示を活用します][next-lesson]。

## リソース

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [カスタム指示作成のベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
