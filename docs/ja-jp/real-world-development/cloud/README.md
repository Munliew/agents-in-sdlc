---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウドで非同期に作業できます。GitHub で作業を割り当てると、cloud agent がバックグラウンドでリポジトリを調査し、変更を加えてプルリクエストを作成するため、その間も別の作業を続けられます。

各演習では、まず cloud agent が従うカスタム指示を追加し、GitHub issue を割り当てて実装を任せます。作業をレビューし、カスタムエージェントを使ってアプローチを調整し、エージェントのダッシュボードからセッションの進捗を監視して方向を示します。最後に、作成されたプルリクエストをレビューし、結果を改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. カスタム指示][ex1] | コンテキスト | cloud agent が従うカスタム指示を追加します |
| [2. Cloud Agent][ex2] | 非同期エージェント | GitHub issue を Copilot cloud agent に割り当てます |
| [3. カスタムエージェント][ex3] | 専門エージェント | カスタムエージェントを確認して使います |
| [4. エージェントを管理する][ex4] | 監視 | エージェントセッションを監視し、方向を示します |
| [5. 反復する][ex5] | レビュー | プルリクエスト (PR) をレビューし、Copilot の作業を改善して、次のステップを選びます |

## 前提条件

ワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、または Enterprise** プランの GitHub アカウント
- [ ] アカウントまたは組織で有効になっている Copilot cloud agent

> [!TIP]
> 有料プランを利用していない場合、認定を受けた学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、どの環境のワークショップにも参加できます。

[student-plan]: https://github.com/education/students

> [!NOTE]
> cloud agent は **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Free は対象外です。Copilot Business または Enterprise では、管理者が組織で有効にする必要があります。

> [!NOTE]
> この環境のワークショップでは Model Context Protocol (MCP) を扱いません。Copilot で MCP サーバーを使う方法については、[CLI 環境][cli]または [VS Code 環境][vscode]を参照してください。

## 開始する

**[演習 0: 前提条件から始める →][ex0]**

[cli]: ../cli/
[vscode]: ../vscode/
[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
