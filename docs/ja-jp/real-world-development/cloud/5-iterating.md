---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善や機能追加に関する複数のタスクで GitHub Copilot と作業してきました。コードへのドキュメント追加、デザインチームが改善を続けるための関連ゲーム機能の構築、ハイコントラストモードとライトモードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送り、作業を改善しましょう。

### シナリオ

繰り返し説明してきたとおり、生成 AI を導入してもソフトウェア設計と開発・運用 (development and operations、DevOps) の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスを進める必要があります。チームのほかのメンバーにレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した変更を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期で、監視を受けずにタスクを実行するため、安全を保つためのセキュリティ制限があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みアクセスは作業に使用するブランチに**のみ**付与されます。
- cloud agent は GitHub Actions 内で実行され、作業用に独立した一時的な環境を作成します。
- GitHub Actions のワークフローを実行するには、人間による承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が最初に作成したプルリクエスト (PR) を確認します。これはコードへのドキュメント追加です。GitHub.com の通常の PR インターフェイスを使って作業します。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールによってブロックされたという警告が表示されることがあります。Copilot は外部 MCP サーバーへの呼び出しを含め、既定では外部リソースへのアクセスが制限されているため、これは**想定された動作**です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択してプルリクエスト一覧を開きます。
3. **Add missing documentation** のようなタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が付いています。その場合は作業が完了するまで待ちます。数分かかることがあるため、休憩を取ったり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら、**Files changed** タブを選択して変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc ドキュメントコメントなど、更新されたコードを確認します。具体的な変更内容は異なる場合があります。

   変更を確認するときは、TSDoc コメント、TypeScript 規約、コメントヘッダーを探します。これらは演習 2 の最初に確認したカスタム指示ファイルによるものです。

6. 更新をレビューして問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示す表示があるか確認します。
8. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローがキューに入ったことが表示されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了には数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業するのは一方通行ではありません。ほかのチームメンバーと同じように、プルリクエストへのコメントやコードのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応する新しいセッションを開始します。結果は決定論的ではないため、依頼する文章をそのまま指定することはできません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、ファイルの内容を簡潔に説明するコメントヘッダーを追加する。
- TypeScript ファイルと Astro ファイルに TSDoc ドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の案などを使って、生成されたドキュメントの変更を依頼するコメントを追加し、ユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択して、Copilot が作業する様子を確認します。Copilot は更新用の新しいセッションを開始します。
3. **Back to pull request** を選択するとプルリクエストに戻れます。

   ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択し、変更をレビューします。

納得できるまで反復して改善してください。問題がなければ、ドラフトからレビュー可能な状態に変更して `main` ブランチにマージできます。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対して Copilot が作成した PR に戻り、変更を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** のようなタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新をレビューして問題がなければ、**Conversation** タブに戻って下にスクロールします。
6. ワークフローの承認待ちを示す表示があるか確認します。
7. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄にワークフローがキューに入ったことが表示されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了には数分かかる場合があります。
9. **省略可能:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます。例: **copilot/fix-8**)。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を構築しました。前と同じように、Copilot cloud agent に更新を依頼して反復できます。たとえば、表示する関連ゲームの数の変更や、コメントヘッダーと TSDoc コメントの追加を依頼できます (この Issue を割り当てた時点では、カスタム指示をまだ更新していなかったことを思い出してください)。前と同様、**Conversation** タブでコメントを追加すると Copilot が確認し、新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントが実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 でミッション コントロールから依頼したライトモードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** のようなタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が付いています。作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - モードを切り替える UI コンポーネント。
   - ユーザー設定を保持するためのローカル ストレージの使用。
   - ハイコントラストモードとライトモードの Cascading Style Sheets (CSS) またはスタイル変更。
   - アクセシビリティ属性 (Accessible Rich Internet Applications (ARIA) ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新をレビューして問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示す表示があるか確認します。
8. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローがキューに入ったことが表示されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了には数分かかる場合があります。
10. **省略可能:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます)。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   その後、アプリケーションを起動して、ハイコントラストモードとライトモードの切り替えが期待どおりに動作し、ページの再読み込み後も設定が保持されることをブラウザーで確認します。

アクセシビリティ用カスタムエージェントが、アクセシビリティのベストプラクティスに従って機能の実装を Copilot に促したことが分かります。アクセシビリティ上の懸念や改善点があれば、前の PR と同じようにコメントで **@copilot** にメンションして修正を依頼できます。

## 省略可能な演習 — 引き続き作業を委任する

cloud agent に実際のバックログ項目を任せ、その間に別の作業に取り組むのが効果的です。この方法に慣れるため、リポジトリに Issue を追加して Copilot に割り当ててみましょう。たとえば、次のようなタスクがあります。

- ゲーム詳細ページに支援者の関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent のハーネスを完了しました。これらのレッスンでは、次のことを行いました。

- **リポジトリに含まれるカスタム指示ファイルを調べ、後で cloud agent の出力への影響を確認できるようにしました。**
- **Issue を Copilot cloud agent に割り当て、環境のセットアップ、計画、非同期の実行を確認しました。**
- **アクセシビリティ用のカスタムエージェントを作成して使い、ハイコントラストモードとライトモードの切り替えを追加しました。**
- **Copilot agents ページをミッション コントロールとして使い、アクセシビリティのセッションを監視して途中で方向付けました。**
- **Copilot のプルリクエストをレビューして反復し、変更を依頼するために `@copilot` をメンションし、ワークフローを承認しました。**

## レビューと次のステップ

Cloud agent のハーネスを完了しました。引き続き学ぶ場合は、ほかのハーネスで今回の内容を補完できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェントモードと MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェントスキル、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent に戻るオプションの `/delegate` を使い、ターミナルからレビュー済みの 2 つの変更をリリースします。

自分のリポジトリでは、次のアイデアを試してください。

- リファクタリングやテストカバレッジに関する Issue を cloud agent に割り当てる。
- 別の分野向けにカスタムエージェントを作成する。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
