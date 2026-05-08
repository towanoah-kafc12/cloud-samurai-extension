# Common Questions

## Entries

| ID | Title | Status | Needed By | Decision Driver | Related Docs | Resolution |
| --- | --- | --- | --- | --- | --- | --- |
| Q-COM-001 | 技術スタックが未確定 | Resolved | 実装開始前 | app / infra の検証方法を決めるため | `docs/TECH-STACK.md` | MVP は Chrome Extension MV3 + local Node.js bridge + Codex app-server。AWS/IaC は初期対象外 |
| Q-COM-002 | Codex app-server の起動方法と認証状態 | Open | bridge 実装前 | ローカル bridge から stdio 接続できる前提とユーザー環境の確認が必要 | `docs/TECH-STACK.md`, `docs/ARCHITECTURE.md` | 未決 |
| Q-COM-003 | Chrome 拡張から localhost bridge へ接続する許可範囲 | Open | 拡張 manifest 作成前 | host permissions と CORS を最小化するため | `docs/TECH-STACK.md`, `docs/ARCHITECTURE.md` | 未決 |
