# DELIVERY

## Purpose

このドキュメントは、AI がどこまで自律的に進めてよいか、どの時点で完了扱いにするかを定義する。
デプロイは実装と検証の後段に分け、明示されない限り自動では進めない。

## Default Rule

AI は、対象範囲が明確で、必要な技術と検証方法が定義されている限り、完了条件を満たすところまで自律的に進める。
不明点が完了条件や安全性に影響するなら、実装を止めて `QUESTIONS.md` に記録する。

## Done Definition by Work Type

| Work Type | Done Definition | Out of Scope by Default |
| --- | --- | --- |
| App Change | 実装、関連 docs 更新、lint/test 実行、結果記録まで | 本番デプロイ |
| Infra Change | IaC 修正、関連 docs 更新、lint/validate/plan まで | 本番反映 |
| Incident Investigation | 再現条件、仮説、切り分け結果、暫定 / 恒久対応案の記録まで | 恒久対応の自動実装 |
| Project Build | Goal、Scope、Tech 候補、未確定事項、停止条件の定義まで | 詳細実装 |

## Done Definition by Work Unit

| Work Unit | Done Definition |
| --- | --- |
| Config / Env | 必要な設定項目、取得元、必須 / 任意、ローカル検証方法が定義され、秘密情報が repo に入っていない |
| App Entry / API / Webhook | 入力、出力、エラー時の扱い、検証方法が定義され、最小の正常系が確認できる |
| Adapter / External Service | 接続先、認証方式、失敗時の扱い、モックまたは実接続の検証方針が記録されている |
| Tests | 対象シナリオ、実行コマンド、期待結果があり、失敗時は原因または未解決 BUG が残っている |
| Docs Update | 変更理由、変更後の使い方、関連する tracking ID または work package ID が追える |
| Tracking Update | `BUGS.md` と `QUESTIONS.md` が分離され、各項目に Status と次アクションがある |
| Git Checkpoint | 保存対象を `git status` で確認し、work package の区切りと commit 粒度が対応している |

## Assumption and Block Rule

- 未確定事項は、作業を止めるものと仮説で進められるものに分ける
- 完了条件、安全性、公開範囲、外部課金、本番影響に関わる未確定事項は止める
- 命名、内部構成、後で置き換え可能な実装詳細は、`Assumption` として記録すれば進めてよい
- 仮説で進めた場合は、該当 work package または `QUESTIONS.md` に見直し条件を残す

## Git Checkpoint Rules

- 意味のある work package が完了したら commit 候補にする
- 1 commit は原則として 1 work package、または強く関連する docs 更新を含む 1 まとまりにする
- 一時ログ、生成物、秘密情報、検証用の不要ファイルは commit しない
- push は、リモート共有、レビュー、バックアップ、ユーザー明示要求のいずれかの節目で行う
- branch / PR は、main に直接積むよりレビュー単位を分ける価値がある作業量になった時点で検討する
- deploy、force push、履歴改変は明示要求があるときだけ行う

## Stop Conditions

次のどれかに当てはまるときは、いったん停止して確認する。

- 本番影響のある操作が必要
- 未確定事項が安全性や完了条件に直接影響する
- 技術選定が未確定で検証方法を定義できない
- 依存追加や構成変更の影響範囲が大きい

## Tracking Rules

- 実装中の不具合は `BUGS.md` に記録する
- 仕様や方針の不明点は `QUESTIONS.md` に記録する
- 設計変更が入ったら、関連 ID を `PROJECT.md` または `ARCHITECTURE.md` の改訂履歴へ残す

## Revision History

| Date | Version | Summary | Related IDs |
| --- | --- | --- | --- |
| 2026-04-11 | 0.1 | AI の完了条件と停止条件のテンプレートを追加 | - |
| 2026-04-26 | 0.2 | work package 単位の完了条件、未確定事項、Git checkpoint ルールを追加 | - |
