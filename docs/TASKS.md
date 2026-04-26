# TASKS

## Purpose

このファイルは、AI が次に着手する作業を決めるための実行ボードだ。
人間向けの TODO 一覧ではなく、実行順、入力、成果物、検証、完了条件を work package 単位で管理する。

## Execution Loop

各作業は次の順で進める。

1. `Ready` の work package から優先順位が最も高いものを 1 つ選ぶ
2. `Inputs` を読む
3. `Dependencies` と `Blockers` を確認する
4. `Files to touch` の範囲で実装または文書更新を行う
5. `Validation` を実行または実行不能理由を記録する
6. `Outputs` と `Done condition` を満たしたら `Status` を `Done` にする
7. 次に解放される work package を明示する

## Next Action Rule

- `Status: In Progress` があれば、それを最優先で閉じる
- `Blocked` は、ブロッカーが解消するまで着手しない
- 同じ優先度なら `common`、`app`、`infra` の順で、後続作業を多く解放するものを優先する
- 未確定事項があっても `Assumption` として明記でき、完了条件や安全性に影響しない範囲は進めてよい
- 完了条件、安全性、外部公開、本番影響に関わる未確定事項は `docs/tracking/*/QUESTIONS.md` に記録して止める

## Work Package Schema

各 work package は次の項目を持つ。

| Field | Meaning |
| --- | --- |
| ID | `WP-COM-001`、`WP-APP-001`、`WP-INF-001` の形式 |
| Phase | `bootstrap`、`design`、`implementation`、`validation`、`delivery` |
| Status | `Ready`、`In Progress`、`Blocked`、`Done` |
| Priority | `P0`、`P1`、`P2` |
| Objective | この作業で達成すること |
| Inputs | 着手前に読むファイルや外部入力 |
| Files to touch | 原則として変更してよいファイル |
| Outputs | 完了時に残す成果物 |
| Validation | 実行する検証、または検証不能時の記録先 |
| Dependencies | 先に完了している必要がある work package |
| Blockers | 未解決なら止まる条件 |
| Done condition | 完了判定 |
| Git checkpoint | commit / push の判断 |

## Current Work Packages

### WP-COM-001: Project direction baseline

| Field | Value |
| --- | --- |
| Phase | bootstrap |
| Status | Ready |
| Priority | P0 |
| Objective | PJ の目的、スコープ、成功条件を実装前に最低限決める |
| Inputs | `README.MD`, `docs/BOOTSTRAP.md`, ユーザー要件 |
| Files to touch | `docs/PROJECT.md`, `docs/tracking/common/QUESTIONS.md` |
| Outputs | Goal, Non-Goals, Users, Scope, Success Criteria の初期版 |
| Validation | `docs/BOOTSTRAP.md` の Bootstrap Checklist と照合する |
| Dependencies | なし |
| Blockers | Goal / In Scope / Out of Scope が一文でも書けない |
| Done condition | 実装対象と対象外が分かり、未確定事項が `QUESTIONS.md` に逃がされている |
| Git checkpoint | Done になったら docs-only commit 候補 |

### WP-COM-002: Technical baseline

| Field | Value |
| --- | --- |
| Phase | bootstrap |
| Status | Ready |
| Priority | P0 |
| Objective | app / infra の仮技術と検証入口を決める |
| Inputs | `docs/PROJECT.md`, `docs/BOOTSTRAP.md`, ユーザー要件 |
| Files to touch | `docs/TECH-STACK.md`, `scripts/lint.sh`, `scripts/test.sh`, `docs/tracking/common/QUESTIONS.md` |
| Outputs | 技術候補、採用理由、未確定事項、仮の lint/test コマンド |
| Validation | `./scripts/lint.sh` と `./scripts/test.sh` が実行入口として存在することを確認する |
| Dependencies | WP-COM-001 |
| Blockers | 技術候補を 1 つも置けない |
| Done condition | 技術が未確定でも、次に検証すべき候補とコマンド入口が分かる |
| Git checkpoint | Done になったら commit 候補 |

### WP-APP-001: App responsibility design

| Field | Value |
| --- | --- |
| Phase | design |
| Status | Ready |
| Priority | P1 |
| Objective | アプリケーションの種類、責務、外部入出力を定義する |
| Inputs | `docs/PROJECT.md`, `docs/TECH-STACK.md`, `docs/ARCHITECTURE.md` |
| Files to touch | `docs/ARCHITECTURE.md`, `docs/tracking/app/QUESTIONS.md` |
| Outputs | app の責務、主要コンポーネント、外部サービス、未確定事項 |
| Validation | `docs/PROJECT.md` の In Scope と矛盾しないことを確認する |
| Dependencies | WP-COM-001, WP-COM-002 |
| Blockers | アプリの種類または主要責務が決まらない |
| Done condition | app 配下で最初に作るファイルまたはモジュールの候補が分かる |
| Git checkpoint | Done になったら commit 候補 |

