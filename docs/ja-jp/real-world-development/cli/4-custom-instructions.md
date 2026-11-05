---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント作成の規約を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合は、その指針を Copilot が参照できるようにします。[指示ファイル][instruction-files]では、求めるコードだけでなく、その構造や書き方も示します。フィルタリング機能を実装したので、Copilot が参照した指示を確認し、ドキュメント作成の規約を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示ファイルとパス指定の指示ファイルがエージェントに適用される方法を確認します。
- コーディング規約に従うよう、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。

- コメントにはコードの説明ではなく、意図や自明でない判断を記述します。
- `db/` と `src/lib/` のエクスポートされた関数では、目的、パラメーター、戻り値を TypeScript Documentation (TSDoc) / JavaScript Documentation (JSDoc) で説明します。該当する場合は、注入可能な `db` 引数も記載します。
- 再利用可能な Astro コンポーネントでは、`Props` の契約を文書化し、関連コードが変更された際にコメントも更新します。
- 既存の書式設定と lint の指針を維持します。

指示ファイルを使って、Copilot がこれらの開発方針に沿って作業できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件のコンテキストを Copilot に渡し、より適切な提案やコード生成を促せます。好みの規約、ライブラリ、コードコメントの形式などを指定できます。リポジトリ全体、または特定のファイルやタスクに指示を設定できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。多くのリクエストに関係する技術スタック、開発内容の概要、ベストプラクティスなど、プロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、言語やファイルの種類、特定のタスクに対する指針を設定します。

> [!NOTE]
> その他の指示形式とサポート状況は環境によって異なります。特定の形式を利用する前に、[カスタム指示のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトには、すでに指示ファイルが用意されています。変更の影響を確認するため、まず既存のファイルを見てみましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング指針を確認します。この指示は、リポジトリでの Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルを指定する glob であることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存プロジェクトファイルへのリンクを確認します。これにより、大きな指示を再利用できる小さなファイルに分け、コード生成時に参照する例も指定できます。

## チームの方針に合わせて指示ファイルを更新する

既存の指示は良い出発点ですが、追加できる項目があります。中核となる `copilot-instructions.md` を変更し、新しく生成される TypeScript に TSDoc コメントを追加するよう指定します。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** という見出しを探します。35 行目付近にあります。
2. 最後の箇条書きに次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新した指針を適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。フィルタリング機能のコンテキストを保ったまま更新後の指示を読み込めるよう、会話を終了して再開します。

1. フィルタリングの会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたら、フィルタリングの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. Copilot に更新後の指針の適用を依頼します。

   ```plaintext
   指示とコードのガイダンスを更新しました。生成したコードをこのガイダンスに合わせて更新してください。
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する方法を確認し、新しい規約を機能に適用しました。このレッスンでは、次のことを行いました。

- リポジトリの指示ファイルとパス指定の指示ファイルがエージェントに適用される方法を確認しました。
- コーディング規約に従うよう、指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、[再利用可能な `quality-checks` スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行できるようにします。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示のベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
