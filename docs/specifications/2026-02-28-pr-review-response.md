# PR レビュー対応機能

## 概要

- 対応PRD要件: F-5（PR レビュー対応）
- 作成日: 2026-02-28
- 状態: 完了

## 背景・目的

人間が PR レビュー中に修正を求める場合、PR コメントで `@claude` をメンションするだけで Claude Code が自動的に対応する。人間は修正内容を自然言語で伝えるだけでよい。

## 機能仕様

### Feature: PR コメントによる修正指示対応

```
背景:
  開発者が PR をレビューし、Claude Code に修正を依頼したい

Scenario: @claude メンションで修正が行われる
  Given PR が作成されている
  And claude-review.yml ワークフローが設定されている
  When 開発者が PR コメントで `@claude [修正内容]` と記述する
  Then GitHub Actions ワークフローが起動する
  And Claude Code がコメント内容を解析する
  And 修正が実装される
  And 同じブランチにコミットがプッシュされる

Scenario: 修正内容が不明確な場合
  Given PR が作成されている
  When 開発者が `@claude` とだけコメントする（修正内容が不明確）
  Then Claude Code が修正内容を確認するコメントを返す
```

## 技術的考慮事項

- `.github/workflows/claude-review.yml` でトリガーを設定する
- PR コメントの `@claude` メンションを検知する
- 修正は同じ PR ブランチ（`issue-{number}`）にプッシュする

## 受入条件

- [x] PR コメントで `@claude` メンションが検知される
- [x] 修正が同じブランチにプッシュされる
- [x] ワークフロー設定が完了している

## 実装ステップ

1. [x] `.github/workflows/claude-review.yml` の作成
2. [x] `@claude` メンション検知設定
3. [x] レビュー（ビルド確認 + Lint + `/code-review`）
