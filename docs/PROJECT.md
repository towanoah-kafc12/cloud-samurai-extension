# PROJECT

## Goal

Cloud Samurai の解答後ページで、問題とユーザーの解答状況に合わせた学習用解説を表示する Chrome 拡張を作る。

## Non-Goals

- 初期 MVP では解説品質の細かなプロンプト調整はしない
- 初期 MVP ではクラウド公開 API や本番デプロイを作らない
- Cloud Samurai 側のデータや画面を改変しない
- 複数学習サイト対応や履歴保存は後続検討に回す

## Problem Statement

Cloud Samurai の標準解説は有用だが、ユーザーがどこで迷ったかに合わせた補足までは自動で出ない。
解答後ページには問題文、選択肢、正解、既存解説が表示されるため、Chrome 拡張から学習コンテキストを抽出できる。
Codex app-server をローカルに置き、まずは「この問題を学習用に解説して」程度の最小指示で、回答後の復習を速くする。

## Users

- AWS 認定試験の勉強で Cloud Samurai を使う学習者
- 解答直後に、自分の理解度に合わせた追加解説を読みたいユーザー

## Scope

今回やることと、今回やらないことを分けて書く。

### In Scope

- Cloud Samurai の解答後ページで動く Chrome 拡張の MVP
- ページ内ボタンまたは拡張 UI からの解説生成トリガー
- DOM からの問題文、選択肢、正解、既存解説、ユーザー解答状態の抽出
- ローカル bridge 経由で Codex app-server に解説生成を依頼する構成
- 生成された解説をページ上に表示する最小 UI

### Out of Scope

- クラウド上の公開バックエンド
- AWS インフラ構築
- ユーザー認証、課金、共有機能
- 学習履歴、分析ダッシュボード、長期メモリ
- Codex app-server WebSocket 直結の本採用

## Success Criteria

- [x] 必須機能が定義されている
- [x] AWS / アプリの責務分離が明確になっている
- [x] 最低限の検証方針が決まっている
- [x] 運用や保守に必要な情報が残っている

## Milestones

- M1: 初期方向性と技術選定が完了している
- M2: サンプル HTML から問題情報を抽出できる
- M3: Chrome 拡張からローカル bridge にリクエストできる
- M4: Codex app-server からの解説をページ上に表示できる

## Constraints

- 予算: 初期 MVP ではローカル実行を前提にクラウド費用を発生させない
- 納期: TBD
- 既存システム制約: Cloud Samurai の DOM 構造変更に弱い可能性がある
- 利用可能なサービスやランタイム: Chrome 拡張、ローカル Node.js bridge、Codex app-server を候補にする

## Risks

- Cloud Samurai の DOM 構造変更で抽出が壊れる
- 解答ページ内のコピー保護や表示制御により抽出できる情報が制限される
- Codex app-server はブラウザから stdio 接続できないためローカル bridge が必要
- ローカル bridge の公開範囲や CORS 設定を誤るとローカルサービスの安全性が下がる

## Related Tracking

- 未確定事項: `docs/tracking/common/QUESTIONS.md`
- アプリ未確定事項: `docs/tracking/app/QUESTIONS.md`
- 共通不具合: `docs/tracking/common/BUGS.md`

## Revision History

| Date | Version | Summary | Related IDs |
| --- | --- | --- | --- |
| 2026-04-11 | 0.1 | 初期テンプレート作成 | - |
| 2026-04-11 | 0.2 | 初期ビルド運用と課題管理連携の前提を追加 | - |
| 2026-05-08 | 0.3 | Cloud Samurai 解説生成拡張の目的と MVP 範囲を定義 | Q-COM-001, Q-APP-001, Q-INF-001 |
