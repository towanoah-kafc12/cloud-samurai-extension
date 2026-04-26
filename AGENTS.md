# AGENTS.md

## Purpose

このリポジトリは、AWS インフラ開発とアプリケーション開発を AI と人間が協調して進めるためのテンプレートだ。
変更では、安全性、再現性、レビューしやすさを優先すること。
特に、PJ 初期の方向性決定、実装中の課題管理、設計変更の追跡が分断されないことを重視する。

## Read Order

作業開始時は次の順で確認すること。

1. `README.MD`
2. `docs/DELIVERY.md`
3. `docs/TASKS.md`
4. `docs/BOOTSTRAP.md`
5. `docs/TECH-STACK.md`
6. `docs/SKILLS.md`
7. `skills/INDEX.md`
8. `docs/PROJECT.md`
9. `docs/ARCHITECTURE.md`

作業選択は `docs/TASKS.md` の work package を起点にする。
変更対象に応じて `docs/tracking/` と関連する `skills/` も確認すること。

## Working Rules

- 変更前に対象範囲と方針を短く整理する
- `docs/TASKS.md` に `In Progress` の work package があれば、それを最優先で閉じる
- `In Progress` がなければ、`Ready` かつ優先度が最も高い work package を 1 つ選ぶ
- 選んだ work package の `Inputs`、`Dependencies`、`Blockers`、`Done condition` を確認してから作業する
- 未確定事項が残るなら、進めてよい仮説か停止すべきブロッカーかを分ける
- 停止すべき未確定事項は、推測で埋めず `docs/tracking/*/QUESTIONS.md` に記録する
- 作業完了時は、該当 work package の成果物、検証結果、次に解放される work package を確認する
- 不要な大規模リファクタや無関係な整形はしない
- 既存の命名規則と責務分離を尊重する
- 依存関係の追加は最小限にする
- 破壊的変更は明示する
- 指示がない限り、構造全体を作り直さない
- 不具合を直したら、必要に応じて設計書と課題管理を同時に更新する
- 同じ種類の作業説明や手順が繰り返されるなら、Skill 化候補として扱う
- Skill を新規作成または更新するときは、repo 標準搭載の `skills/skill-creator/SKILL.md` を使う前提で進める
- Skill を新規作成、更新、廃止するときは `AGENTS.md` の Skills 節も同じ変更で更新する

## Git Workflow

- work package が意味のある単位で完了したら commit 候補として扱う
- commit 前に `git status` で対象を確認し、一時ファイル、生成物、秘密情報を含めない
- commit 粒度は原則として 1 work package、または強く関連する docs 更新を含む 1 まとまりにする
- push、branch 作成、PR 作成は、ユーザー明示要求または共有・レビューの節目で行う
- deploy、force push、履歴改変は明示要求なしに行わない

## AGENTS.md Maintenance

- PJ 固有の判断が 2 回以上繰り返されたら、常設ルールに昇格するか検討する
- 新しい docs、tracking、skills が作業開始時の必読になったら Read Order を更新する
- 初期テンプレートの一般論が現行 PJ の実態とずれたら、削るか PJ 固有ルールへ置き換える
- 一時的な task、未確定事項、実装メモは `AGENTS.md` に置かず、`docs/TASKS.md` または `docs/tracking/` に置く
- `AGENTS.md` は短く保ち、詳細手順は `docs/*` または `skills/*` に逃がす

## Validation Rules

- コード変更時は `./scripts/lint.sh` と `./scripts/test.sh` を基準に関連確認を行う
- インフラ変更時は差分確認と、定義された lint / validate / plan を行う
- デプロイは別フェーズとして扱い、明示要求があるときだけ `./scripts/deploy.sh` を使う
- 失敗した検証は原因と再現条件を記録する
- 技術未確定で実行できない確認は、想定コマンドを `docs/TECH-STACK.md` に残す

## Documentation Rules

- PJ 開始時の要件整理は `docs/BOOTSTRAP.md` に残す
- 技術選定と検証入口は `docs/TECH-STACK.md` を更新する
- AI の完了条件が変わったら `docs/DELIVERY.md` を更新する
- Skill の追加、更新、廃止があったら `docs/SKILLS.md` と `skills/INDEX.md` を更新する
- 仕様変更時は `docs/PROJECT.md` を更新する
- 構成変更時は `docs/ARCHITECTURE.md` を更新する
- タスク状況が変わったら `docs/TASKS.md` を更新する
- 設計書を更新したら、各ドキュメントの改訂履歴に日付、要約、関連 ID を残す
- 不具合は `BUGS.md`、不明点は `QUESTIONS.md` に分けて管理する

## Skills

作業に入る前に `skills/INDEX.md` で対応 skill を確認すること。

以下の作業では対応する skill を参照すること。

### Standard System Skills

### Standard Bundled Skills

- Skill 作成 / 更新: `skills/skill-creator/SKILL.md`

### Project Skills

- PJ 初期ビルド: `skills/project-bootstrap/SKILL.md`
- AWS インフラ変更: `skills/aws-infra-change/SKILL.md`
- アプリ機能追加: `skills/app-feature/SKILL.md`
- 障害調査: `skills/incident-investigation/SKILL.md`

## Guardrails

- 秘密情報を新規に埋め込まない
- 本番影響がある変更は慎重に扱う
- 説明できる変更だけを行う
- 常設ルールと一時的なタスクを同じ場所に混ぜない
- 課題管理の ID と設計改訂の関連を切らない
