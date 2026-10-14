---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更の計画、コードの作成、動作のテスト、プルリクエストのレビュー、エージェントを使った作業管理を反復して行います。

## シナリオ

開発者をテーマにしたボードゲームのクラウドファンディングを提供する架空の会社、Tailspin Toys に新しく参加した開発者として作業します。チームのバックログは GitHub Issue として登録済みで、すぐに取り組めます。フィルターやページネーションなどの機能開発に加え、アクセシビリティやコーディング規約などの品質改善も含まれています。

## 開発環境を選ぶ

GitHub Copilot は、普段使う開発環境で利用できます。自分に合った環境を選んでください。どのワークショップも独自のセットアップから始まり、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示、スキル、カスタムエージェント、Playwright MCP、Agent Merge、実用的なスラッシュコマンドを扱います。

### [GitHub Copilot app][app]

デスクトップアプリでエージェントセッションを並行して実行し、セッションモードを切り替え、Canvas で共同作業し、GitHub Issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業を案内し、進行状況を監視し、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。Copilot Chat のエージェントモード、MCP サーバー、カスタムエージェントをエディターから離れずに活用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
