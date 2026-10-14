---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用できるカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連の作業に取り組みました。具体的には、次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、登録済みのフィルター機能の Issue を見つけました。
- 星評価を追加し、転送したブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター機能の Issue から作業を始め、Plan モードでアプローチを定義し、Autopilot モードで構築して、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの確認内容を評価しました。
- フィルター機能の変更全体をレビューし、フィルター機能の PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、クラウドへの任意の委任に使うスラッシュコマンドを確認しました。

## リリースした内容

このワークショップでは、更新済みの `main` からそれぞれブランチを作成し、2 つの PR を作成しました。

1. **星評価**: 既存の `starRating` と、評価が設定されていないことを示す状態をゲームカードに表示します。
2. **フィルター機能と品質ワークフロー**: フィルターを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して関連するテストを追加します。

フィルター機能の計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を通じた Copilot のブラウザー確認など、複数の方法でコードを検証しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、追加した形式で結果を報告します。QA は要件とテストカバレッジのレビューにそれらの結果を組み合わせ、PR 作成前の確認を支援します。

追加するテストは、実際に存在する不足を補うものにします。QA の実行時にテストを追加する必要がない場合もあります。マージを許可する前にコードと証拠をレビューし、変更を加えた後は関連する証拠を更新してください。

## ベストプラクティス

Copilot に提供するコンテキストとツールが、作業の結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズは複数の会話で再利用し、チームのニーズに合わせて調整できます。指示は標準を定め、スキルは繰り返し実行するタスクを説明し、カスタムエージェントは専門の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は実装前の検討に、**Interactive** は範囲を絞った変更を確認しながら進める場合に、**Autopilot** は範囲が明確なタスクに適しています。日常的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。何を構築するのか、なぜ必要なのか、どのように進めるのかを明確に説明すると、結果が大きく変わります。

## さらに試す

中心的なワークフローは完了しました。次の CLI 機能も試してみてください。

- `/review`: コードレビューエージェントに変更の分析を依頼します。
- `/rubber-duck`: 問題について対話し、別の視点を得ます。
- `/fleet`: 独立したサブタスクを並列に調整します。
- `/worktree`: 別のタスクを分離します。
- `/delegate`: タスクを Copilot cloud agent に送信します。

## 次のステップ

ツールを上達させる一番の方法は、使い続けることです。本番コード、趣味のコード、長年思い描いていた小さなアプリなどで活用してください。チームで知見を共有し、互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys をさらに拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの開発環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
