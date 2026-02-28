# Issue ラベルトリガー機能

## 概要

- 対応PRD要件: F-1（Issue ラベルトリガー）、SR-1（GitHub Actions）、SR-2（認証）
- 作成日: 2026-02-28
- 状態: 完了

## 背景・目的

開発者が GitHub Issue に `claude:implement` ラベルを付与するだけで、自動的に Claude Code の実装フローが開始される仕組みを提供する。既存の GitHub 操作をそのまま使えるため、学習コストなしで利用できる。

## 機能仕様

### Feature: Issue ラベルトリガーによる自律実装開始

```
背景:
  開発者が GitHub でプロジェクトを管理しており、Claude Code を使って自律的に実装を行いたい

Scenario: claude:implement ラベルで実装が開始される
  Given GitHub リポジトリに Issue が存在する
  And リポジトリに ANTHROPIC_API_KEY シークレットが設定されている
  And `.github/workflows/claude-implement.yml` が配置されている
  When 開発者が Issue に `claude:implement` ラベルを付与する
  Then GitHub Actions ワークフローが起動する
  And Claude Code が Issue の内容を読み込む
  And `issue-{number}` ブランチが作成される
  And 実装が開始される

Scenario: 必要なシークレットが未設定の場合
  Given GitHub リポジトリに Issue が存在する
  And リポジトリに ANTHROPIC_API_KEY シークレットが設定されていない
  When 開発者が Issue に `claude:implement` ラベルを付与する
  Then GitHub Actions ワークフローが起動する
  And 認証エラーでワークフローが失敗する
```

## 技術的考慮事項

- `claude-code-action@v1` を使用して Claude Code を実行する
- ワークフローに必要な権限:
  - `contents: write` — ブランチ・コミット操作
  - `pull-requests: write` — PR 作成
  - `issues: write` — コメント・ラベル操作
  - `id-token: write` — 認証トークン
- `.claude/` 配下のドキュメントが Claude Code のコンテキストに読み込まれる

## 受入条件

- [x] `claude:implement` ラベル付与で GitHub Actions が起動する
- [x] ANTHROPIC_API_KEY が設定されていれば Claude Code が正常に起動する
- [x] 必要なパーミッションが `claude-implement.yml` に設定されている

## 実装ステップ

1. [x] `.github/workflows/claude-implement.yml` の作成
2. [x] `claude-code-action@v1` の設定（権限・シークレット）
3. [x] `.claude/` 配下のスキル・ルール設定
4. [x] レビュー（ビルド確認 + Lint + `/code-review`）
