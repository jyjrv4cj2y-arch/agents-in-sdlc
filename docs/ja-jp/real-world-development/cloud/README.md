---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** は、GitHub Copilot をクラウドで非同期に実行します。GitHub で作業を割り当てると、cloud agent がバックグラウンドでリポジトリを調べ、変更を加え、プルリクエストを作成します。その間、ほかの作業を続けられます。

これらの演習では、まず cloud agent が従うカスタム指示を追加し、GitHub Issue を割り当てて実装を任せます。カスタムエージェントをレビューして作業方針を調整し、エージェントのダッシュボードからセッションの進行を監視して誘導します。最後に、作成されたプルリクエストをレビューし、結果を反復して改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. カスタム指示][ex1] | コンテキスト | cloud agent が従うカスタム指示を追加します |
| [2. Cloud Agent][ex2] | 非同期エージェント | GitHub Issue を Copilot cloud agent に割り当てます |
| [3. カスタムエージェント][ex3] | 専門エージェント | カスタムエージェントをレビューして使用します |
| [4. エージェントの管理][ex4] | 監視 | エージェントのセッションを監視し、誘導します |
| [5. 反復して改善する][ex5] | レビュー | プルリクエスト (PR) をレビューし、Copilot の作業を反復して改善して次のステップを選びます |

## 前提条件

このワークショップを始める前に、次のものを用意してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかが有効な GitHub アカウント
- [ ] アカウントまたは組織で有効になっている Copilot cloud agent

> [!TIP]
> 有料プランを利用していない場合、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、すべてのハーネスを完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot cloud agent は Copilot Free 以外の **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Business または Copilot Enterprise では、管理者が組織で有効にする必要があります。

> [!NOTE]
> このハーネスでは MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI ハーネス][cli]または [VS Code ハーネス][vscode]を参照してください。

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli]: ../cli/
[vscode]: ../vscode/
