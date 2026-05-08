# App Questions

## Entries

| ID | Title | Status | Needed By | Decision Driver | Related Docs | Resolution |
| --- | --- | --- | --- | --- | --- | --- |
| Q-APP-001 | アプリの種類が未確定 | Resolved | app 雛形作成前 | ディレクトリとテスト方針を決めるため | `docs/PROJECT.md`, `docs/TECH-STACK.md` | Chrome Extension MV3 と local Node.js bridge を `app/` に置く |
| Q-APP-002 | Cloud Samurai DOM からユーザーの選択状態を安定抽出する方法 | Open | 抽出実装前 | sample では正解選択肢が緑表示だが、誤答時や部分正解時の DOM 差分確認が必要 | `sample/after-answer/after-answer.html`, `docs/ARCHITECTURE.md` | 未決 |
| Q-APP-003 | 解説表示 UI の配置 | Open | content script 実装前 | ページ内ボタン、サイドパネル、popup のどれを MVP にするか決めるため | `docs/ARCHITECTURE.md` | 未決 |
