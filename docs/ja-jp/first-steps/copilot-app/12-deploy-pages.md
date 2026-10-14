---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app で Pages ワークフローを構成し、プルリクエストをレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages でデプロイして、Space Quiz を公開しましょう。Copilot はワークフローを準備し、デプロイを案内できますが、変更をレビューしてサイトが公開されたことを確認するのは開発者の責任です。

このレッスンでは、次のことを行います。

- Copilot に GitHub Pages のデプロイを構成してもらいます。
- マージする前にワークフローとプルリクエストをレビューします。
- デプロイの進行を確認し、公開されたサイトを検証します。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、ほかの人が簡単に試せる方法がありません。単一ファイルのサイトを GitHub Pages で公開すると、サーバーや実行時の依存関係を追加せずに、公開 URL からアクセスできるようになります。

## Copilot にサイトのデプロイを依頼する

`space-quiz` の Copilot app セッションで、GitHub Pages を設定してサイトを公開するよう依頼します。

1. 次のプロンプトを送信します。

   ```plaintext
   Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
   ```

![3 段階の図: マージ済みの space-quiz プルリクエスト、Build website、Upload artifact、Deploy to Pages の各ステップが成功した GitHub Actions の Deploy website 実行、yourname.github.io/space-quiz/ で公開された Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> このプロンプトは、エージェントにプルリクエストの作成とマージ、およびリポジトリの Pages 設定の変更を依頼します。続行する前に、差分、デプロイ権限、GitHub の承認要求を確認してください。

## ワークフローをレビューしてデプロイを検証する

ワークフローと公開サイトを確認し、変更が期待どおりにデプロイされたことを検証します。

1. リポジトリに README がない場合に限り、README が追加されていることを確認します。
2. ワークフローのビルド手順とデプロイ手順を含め、プルリクエストの変更をレビューします。
3. 必須のプルリクエストチェックが成功した後にのみマージします。
4. GitHub Actions のデプロイ実行が成功するまで進行を確認します。
5. 報告された URL を開き、Space Quiz が読み込まれることを確認します。

エージェントが Pages を有効にできない場合やプルリクエストをマージできない場合は、提示された具体的な手順に従ってください。デプロイが成功し、公開サイトが読み込まれるまでは、タスクが完了したと判断しないでください。

## まとめと次のステップ

Space Quiz が公開サイトとして利用可能になり、エージェントの報告に頼らずデプロイを確認できました。続けて[レッスン 13: 振り返りと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
