# 質問・待機機能

## 概要

- 対応PRD要件: F-6（質問・待機）
- 作成日: 2026-02-28
- 状態: 完了

## 背景・目的

Issue の内容だけでは実装方針が決まらない場合、Claude Code が Issue にコメントで質問し、`claude:waiting` ラベルを付与して待機状態を明示する。これにより人間は Claude が何を待っているかが一目でわかる。

## 機能仕様

### Feature: 不明点がある場合の質問・待機

```
背景:
  Claude Code が Issue の内容から実装方針を決定しようとするが、不明点がある

Scenario: 不明点がある場合に質問する
  Given claude:implement ラベルが付与されて Claude Code が起動している
  And Issue の内容が不明確で実装方針が決まらない
  When Claude Code が不明点を検出する
  Then Issue にコメントで質問が投稿される
  And `claude:waiting` ラベルが Issue に付与される

Scenario: 質問後に実装が再開される
  Given claude:waiting ラベルが付与されて Claude Code が待機している
  And 開発者が Issue コメントで質問に回答した
  When 開発者が `claude:implement` ラベルを再付与する
  Then Claude Code が回答を読み込んで実装を再開する
```

## 技術的考慮事項

- 質問のコメント後、`gh issue edit {number} --add-label "claude:waiting"` でラベルを付与する
- `claude:waiting` ラベルはリポジトリに事前作成が必要

## 受入条件

- [x] 不明点がある場合に Issue コメントで質問できる
- [x] `claude:waiting` ラベルが付与できる
- [x] `claude:waiting` ラベルがリポジトリに存在する

## 実装ステップ

1. [x] `claude:waiting` ラベルの作成（リポジトリ設定）
2. [x] 質問・ラベル付与ロジックのルール化（`.claude/CLAUDE.md`）
3. [x] レビュー（ビルド確認 + Lint + `/code-review`）
