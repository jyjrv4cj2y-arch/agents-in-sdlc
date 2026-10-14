---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルター機能を構築しました。プルリクエストを開く前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続して Copilot に実際のブラウザーを操作させ、機能をテストします。その後、ブランチを公開して PR を開きます。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを確認します。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot に Playwright MCP を使ってブラウザーでフィルター機能を手動テストしてもらいます。
- ブランチを公開し、フィルター機能のプルリクエストを開きます。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部にもアクセスできると、さらに強力になります。Copilot が外部の機能を利用するための標準的な方法が Model Context Protocol (MCP) です。MCP を使うと、エージェントは外部のツールやサービスと通信できます。

![エージェントモードの内部と、MCP サーバーや VS Code 拡張機能が提供するツールを含むコンテキスト、大規模言語モデル (LLM)、ツールとのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP を通じて、エージェントはリソースを使って最新情報にアクセスし、ツールを使って代わりに操作を実行できます。

エージェントは、外部のツールやサービスとの橋渡しをする MCP サーバーを介して、ツールとリソースにアクセスします。MCP サーバーは、エージェントと外部ツール (既存のアプリケーション プログラミング インターフェイス (API) や NPM パッケージなどのローカルツール) との通信を管理します。それぞれの MCP サーバーは、エージェントが利用できるツールとリソースのセットを提供します。

よく使われている MCP サーバーの例を次に示します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。AI エージェントは、新しいリポジトリの作成、既存リポジトリの更新、Issue やプルリクエストの管理などを行えます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。AI エージェントは、Web ページへの移動、フォームへの入力、ボタンの選択などを行えます。

ほかにも、さまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub は、エコシステム内で MCP サーバーを見つけやすくし、貢献を促すための [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティに関しては、MCP サーバーをプロジェクトのほかの依存関係と同じように扱ってください。利用前にソースコードを注意深く確認し、公開元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使用し、機密リソースや操作へのアクセスを許可する場合は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーが既に構成されていることを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この `github` エントリはプロジェクトのテンプレートに含まれています。そのため、前の演習で Copilot が Issue のバックログを確認できました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルへのインストールは不要です。VS Code は HTTP で接続し、Copilot が初めてツールを使うときに GitHub の認証を行います。

## Playwright MCP サーバーを追加する

次は 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot が操作できるブラウザーを提供します。機能のテストに利用します。

1. `.vscode/mcp.json` で `github` と並ぶよう `playwright` エントリを追加し、ファイルを次のようにします。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル**サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、Playwright は表示ウィンドウなしでブラウザーを実行します。デスクトップ画面がない Codespace では、この指定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンドテストに Playwright を使っているため、必要なブラウザーは通常インストール済みです。Copilot がブラウザーがないと報告した場合は、`npx playwright install chromium` を実行するよう依頼して、もう一度試します。

## サーバーを起動して信頼する

VS Code は必要になったときに MCP サーバーを起動します。Copilot がサーバーのツールを初めて使うときにサーバーを起動し、信頼するかどうかを確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** アクションが表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code がサーバーを信頼するよう求めたら、構成を確認して信頼を許可し、サーバーを起動します。

手動での起動は省略できます。その場合、次の手順で Copilot が初めてツールを必要としたときに VS Code がサーバーを起動し、信頼を確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。開発者がアプリを操作して確認する代わりに、エージェントがページを開き、移動し、フィルターを適用し、結果を読み取って要約できます。会話を離れずに、機能が期待どおりに動くかを手早く確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。そのため、エージェントは支援技術と同じように、ラベル付きの構造化された要素 (ボタン、リンク、リスト項目) を調べます。簡単な機能テストとともに、アクセシビリティの基本的な確認もできます。

サーバーを接続してアプリを実行した状態で、Copilot にフィルター機能を試すよう依頼します。

1. 次のプロンプトを Copilot に送信します。

   ```text
   Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

   1. Go to the games page and note how many games are listed.
   2. Apply a category filter and confirm the list updates to only show games in that category.
   3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
   4. Combine a category and a publisher filter and confirm the results respect both.

   Report what you observe at each step, and call out anything that does not behave as expected.
   ```

Copilot は Playwright MCP サーバーを介してブラウザーを起動し、各手順を実行して結果を報告します。Issue の受け入れ条件と照らして結果を読みます。問題があれば、追加の質問をするか、プルリクエストを開く前にコードを修正するよう依頼します。

> [!NOTE]
> このテストにはアプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止している場合は、プロンプトを送る前に再起動します。Copilot が Playwright MCP サーバーを初めて使うとき、ブラウザーをダウンロードする必要がある場合があります。ブラウザーがないと報告した場合は、`npx playwright install chromium` を実行するよう依頼して、もう一度試します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたら、チームがレビューできるようプルリクエスト (PR) を開きます。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が加えた変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンでコミットメッセージを生成します。

   ![Copilot が加えた変更を表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

プルリクエストは github.com や GitHub コマンド ライン インターフェイス (CLI) など、複数の方法で作成できます。GitHub Copilot を利用しているので、PR の作成も依頼しましょう。Copilot は関連 Issue を見つけ、Issue に関連付けた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェントのドロップダウンで **Agent** が選択されていることを確認し、Copilot が GitHub ツールを利用できるようにします。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、Copilot が情報を収集して操作を実行できるようにします。GitHub ツールを初めて使うときに GitHub の認証が求められる場合があります。画面の指示に従って許可します。
5. Copilot が Issue を検索して適切なものを見つけ、PR を作成したことを確認します。
6. Copilot が生成したリンクを選択してプルリクエストをレビューします。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを確認しました。
- Playwright MCP サーバーをワークスペースに追加しました。
- Playwright MCP を使い、ブラウザーでフィルター機能を手動テストしました。
- ブランチを公開し、フィルター機能のプルリクエストを作成しました。

MCP サーバーを使って機能を**テスト**しましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次は、アクセシビリティレビューなど、範囲を絞ったタスクを効率化する[カスタムエージェント][next-lesson]を作成します。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
