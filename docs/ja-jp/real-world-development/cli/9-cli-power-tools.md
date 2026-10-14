---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、クラウドへの任意の委任に役立つスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの優れた CLI ツールと同様に、GitHub Copilot CLI には操作用のスラッシュコマンドが多数あります。これらのコマンドを使うと、高度な機能、内部の情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。次は、ほかの便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` を使って、利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を確認します。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認します。

## シナリオ

CLI の中心的なワークフローを終えたので、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加の機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルのコンテキストウィンドウの上限に達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュコマンドを使って状態を確認したり、自分で圧縮したりできます。

1. Codespace に戻り、Copilot CLI がまだ開いていない場合はリポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように分割されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話がすでに短い場合は、大きな違いが見られないことがあります。

> [!NOTE]
> コンテキストウィンドウがいっぱいになると、Copilot CLI は自動的にコンテキストを圧縮します。タイミングを自分で選ぶ場合は `/compact` を使います。関係のない別のタスクに移るため新しい会話が必要な場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに強みがあり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルの一覧を表示して使うモデルを選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況に関する情報を確認します。
3. 現在のモデルをそのまま使うか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、知見を共有すると、AI ツールの活用方法を互いに改善できます。`/share` コマンドでは、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みバージョンで利用できる `/share` のオプションを確認します。
2. セッションを共有する場合は、ローカルの Markdown ファイルに出力する `/share file` など、目的に合った方法を選びます。
3. エクスポートした内容を、送信または公開する前に確認します。セッションの出力には、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有する予定のないリポジトリや会話の内容は公開しないでください。

## 省略可能: 作業を並行化または委任する

ワークショップの中心的な内容は完了しました。Copilot CLI には、大きなタスクに役立つコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに割り当てて並列実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、非同期で作業し、プルリクエストを作成する場合もある Copilot cloud agent にタスクを送信できます。

追加の worktree やリモート作業が発生する可能性があるため、これらのコマンドは任意です。試す場合は、新しく範囲を明確にしたタスクを開始し、通常のワークフローに沿って結果をレビューしてください。非同期エージェントの作業を詳しく確認する場合は、[cloud agent のワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、CLI を構成し、セッションを共有し、内部の状態を確認できます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` を使って、利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を確認しました。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

Copilot CLI には、ほかにも多くのスラッシュコマンドがあります。次は、これまでの内容と今後の学習方法を[振り返ります][next-lesson]。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
