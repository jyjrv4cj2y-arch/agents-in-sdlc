---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいる組織は少なくありません。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能要求などがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加といったタスクを自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフトのプルリクエスト (PR) を作成します。手間のかかる作業を委任して開発を加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次のことを確認します。

- コード生成に使う環境をカスタマイズする方法。
- 操作を安全に行う方法。
- Issue の範囲を明確に定めることの重要性。
- Issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には対応したい技術的負債があります。最初のサイトを作成した委託業者は、ドキュメントを十分に残していません。まず、アプリケーションのエクスポート関数に TSDoc のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属するほかのゲームを表示し、閲覧を続けられるようにします。完成度の高い実装はまだ必要なく、ユーザー体験 (UX) の受け入れテストに使えるものを求めています。これは現在の障害になっていますが、優先度がさらに高い Issue がほかにあります。

このような作業は後回しになりやすく、Copilot cloud agent に割り当てるのに適しています。cloud agent が非同期で作業している間、開発者は別のタスクに取り組み、後から戻って変更をレビューできます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。たとえば、[GitHub Issue を Copilot に割り当てる][assign-issue]方法があります。割り当てると、Copilot は進行状況を追跡するドラフトのプルリクエストを作成し、環境をセットアップして、タスクに取り組みます。作業中または完了後に Copilot のセッションを確認できます。提案された解決策がレビューできる状態になると、Copilot がプルリクエストで開発者に通知します。

## 範囲を適切に定めた指示の重要性

GitHub Copilot に魔法はありません。考える手順を省けるわけではありません。一見簡単な操作でも、詳細を確認すると複雑な場合があります。[Copilot cloud agent にタスクを依頼するときは、範囲を慎重に定めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階的に進め、学び、試し、必要に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot のガイドに使う方法を確認しました。カスタム指示のハンズオンを既に完了している場合は、`.github/copilot-instructions.md` の動作を見ています。まだ確認していない場合は、ここで簡単に見ておきましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認すると、後でその影響を把握できます。

リポジトリの GitHub Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript 規約と TSDoc ドキュメントコメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` frontmatter が `**/*.test.ts` ファイルに指示を適用していることを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てるとき、生成されたプルリクエストに TSDoc コメント、TypeScript 規約、コメントヘッダーが含まれるか確認してください。これらの指示ファイルが反映されます。後の演習で PR をレビューするときにも、この点を取り上げます。

## Copilot cloud agent 用の開発環境をセットアップする

誰がコードを作成する場合でも、通常は適切な環境とセットアップスクリプトが必要です。Copilot にタスクを割り当てる場合も同様です。Copilot はソフトウェア エンジニア (software engineer、SWE) に似た方法で作業します。

cloud agent は作業環境に [GitHub Actions][github-actions] を使います。作業開始前に実行する[セットアップ ワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成して、必要な開発ツールと依存関係を利用できるようにできます。ラボの流れとこの学習機会を支援するため、この設定は事前に用意されています。単一の Astro アプリに必要な Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションとシードを済ませた SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *この手順*に必要な最小限のアクセス許可を設定します。Copilot は操作用に独自のトークンを取得します。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は、`contents: read` 権限が必要です。ここでクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL になりました)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを使用できるよう、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

通常の GitHub ワークフローと同様ですが、いくつか重要な点があります。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは Copilot がプルリクエストの作業を始める前に、GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot の実行を待たずに、ワークフローが正常に動くか確認するのに便利です。

## ドキュメントを追加する

ドキュメントが重要だと誰もが理解していますが、古い情報しかない、またはドキュメントがまったくないプロジェクトも少なくありません。このような技術的負債を放置すると、生産性が下がり、コードベースの保守や新しい開発者の受け入れが難しくなります。Copilot はドキュメントの作成に役立つため、この Issue を Copilot cloud agent に割り当てます。バックグラウンドで必要なドキュメントを作成します。後の演習で成果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して新しい Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

   ![Copilot の割り当ての詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。一覧に新しい PR がない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が TODO リストを作成したことを確認します。

> [!NOTE]
> Copilot の TODO リストが PR に表示されるまで、数分かかることがあります。Copilot は環境を作成し (前述のワークフローを実行)、プロジェクトを分析して、問題に取り組む方法を決めています。

13. リストと、Copilot が実行する予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された状態を表示するには、ウィンドウを更新する必要がある場合があります。

16. ライブセッションをスクロールして、Copilot の作業を確認します。コードと現在の状態を調べ、適切な計画を考えて選び、コードを作成する過程が表示されます。

この作業には数分かかる場合があります。Copilot cloud agent の主な目的の 1 つは、非同期にタスクを実行して開発者がほかの作業に集中できるようにすることです。この機能を活用するため、もう 1 つのタスクを Copilot cloud agent に割り当ててから、アプリケーションに機能を追加する作業に移ります。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の大きな利点の 1 つは作業を分担できることです。デザインチームのために関連ゲームのセクションを追加する作業は短時間で終わるかもしれませんが、その時間を別のタスクに使うこともできます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Issue の最初のコメントに 👀 が表示され、Copilot が作業中であることが分かります。

![Copilot が 👀 の絵文字で Issue の作業中であることを示す](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の要求にも取り組んでいます。Copilot cloud agent はソフトウェア エンジニア (SWE) に似た方法で作業するため、常に監視する必要はありません。完了後にレビューします。ここからはカスタムエージェントの作成と使用に進みます。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]を確認しました。cloud agent に Issue を割り当てると、非同期に作業を実行できます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などを任せられます。

次の概念を確認しました。

- コード生成に使う環境をカスタマイズする方法。
- 操作を安全に行う方法。
- Issue の範囲を明確に定めることの重要性。
- Issue を Copilot に割り当てる方法。

cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も使えます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
