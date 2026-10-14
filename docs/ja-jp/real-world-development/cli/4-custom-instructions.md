---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント作成の標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときはコンテキストが重要です。タスクを特定の方法で行う必要がある場合、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]では、必要なコードだけでなく、その構成方法も指定できます。フィルター機能を構築したので、Copilot が使った指示を確認し、ドキュメント作成の標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパスに対応した指示ファイルがエージェントに届く仕組みを確認します。
- コーディング標準を徹底するために指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には開発プラクティスに関するガイドラインがあります。

- コメントでは、コードの内容を繰り返すのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。該当する場合は、注入可能な `db` 引数も含めます。
- 再利用可能な Astro コンポーネントには `Props` の契約を記述し、関連コードが変更された場合はコメントも最新の状態に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使って、これらのプラクティスに沿ったタスクを Copilot に実行させます。

## 指示ファイル

カスタム指示を使うと、コンテキストや好みを Copilot に伝えられます。コーディング規約、ライブラリ、コメントの書き方などを指定できるため、より適切な提案やコード スニペットを得られます。リポジトリ全体、または特定の種類のファイルに適用する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md`: リポジトリへのすべての要求で Copilot に送られる、単一の指示ファイルです。多くの要求に関係するプロジェクト全体の情報を記述します。
- `.github/instructions/*.instructions.md`: 特定の言語、ファイルの種類、タスクに対するガイダンスを記述します。

> [!NOTE]
> ほかの指示形式とそのサポート状況は環境によって異なります。特定の形式を使う前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトには指示ファイルが用意されています。変更を加える前に内容を確認し、その影響を理解しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく)、`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングのガイダンスを確認します。この指示は、このリポジトリで Copilot と行うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどに対する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭にある `applyTo` フィールドが、指示の適用対象ファイルを決める glob を設定していることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存のプロジェクトファイルへのリンクを確認します。これにより、大きな指示を再利用できる小さなファイルに分割し、参考にする例を Copilot に示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルは出発点として役立ちますが、TypeScript の新しいコードに TSDoc コメントを追加する指示が不足しています。コアの `copilot-instructions.md` を更新します。

1. `.github/copilot-instructions.md` で、35 行目あたりにある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存したら、フィルター機能のコンテキストを保ったまま会話を終了して再開し、新しいガイダンスを読み込ませます。

1. フィルター機能の会話がまだ開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   メッセージが表示されたら、フィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 次のプロンプトで、更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、その説明がコードに対して正確であることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルのコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの指示とパスに対応した指示ファイルがエージェントに届く仕組みを確認しました。
- コーディング標準を徹底するために指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用できる `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストを一貫して実行できるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