### WP-INF-001: Infra responsibility design

| Field | Value |
| --- | --- |
| Phase | design |
| Status | Ready |
| Priority | P1 |
| Objective | AWS / IaC の責務と環境方針を定義する |
| Inputs | `docs/PROJECT.md`, `docs/TECH-STACK.md`, `docs/ARCHITECTURE.md` |
| Files to touch | `docs/ARCHITECTURE.md`, `docs/tracking/infra/QUESTIONS.md` |
| Outputs | infra の責務、主要 AWS リソース候補、環境差分、未確定事項 |
| Validation | `docs/PROJECT.md` の Constraints と矛盾しないことを確認する |
| Dependencies | WP-COM-001, WP-COM-002 |
| Blockers | AWS を使う範囲または IaC 方針が決まらない |
| Done condition | infra 配下で最初に作るファイルまたはスタックの候補が分かる |
| Git checkpoint | Done になったら commit 候補 |

### WP-APP-002: App implementation slice

| Field | Value |
| --- | --- |
| Phase | implementation |
| Status | Blocked |
| Priority | P1 |
| Objective | 最小の app 実装単位を追加する |
| Inputs | `docs/PROJECT.md`, `docs/ARCHITECTURE.md`, `docs/TECH-STACK.md`, WP-APP-001 の成果 |
| Files to touch | `app/`, `docs/ARCHITECTURE.md`, `docs/tracking/app/BUGS.md`, `docs/tracking/app/QUESTIONS.md` |
| Outputs | 最小実装、関連 docs 更新、必要な tracking 更新 |
| Validation | `./scripts/lint.sh` と `./scripts/test.sh`、または `docs/TECH-STACK.md` に定義された app 検証 |
| Dependencies | WP-APP-001 |
| Blockers | app の責務、技術、検証方法が未定義 |
| Done condition | 変更対象の実装が動き、関連検証と docs 更新が完了している |
| Git checkpoint | Done になったら 1 work package = 1 commit を基本にする |

### WP-INF-002: Infra implementation slice

| Field | Value |
| --- | --- |
| Phase | implementation |
| Status | Blocked |
| Priority | P1 |
| Objective | 最小の infra 実装単位を追加する |
| Inputs | `docs/PROJECT.md`, `docs/ARCHITECTURE.md`, `docs/TECH-STACK.md`, WP-INF-001 の成果 |
| Files to touch | `infra/`, `docs/ARCHITECTURE.md`, `docs/tracking/infra/BUGS.md`, `docs/tracking/infra/QUESTIONS.md` |
| Outputs | 最小 IaC 実装、関連 docs 更新、必要な tracking 更新 |
| Validation | `./scripts/lint.sh` と `./scripts/test.sh`、または `docs/TECH-STACK.md` に定義された infra 検証 |
| Dependencies | WP-INF-001 |
| Blockers | infra の責務、技術、検証方法が未定義 |
| Done condition | IaC 差分の検証入口が動き、関連 docs 更新が完了している |
| Git checkpoint | Done になったら 1 work package = 1 commit を基本にする |

### WP-COM-003: Delivery and tracking review

| Field | Value |
| --- | --- |
| Phase | delivery |
| Status | Ready |
| Priority | P2 |
| Objective | 完了条件、停止条件、tracking の運用が現状に合っているか確認する |
| Inputs | `docs/DELIVERY.md`, `docs/tracking/README.md`, 各 `BUGS.md`, 各 `QUESTIONS.md` |
| Files to touch | `docs/DELIVERY.md`, `docs/tracking/README.md`, `AGENTS.md` |
| Outputs | 現行 PJ に合った停止条件、tracking ルール、必要なら AGENTS への常設ルール昇格 |
| Validation | 未解決の `QUESTIONS.md` が、進行可能か停止対象か分類されていることを確認する |
| Dependencies | WP-COM-001 |
| Blockers | なし |
| Done condition | AI が次に止まるべき条件と進めてよい範囲を判断できる |
| Git checkpoint | Done になったら docs-only commit 候補 |

## Done

- テンプレート構成の基本方針を決定した
- 課題管理と設計改訂の連携ルールを追加した
