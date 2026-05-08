# ARCHITECTURE

## System Context

- Frontend: Chrome Extension MV3 content script / page UI
- Backend/API: Local Node.js bridge for browser-to-Codex app-server communication
- Database: None for MVP
- AWS: None for MVP
- External Services: Cloud Samurai page DOM, local Codex app-server

## Design Principles

- 疎結合を保つ
- 最小権限を意識する
- 再現可能なデプロイを目指す
- 障害時に原因を追いやすい構成にする

## Main Components

### app

Chrome 拡張とローカル bridge を置く。
Chrome 拡張は Cloud Samurai の解答後ページにボタンと解説表示領域を追加し、DOM から学習コンテキストを抽出する。
ローカル bridge は Chrome 拡張から localhost で受け取り、Codex app-server へ stdio 経由で解説生成を依頼する。
不具合と不明点は `docs/tracking/app/` で管理する。

### infra

MVP では AWS / IaC は持たない。
将来、公開 API、認証、課金、共有機能が必要になった時点で責務を再定義する。
不具合と不明点は `docs/tracking/infra/` で管理する。

### scripts

セットアップ、検証、デプロイなどの入口を置く。
エージェントが迷わないように、入口名は安定させる。

### docs

初期ビルド、技術選定、タスク、課題管理、設計改訂をまとめる。
実装だけ進んで文書が置いていかれないよう、更新責務をここで持つ。

## Operational Considerations

- ログ出力方針: ローカル bridge は秘密情報や問題全文を不要に永続化しない
- 監視対象: MVP ではローカル実行時の console / terminal 出力
- アラート方針: MVP では対象外
- デプロイ方針: Chrome の unpacked extension とローカル bridge 起動で検証する
- ロールバック方針: 拡張を Chrome から無効化し、ローカル bridge を停止する

## MVP Flow

1. ユーザーが Cloud Samurai の解答後ページを開く
2. content script がページ内に「解説して」ボタンを追加する
3. ボタンクリック時に問題文、選択肢、正解、既存解説、ユーザー解答状態を DOM から抽出する
4. Chrome 拡張が local Node.js bridge に抽出結果を送る
5. bridge が Codex app-server に最小プロンプトで解説生成を依頼する
6. Chrome 拡張が生成結果を受け取り、ページ上のパネルに表示する

## Change Management

- 設計変更が発生したら、関連する `BUG-*` または `Q-*` の ID を改訂履歴へ残す
- 変更理由が暫定対応か恒久対応かを区別して書く
- 技術選定変更時は `docs/TECH-STACK.md` と一緒に更新する

## Revision History

| Date | Version | Summary | Related IDs |
| --- | --- | --- | --- |
| 2026-04-11 | 0.1 | 初期テンプレート作成 | - |
| 2026-04-11 | 0.2 | 課題管理と設計改訂の連動ルールを追加 | - |
| 2026-05-08 | 0.3 | Chrome 拡張とローカル bridge による MVP 構成を追加 | Q-APP-001, Q-INF-001 |
