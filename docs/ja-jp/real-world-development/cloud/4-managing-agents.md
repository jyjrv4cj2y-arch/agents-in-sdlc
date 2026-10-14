---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習で、Copilot cloud agent にユーザー体験の改善や機能追加に関する 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に動作しますが、作業を監視することも重要です。

cloud agent に割り当てたタスクの管理には、GitHub.com の[エージェントページ][agents-page]など、さまざまなツールを使えます。このミッション コントロールでは、開いているプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行された操作を調べたり、進行中のセッションを誘導したりできます。

このレッスンでは、次のことを行います。

- エージェントページで cloud agent のタスクを監視します。
- 実行中のセッションに追加機能を依頼して方向付けます。

## シナリオ

エージェントにハイコントラストモードの作成を依頼した後、チームはライトモードも追加するのによい機会だと考えました。サイトのスタイルを更新し、ユーザーインターフェイス (UI) に切り替え機能を追加する作業を進めているため、ライトモードも含めるのが自然です。ハイコントラストモードに加えてライトモードも追加するよう、エージェントの作業を方向付けます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左側のペインにあるタスク一覧を確認します。割り当てた次のタスクが表示されます。
   - コードベースのドキュメントの更新。
   - ゲームを変更する API の生成。
   - Web サイトへのハイコントラストモードの追加。
3. 実行中のタスクを 1 つ選択し、Copilot が行った作業を確認します。次のような操作が含まれる場合があります。
   - リポジトリからコードをチェックアウトする。
   - Copilot の作業環境を作成する。
   - MCP サーバーをセットアップする。
   - 割り当てられたタスクを完了するためにさまざまな手順を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。ここでは PR と変更されたファイルを確認し、進行状況をさらに監視できます。

## cloud agent の作業を方向付ける

実行中のタスクを確認できました。次はハイコントラストモードの作業中にライトモードも追加するよう Copilot に依頼します。

1. ハイコントラストモードの追加に関するセッションを選択します。正確なタイトルは、Copilot が付けた名前と作業状況によって異なります。

   ![ミッション コントロールに表示されたアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まるまで、数分間セッションを確認します。以下のようなメッセージが表示されたら、作業を開始しています。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![エージェントページで、実行中の Copilot セッションの誘導ダイアログが強調表示されたスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを認識し、作業に取り込むことを確認します。

## Copilot の作業を待つ

更新されたタスクに Copilot が取り組みます。メッセージを送信した時点で進行中だった手順が完了すると、新しい要求を作業に取り込みます。

今回も完了まで数分かかります。その間に、これまで学んだ内容や確認した内容を振り返りましょう。

## まとめと次のステップ

Copilot agents ページを確認しました。このページは GitHub Copilot cloud agent のタスクを監視し、方向付けるための中心的な場所です。ミッション コントロールでは、実行中と完了済みのタスクを追跡し、実行された作業をレビューして、進行中のタスクの範囲を変更したり追加のガイダンスを提供したりできます。

次の概念を確認しました。

- ミッション コントロールとエージェントページを使って cloud agent のタスクを監視しました。
- 実行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、次は作成されたプルリクエストを[反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
