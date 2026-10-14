---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときはコンテキストが重要です。タスクの進め方に指定がある場合や、Copilot が知っておくべき背景情報がある場合は、その情報を参照できるようにします。[指示ファイル][instruction-files]を使うと、何をするかだけでなく、どのように進めるかも Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに対応した指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に届く仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここでは変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue に対して非同期に作業するため、影響をリアルタイムで並べて示すのは困難です。このハーネスの後半で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認できます。

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
> GitHub Copilot を使うプロジェクトでは、充実した指示ファイルを用意すると役立ちます。このプロジェクトの指示を確認すると、[ユーザーインターフェイス (UI) の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク用のファイルが見つかります。
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
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` frontmatter を確認します。これは、指示を `**/*.test.ts` などの TypeScript テストファイルに適用する glob を指定します。
5. このプロジェクトでユニットテストを作成するときの指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて末尾までスクロールします。`unit-tests.instructions.md` などほかの指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。これにより、大きな指示を再利用できる小さなファイルに分割し、Copilot に参考例を示せます (パスはリポジトリのルートではなく指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメント作成はまだ必須ではありません。次の手順で TSDoc コメントとファイルヘッダーのルールを追加します。

## リポジトリ全体に適用する標準を追加する

ここで行う編集は 1 つです。コード内のドキュメントを TSDoc コメントとファイルレベルのコメントヘッダーで記述するルールをプロジェクト全体に追加します。後の演習で Issue を割り当てると、cloud agent はこの標準に従います。

編集する前に、新しいブランチを用意します。cloud agent は Issue の対象ブランチから指示を読みますが、ここでは変更を整理してコミットします。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` はプロジェクトレベルの情報を Copilot に提供します。コードの提案を改善するため、リポジトリのコーディング標準を明記しましょう。

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

## 指示の更新をコミット、プッシュ、マージする

cloud agent は Issue が対象とするブランチから指示ファイルを読みます。次の演習で Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が変更した指示を読み込めるように、`main` に反映する必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。次の演習で Issue に割り当てると、cloud agent は更新した指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチを省略して `main` に直接コミットできます。実際のプロジェクトに近い手順にするため、このワークショップではブランチを作成しています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の新しい標準を追加しました。このハーネスの後半で、cloud agent が作成するプルリクエストにその標準が反映されているかを確認します。

次は、最初の Issue を [Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
