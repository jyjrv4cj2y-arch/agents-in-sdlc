---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストなどの操作を実行する必要があります。開発者は必要なファイルを探して変更し、テストを実行し、デバッグし、見落としたファイルを特定してさらに変更する、といった作業を繰り返します。必要な作業はそれだけではありません。

このような作業に役立つのが Copilot Agent Mode です。

Copilot Agent Mode は IDE 内でより自律的に動作します。開発者と同様に、まず既存のプロジェクト構成を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode でサイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる種類の操作向けに設計されています。エージェントモードは IDE 内で作業し、素早いフィードバックと対話を可能にします。cloud agent はチームメンバーのように非同期で作業し、Issue とプルリクエストを通じて開発者とやり取りします。

この演習では、次の方法を確認します。

- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定し、連携した変更を加える。
- GitHub Copilot Agent Mode で UI とデータ層にまたがる新機能を実装する。
- コードベースにマージする前に、Agent Mode が生成した変更とテストをレビューする。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリと発行元で絞り込めるようにします。前の演習では発行元のヘルパーを追加しました。ここでは Copilot Agent Mode を使い、残りのデータ層、UI、テストの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認して、現在の機能を把握します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲーム一覧と詳細を表示できます。サイトは単一の Astro アプリで、ビルド時にページを静的 HTML としてレンダリングします。ページは Drizzle オブジェクト リレーショナル マッパー (ORM) を通じて frontmatter 内からローカル SQLite データベースを直接問い合わせます。独立したバックエンド アプリケーション プログラミング インターフェイス (API) やクライアント側のユーザーインターフェイス (UI) フレームワークはありません。再利用可能なデータアクセス ヘルパーは `src/lib/` にあり、インタラクティブな動作は標準ドキュメント オブジェクト モデル (DOM) API を使う小規模でスコープを限定した Astro `<script>` で追加します。

### Web サイトを起動する

`npm run dev` タスクを使うと、単一の Astro 開発サーバーを起動できます。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習を終えていれば `main` に戻っています。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、Codespace 内に新しいターミナルを開きます。
3. フィルター機能用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが準備できると、次のような URL を示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くか尋ねるダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルから localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Codespace へのプライベートトンネルで、Web サーバーがそこで実行されています。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホームページ**: ボードゲームの一覧とタイトル、画像、説明を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの詳細が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を開始します。モードとモデルの選択は演習によって異なるため、必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加して強化します。まずバックログを確認しましょう。テンプレートからプロジェクトをセットアップすると、Issue のバックログが自動的に作成されています。GitHub Copilot にその一覧を表示してもらいます。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** エージェントはファイル間の変更を自律的に計画して実装し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップ作成時に Claude Sonnet 4.5 を使用したため、手順に含めています。受け取るコード提案に一貫性を持たせ、よい体験にすることを意図しています。ただし、大規模言語モデル (LLM) は確率的に動作するため、ワークショップの記述と異なる提案が表示される場合があります。これは正常であり、想定された動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使う場合がありますが、タスクは同じように完了できるはずです。

3. 次のプロンプトを Copilot に送信し、Issue のバックログについて質問します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot は、ユーザーにとって最も役立つと判断した項目に基づいて、Issue に優先順位も付けています。

## 指示ファイルを確認する

エージェントにコード生成を依頼する前に、Copilot に作業のコンテキストを提供する指示ファイルを確認します。Web サイトに機能を追加する方法が記述された[ユーザー インターフェイス (UI)][ui-instructions]ファイルを使います。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加の方法に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle など、ファイル種類別の指示へのリンク。

> [!TIP]
> 指示ファイルからは、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所からの相対パスです。これにより、指示を再利用し、複雑な内容を管理しやすい小さな単位に分割して、例やテンプレートを提示できます。

## フィルター機能を実装する

フィルター機能を完成させるには、アプリケーションに少なくとも次の 3 つの変更が必要です。

- データ層 (`src/lib/`) の残りのフィルターロジックを追加または調整します。
- フィルターの動作を確認するテストを追加または更新します。
- ゲーム一覧ページを更新してフィルター UI を追加します。

さらに、コードベースにマージする前にテストを実行し、成功させる必要があります。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. 現在の Copilot の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルとして **Claude Sonnet 4.5** が選択されていることを確認します。
5. 次のプロンプトで、バックログにある関連 Issue に基づいて機能を実装するよう Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、目的の機能に関係するファイルを探す様子を確認します。データ層のヘルパー、UI、テストを特定し、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトファイルを調べる様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースの複雑さによっては数分かかる場合があります。作業中にコードエディターに **Keep** と **Undo** が表示されることがあります。Copilot が完了すると、すべての変更に **Keep** または **Undo** が表示されるため、作業中に選択する必要はありません。

7. Copilot から求められたら **Continue** を選択してテストを実行します。

   ![テストの実行を確認するダイアログが Copilot Chat ペインに表示されたスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。問題ありません。Copilot はタスクが完了し、エラーを検出しなくなるまで、コード生成とテストを行き来します。

   ![Copilot Agent Mode の完了したチャット セッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを表示しているブラウザーに戻り、新しい機能を試します。
11. すべてが動作することを確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加する方法を確認しました。次のことを学びました。

- GitHub Copilot Agent Mode でユーザーインターフェイス (UI) とデータ層にまたがる新機能を実装する。
- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定し、連携した変更を加える。
- コードベースにマージする前に、Agent Mode が生成した変更とテストをレビューする。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の演習 — ページネーションを実装する

ゲーム一覧が増えると、ページネーションが必要になります。この演習で学んだことを活用して、Copilot にサイトのページネーションを実装してもらいます。コードを作成するときは、次の点を検討してください。

- 既存の指示ファイルを使うなど、現在のベストプラクティスに従う。
- ページサイズをユーザーが選択できるようにするか、固定値にするかなど、ページネーションの方法を検討する。
- 希望どおりの実装に必要なガイダンスを Copilot に提供するプロンプトを作成する。
- Copilot に変更を依頼してコンテキストを提供し、何度か反復する必要がある場合があります。Copilot と作業するときは通常の進め方です。

## リソース

- [Copilot の Ask、Edit、Agent モード: それぞれの機能と使いどころ][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
