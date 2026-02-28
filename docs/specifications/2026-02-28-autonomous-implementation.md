# 自律実装機能

## 概要

- 対応PRD要件: F-2（ブランチ管理）、F-3（自律実装）、F-4（PR 作成）
- 作成日: 2026-02-28
- 状態: 完了

## 背景・目的

Claude Code が Issue の内容を理解し、ブランチ作成・実装・テスト・PR 作成までを自律的に行う。人間はレビューと承認のみに集中できる。

## 機能仕様

### Feature: Issue に基づく自律実装

```
背景:
  GitHub Actions により Claude Code が起動し、Issue の内容に基づいて実装を行う

Scenario: Issue の内容を読んで実装する
  Given claude:implement ラベルが付与されて Claude Code が起動している
  And Issue にタイトルと実装内容が記載されている
  When Claude Code が Issue の内容を解析する
  Then `issue-{issue_number}` ブランチが作成される
  And 実装とテストが行われる
  And PR が作成される
  And PR に `Closes #{issue_number}` が含まれる

Scenario: 実装を完了してPRを作成する
  Given Claude Code が実装・テストを完了した
  When PR を作成する
  Then PR 本文に Summary・変更点・テスト・確認事項が含まれる
  And Issue が PR で参照される

Scenario: main ブランチへの直接コミットを行わない
  Given Claude Code が実装を行っている
  When 変更をコミットする
  Then `issue-{issue_number}` ブランチにコミットされる
  And main ブランチへの直接コミットは行われない
```

## 技術的考慮事項

- ブランチ名は `issue-{number}` 形式（例: `issue-3`）
- コミットメッセージは Conventional Commits 形式（prefix は英語、本文は日本語）
- PR 本文の構成:
  - Summary（箇条書き）
  - 変更点
  - テスト
  - 確認事項（判断に迷った点）
  - `Closes #{number}`
- `.claude/CLAUDE.md` の実装フロー（テスト → ビルドチェック → コードレビュー）に従う

## 受入条件

- [x] `issue-{number}` ブランチが自動作成される
- [x] 実装にテストが含まれる
- [x] PR が自動作成される
- [x] PR に `Closes #{number}` が含まれる
- [x] main ブランチへの直接コミットが行われない

## 実装ステップ

1. [x] ブランチ作成ロジックの確認（GitHub Actions 設定）
2. [x] 実装ガイドライン（`.claude/CLAUDE.md`）の整備
3. [x] PR テンプレート・ルールの定義
4. [x] レビュー（ビルド確認 + Lint + `/code-review`）
