---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行したい場合や、Copilot に背景情報を伝えたい場合は、その情報を参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に作業内容だけでなく方法も伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス指定の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント規約が Copilot に届く方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体の規約を追加します。

> [!NOTE]
> VS Code や CLI の環境とは異なり、この演習では指示を変更する前後に同じプロンプトを実行しません。Copilot cloud agent は GitHub issue に対して非同期で作業するため、リアルタイムで並べて効果を確認するのは難しいからです。後の演習で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には開発手法に関するガイドラインがあります。

- データレイヤーには必ずユニットテストを用意します。
- ユーザーインターフェイス (UI) はダークモードで、現代的な見た目にします。
- コードには TypeScript Documentation (TSDoc) のドキュメントコメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントを追加します。

指示ファイルを使って、Copilot がこれらの開発方針に沿って作業できるようにします。

### カスタム指示

カスタム指示は、コーディングスタイルや要件のコンテキストを Copilot に伝え、より適切な提案やコード生成を促します。好みの規約、ライブラリ、コードコメントの形式などを指定できます。リポジトリ全体や、特定の種類のファイルに対して指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。プロジェクトの技術スタック、開発内容の概要、ベストプラクティスなどを記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に指針を設定します。TypeScript や Astro などの言語、UI コンポーネントやユニットテストの作成などを対象にできます。

> [!NOTE]
> 統合開発環境 (IDE) では、指示ファイルは Copilot Chat のコード生成に使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルをコード生成時に使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの規格の指示ファイルにも対応しています。[対応する指示ファイル][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

このプロジェクトの例を参考に、指示ファイルを次のように構成します。

- `copilot-instructions.md` には、プロジェクトの説明、構造、全体的な規約など、プロジェクト全体に関する指針を記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種類や特定のタスクに関する指示を記載します。
- 自然な言葉を使い、明確な指針とコードの良い例・悪い例を示します。

指示ファイルの作成方法は 1 つではありません。プロジェクトで試しながら、適切な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、指示ファイルを充実させることをお勧めします。このプロジェクトには [UI 更新用][ui-instructions]や [Astro 用][astro-instructions]など、さまざまなタスク向けの指示があります。
>
> Copilot に指示ファイルを生成させることもできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、各環境で操作が異なります。必要な場合は、該当する環境のレッスンで説明します。
>
> テンプレートや出発点には、指示ファイルやカスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を利用できます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

リポジトリに含まれる指示ファイルを、エディターまたは GitHub Web UI で確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** の指針も Copilot とのすべてのやりとりに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データレイヤー、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドが適用対象を指定する glob であることを確認します。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. このプロジェクトのユニットテスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の指示ファイル (例: `unit-tests.instructions.md`) やプロジェクトファイルへのリンクを確認します。指示を再利用できる小さなファイルに分け、Copilot がコード生成時に参照する例を示せます。ここに記載するパスは、リポジトリルートではなく指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトの規約が記載されていますが、コード内のドキュメントはまだ求めていません。次の手順で TSDoc コメントとファイルヘッダーの規約を追加します。

## リポジトリ全体の規約を追加する

この演習で編集するのは、コード内のドキュメントに関するプロジェクト全体の規約です。後の演習で issue を割り当てると、cloud agent がこの指示を使います。

変更用のブランチを作成します。cloud agent は issue の対象ブランチから指示を読み取りますが、ここでは変更をきちんとコミットします。

1. Codespace のターミナルで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` に規約を追加します。

1. `.github/copilot-instructions.md` を開き直します。
2. **Code formatting requirements** セクション (27 行目付近) を探します。現在はコード内のドキュメント規約がないことを確認します。
3. 既存の規約の直後に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> リポジトリ全体の指示には `.github/copilot-instructions.md` を、特定の言語、ファイルの種類、タスクには `*.instructions.md` を使えます。今回のようなプロジェクト全体の規約には、リポジトリレベルのファイルが適しています。

## 指示の更新をコミット、プッシュ、マージする

cloud agent は、issue の対象ブランチから指示ファイルを読み取ります。次の演習で issue を割り当てると、Copilot は `main` からブランチを作るため、指示を `main` にマージしておく必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で issue に取り組むとき、cloud agent がこの指示を読み取れるようになります。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチ作成を省略して `main` にコミットしてもかまいません。実際のプロジェクトの流れに近づけるため、ここではブランチを使います。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` に新しい規約を追加しました。残りの環境別ワークショップで cloud agent が作成する PR をレビューするときに、この規約が使われたか確認します。

次は [Copilot cloud agent に最初の issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [カスタム指示作成のベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
