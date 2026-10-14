---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときはコンテキストが重要です。タスクの進め方に指定がある場合や、Copilot が知っておくべき背景情報がある場合は、その情報を参照できるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。特に重要なのが[指示ファイル][instruction-files]です。コード生成に関するガイダンスを提供できます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに対応した指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に届く仕組みを確認します。
- 現在の指示が適用された状態で、フィルター機能の最初のデータ部分 (発行元ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。
- 同じプロンプトを再実行し、生成されたコードに新しい標準が反映されることを確認します。
- 次の演習で更新したガイダンスを Copilot が使えるよう、指示の更新とフィルター機能の一部を `main` にコミットします。

> [!CAUTION]
> 生成されたコードが、設定したすべての標準に従うとは限りません。Copilot は確率的に動作します。この演習では出力の文字列を完全に一致させるのではなく、指示を更新した後に動作がどのように変わるかを確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には開発プラクティスに関するガイドラインがあります。

- データ層には常にユニットテストが必要です。
- UI はダークモードで、現代的な印象にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、ファイルの内容を説明するコメントブロックを追加します。

指示ファイルを使って、これらのプラクティスに沿ったタスクを Copilot に実行させます。

### カスタム指示

カスタム指示を使うと、コンテキストや好みを Copilot に伝えられます。コーディングスタイル、ライブラリ、コメントの書き方などを指定できるため、より適切な提案やコード スニペットを得られます。リポジトリ全体に適用する指示や、特定のファイル種類に適用する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md`: リポジトリへの**すべての**要求で Copilot に送られる単一の指示ファイルです。多くのチャットや CLI の要求に関係する、技術スタック、構築中の内容、ベストプラクティスなど、プロジェクト全体の情報を記述します。
- `.github/instructions/*.instructions.md`: 特定のタスクやファイル種類に対する指示ファイルです。TypeScript や Astro などの言語、UI コンポーネントやユニットテストの作成などのタスクについて、ガイドラインを記述できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使用され、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示と `applyTo` frontmatter がある `*.instructions.md` ファイルを使用します。
>
> また Copilot は、AGENTS.md や CLAUDE.md など、ほかの標準に基づく指示ファイルも[サポートしています][custom-instructions-support]。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく説明することは、このワークショップの範囲外です。ただし、サンプルプロジェクトには代表的な方法が示されています。主なポイントは次のとおりです。

- `copilot-instructions.md` には、構築するもの、プロジェクトの構成、全体に適用するコーディング標準など、プロジェクトレベルのガイダンスを記述します。
- `*.instructions.md` は、ファイルの種類 (ユニットテスト、Astro コンポーネント、データ層など) や特定のタスクに関する指示に使います。
- 自然な言葉で、明確なガイダンスを書きます。コードの適切な例と不適切な例を示します。

指示ファイルの作成方法も AI の利用方法も、決まった正解はありません。試行錯誤を通じて、プロジェクトやシナリオに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、充実した指示ファイルを用意すると役立ちます。このプロジェクトの指示を確認すると、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク用のファイルが見つかります。
>
> Copilot に指示ファイルを生成させることもできます。画面によって方法は異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init`)。該当するレッスンで手順を説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] リポジトリを確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

リポジトリに含まれる指示ファイルを読みます。コアとなる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスも確認してください。これらは Copilot とのあらゆるやり取りに関係します。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。これはリポジトリのルートからの相対 glob で、たとえば `**/*.test.ts` に一致する TypeScript テストファイルに指示を適用します。
5. このプロジェクトでユニットテストを作成するときの指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて末尾までスクロールします。`unit-tests.instructions.md` などほかの指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。これにより、大きな指示を再利用できる小さなファイルに分割し、Copilot に参考例を示せます (パスはリポジトリのルートではなく指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメント作成はまだ必須ではありません。次の手順で TSDoc コメントとファイルヘッダーのルールを追加します。

## 変更用ブランチを作成する

ここではベストプラクティスに従って、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を開始します。モードとモデルの選択は演習によって異なるため、必要な場合は各手順で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルを適用した状態でプロンプトを送信します。その後、指示を更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に示します。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べ、コードを更新します。通常、ヘルパーファイルとそのテストが変更されます。
6. 提案されたヘルパーが、`db` クライアントを最初の引数に取り、発行元の型付き配列を返す型付き関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層の規約によるものです。
7. 提案されたコードに TSDoc ドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。指示を更新した後の**一貫性**の改善がこの演習の目的です。

## リポジトリ全体に適用する標準を追加する

前述のとおり、`.github/copilot-instructions.md` はプロジェクトレベルの情報を Copilot に提供します。コードの提案を改善するため、リポジトリのコーディング標準を明記します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールがないため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加します。ファイルヘッダーと TSDoc コメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリレベル (`.github/copilot-instructions.md`) に置いて全体に適用することも、特定の言語、ファイル種類、タスク向けの `*.instructions.md` ファイルとして作成することもできます。今回追加したドキュメントコメントのルールのようなプロジェクト全体の標準は、リポジトリレベルのファイルに記述します。

## プロンプトを再実行して変化を確認する

同じプロンプトを使い、更新した指示が提案に与える影響を確認します。

1. Copilot Chat に戻り、**New Chat** を選択してバッファーをクリアします。
2. `src/lib/publishers.ts` を選択し直し、Copilot が適切なファイルに注目するようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

これで Copilot に新しいプロジェクト標準を適用させ、次の演習で発展させる実際のコードに反映できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもリポジトリ内のほかの資産と同様に、ソース管理で管理します。更新とブランチをコミットしてリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示の更新とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで指示の更新と、ヘルパーの基礎部分として Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加ファイルが表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルからブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたので、バックログのドキュメント標準に関する Issue と関連付けてプルリクエストを作成します。手動でもできますが、プロジェクトに接続済みの GitHub ツールを Copilot に使わせます。Issue を見つけ、Issue をクローズする PR を作成し、マージするよう依頼します。

> [!NOTE]
> この演習では、Copilot に GitHub 上の操作を依頼します。プロジェクトのテンプレートでは GitHub ツールがワークスペースに接続済みのため、追加のセットアップは不要です。Copilot が初めてツールを使うときに、VS Code から GitHub へのサインインを求められる場合があります。プロンプトに従って許可してください。後の演習では、この接続に使われる Model Context Protocol (MCP) を確認します。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 指示ファイル更新に関係する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の更新と、新しい発行元ヘルパーの基礎部分の両方を説明に含めます。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成する様子を確認します。
5. Copilot に代わって GitHub 操作を実行する **Allow** を求められたら、コマンドを確認して、適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージし、ローカルのブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot に代わって GitHub 操作やシェルコマンドを実行する **Allow** を求められたら、コマンドを確認して、適切な場合に **Allow** を選択します。

GitHub Copilot の助けを借りて、プルリクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat で次の作業を行いました。

- コード生成プロンプトを送信し、*既存の*指示を使った Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映されることを確認しました。
- 次の演習で作業を続けられるよう、指示の更新とヘルパーの基礎部分をコミットして `main` に反映しました。

次は[エージェントモード][next-lesson]で指示を適用し、Copilot にコードベース全体の新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
