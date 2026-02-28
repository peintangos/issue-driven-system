# issue-driven-system

GitHub Issue をタスクキューとし、ラベル付与をトリガーに Claude Code が自律的に実装→PR作成を行うシステム。

## 概要

人間が Issue を起票してラベルを付与するだけで、Claude Code が自律的にブランチ作成・実装・テスト・PR作成まで行う。人間の指示がボトルネックにならず、非同期でレビュー・修正サイクルを回せる。

```
[人間] ── Issue起票 + ラベル付与 ──> [GitHub Actions]
                                          │
                                          v
                              [claude-code-action@v1]
                              (.claude/ のスキル・エージェント・ルールを読み込み)
                                  │              │
                                  v              v
                            [ブランチ作成]    [Issue コメント]
                            [実装 + テスト]   [質問・報告]
                            [PR 作成]
                                  │
                                  v
                          [人間がPRレビュー]
                          [@claude コメント で修正指示]
                                  │
                                  v
                            [人間がマージ]
```

## 使い方

### 自律実装をトリガーする

1. GitHub Issue を起票する
2. Issue に `claude:implement` ラベルを付与する
3. GitHub Actions が起動し、Claude Code が自律的に実装→PR作成を行う

### PRレビュー中に修正指示を出す

PR のコメントで `@claude` をメンションして修正指示を記載する。Claude Code が自動的に対応する。

## セットアップ

### 必要なシークレット

リポジトリの Settings > Secrets and variables > Actions に以下を設定する:

| シークレット名 | 説明 |
|---|---|
| `ANTHROPIC_API_KEY` | Anthropic API キー |

### ラベルの作成

以下のラベルをリポジトリに作成する:

| ラベル名 | 用途 |
|---|---|
| `claude:implement` | 自律実装のトリガー |
| `claude:waiting` | Claude が回答待ちの状態 |

## ワークフロー

| ファイル | トリガー | 動作 |
|---|---|---|
| `.github/workflows/claude-implement.yml` | Issue に `claude:implement` ラベルが付与された時 | ブランチ作成・実装・テスト・PR作成 |
| `.github/workflows/claude-review.yml` | PR コメントで `@claude` がメンションされた時 | コメント内容に基づく修正対応 |

## サーバー起動

```bash
npm start
```

`http://localhost:3000` でサーバーが起動する。

## Claude Code でのローカル開発

このプロジェクトの devcontainer は [Claude Code](https://docs.anthropic.com/en/docs/claude-code) での開発用に設計されている。

VS Code でこのプロジェクトを開き、「Reopen in Container」を選択した後:

```bash
# ターミナルで Claude Code を起動
claude
```

初回起動時はブラウザ経由で認証が求められる（VS Code が自動的に URL を転送する）。

## 開発環境

- Node.js 22（devcontainer）
- パッケージマネージャ: npm
