---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または構成し、ブラウザーでフィルター機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまで説明したように、コードを書くことだけが開発ではありません。データや外部サービスを扱い、Copilot が追加の自動化を利用できるようにする必要もあります。そのために使うのが MCP サーバーです。MCP サーバーを使うと、Copilot は CLI に組み込まれた機能以外のツールやサービスも利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を確認します。
- Playwright MCP サーバーがまだ利用できない場合は追加します。
- エージェントにブラウザーを操作させ、フィルター機能を試します。

## シナリオ

ユニットテストやエンドツーエンドテストは重要ですが、UI の更新を検証するには、実際に UI を操作する必要があります。ユーザーと同じように Copilot が作業中の Web サイトを使えるようにすると、変更が期待どおりに動くかを確かめる手順を自動化できます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP によって、エージェントは最新情報にアクセスし、代わりに操作を実行できます。

エージェントは、外部のツールやサービスとの橋渡しをする MCP サーバーを通じて、ツールとリソースにアクセスします。それぞれの MCP サーバーは、エージェントが利用できるツールとリソースのセットを提供します。

よく使われている MCP サーバーの例を次に示します。

- **[GitHub MCP Server][github-mcp]**: GitHub のリポジトリ、Issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内の MCP サーバーを見つけやすくし、貢献を促進するための [MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクトのほかの依存関係と同じように扱ってください。利用前にソースコードを確認し、公開元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーのように Web サイトを操作できるように追加します。

> [!NOTE]
> Copilot CLI に Playwright MCP がすでに構成されている場合は、セットアップ手順をスキップして次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを実行して、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類が **STDIO** であることを確認し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright を使って機能を確認するよう Copilot に依頼する

これまでは、機能が期待どおりに動くことを手動で確認していました。次は Copilot から利用できる Playwright MCP サーバーを使って、同じ確認を行います。

1. 次のプロンプトを使って、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常、Copilot は MCP サーバーを自動的に選ぶため、明示的に指定する必要はありません。ただし、使用するサーバーが分かっている場合は、プロンプトで指定すると結果がより一貫し、トークンの使用量も抑えられます。

2. Copilot がブラウザーで機能を確認する操作を行う様子を確認します。
3. 結果のレポートを読み、すべて期待どおりに動作しているかを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認しました。具体的には、次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を確認しました。
- Playwright MCP サーバーがまだ利用できない場合は追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を試しました。

次は、スキルとブラウザーツールを組み合わせる[QA カスタムエージェント][next-lesson]を作成します。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
