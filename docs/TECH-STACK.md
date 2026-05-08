# TECH-STACK

## Purpose

このドキュメントは、使う技術と、その技術に対して AI が何を検証してから完了扱いにするかを管理する。
未確定の技術は仮説として残してよく、決まっていないこと自体を見える化する。

## Current Status

- App Stack: Chrome Extension MV3 + local Node.js bridge + Codex app-server
- Infra Stack: None for MVP
- CI/CD: TBD
- Deployment Target: Local development / unpacked Chrome extension

## Decision Table

| Area | Decision | Status | Rationale | Validation Entry Point |
| --- | --- | --- | --- | --- |
| App | Chrome Extension MV3 + local Node.js bridge | Proposed | content script で Cloud Samurai DOM を抽出し、Codex app-server とは stdio 接続可能なローカル bridge で連携する | `./scripts/lint.sh`, `./scripts/test.sh` |
| AI Runtime | Codex app-server | Proposed | ユーザー要件。ブラウザから stdio 直結できないため bridge 経由を前提にする | `./scripts/test.sh` |
| Infra | なし | Proposed | 初期 MVP はローカル実行のみで、AWS や公開バックエンドを作らない | `./scripts/lint.sh` |
| CI/CD | TBD | Proposed | 初期実装後に npm scripts と合わせて決める | `./scripts/test.sh` |
| Deploy | Chrome の unpacked extension とローカル bridge 起動 | Proposed | まず手元で動く検証を優先し、公開配布や本番デプロイは対象外にする | `./scripts/deploy.sh` |

`Status` は `Proposed`, `Trial`, `Adopted`, `Rejected` を使う。

## Validation Mapping

### App

- lint: 初期実装前はテンプレート入口確認のみ。実装後は Chrome 拡張と bridge の静的検査に置き換える
- test: 初期実装前はテンプレート入口確認のみ。実装後は sample HTML からの抽出テストと bridge の正常系テストを追加する
- build: 実装後に Chrome MV3 の配布物を生成できることを確認する
- manual: UI 変更時は Chrome に unpacked extension を読み込み、Cloud Samurai または `sample/after-answer/after-answer.html` 相当の画面でボタン表示、抽出、解説表示を確認する

### Infra

- lint: MVP では対象外。AWS/IaC を導入するまではテンプレート入口確認のみ
- validate: MVP では対象外
- plan: MVP では対象外

## Open Questions

技術未確定の論点は、ここに書きっぱなしにせず `docs/tracking/common/QUESTIONS.md` に ID 付きで記録する。

## Revision History

| Date | Version | Summary | Related IDs |
| --- | --- | --- | --- |
| 2026-04-11 | 0.1 | 技術選定と検証入口の管理テンプレートを追加 | - |
| 2026-05-08 | 0.2 | Chrome 拡張、ローカル bridge、Codex app-server の MVP 技術候補を追加 | Q-COM-001, Q-APP-001, Q-INF-001 |
