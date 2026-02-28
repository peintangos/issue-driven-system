# ロードマップ

## ビジョン

GitHub Issue をタスクキューとして、Claude Code が自律的に実装するシステム。人間は要件定義とレビューに集中し、実装は Claude に任せる。

## MVP（2026-02）

Issue ラベルトリガーによる自律実装の基本フロー。

### MVP 機能

| 機能 | Specification | 状態 |
|-----|--------------|------|
| Issue ラベルトリガー | [2026-02-28-issue-label-trigger.md](specifications/2026-02-28-issue-label-trigger.md) | ✅ 完了 |
| 自律実装（ブランチ・実装・PR） | [2026-02-28-autonomous-implementation.md](specifications/2026-02-28-autonomous-implementation.md) | ✅ 完了 |
| PR レビュー対応 | [2026-02-28-pr-review-response.md](specifications/2026-02-28-pr-review-response.md) | ✅ 完了 |
| 質問・待機 | [2026-02-28-question-and-waiting.md](specifications/2026-02-28-question-and-waiting.md) | ✅ 完了 |

## エンハンス候補（2026-03以降）

### M2: 品質向上

- テスト自動化統合（Jest / Vitest）
- ESLint / Prettier 統合
- コードカバレッジ計測
- `/code-review` スキルの強化

### M3: ワークフロー拡充

- 複数 Issue の並列実装対応
- ラベル管理自動化（`claude:done` など）
- 実装進捗ダッシュボード
- Issue テンプレートの整備
- 実装コスト（API 使用量）のモニタリング

## 決定事項

- **GitHub Actions + claude-code-action@v1** を採用（2026-02）
  - 理由: GitHub ネイティブで追加インフラが不要
- **main への直接コミット禁止**（2026-02）
  - 理由: 品質ゲートとしてのレビューを必須とする
