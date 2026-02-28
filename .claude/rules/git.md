# Git 運用ルール

- main ブランチに直接コミットする
- コミットメッセージ: prefix は英語、本文は日本語
  - prefix: `feat:`, `fix:`, `docs:`, `refactor:`, `chore:`, `test:`
  - 例: `feat: リンクカードコンポーネントを追加`
- `package-lock.json` は必ずコミットに含める

## 自律実装時（GitHub Actions）のルール

GitHub Actions 経由で自律実装する場合は、以下のルールが上記に優先する:

- main ブランチに直接コミットせず、`issue-{number}` ブランチを作成する
- 実装完了後は PR を作成する（`Closes #{number}` を含める）
- PR の本文に「確認事項」セクションを設け、判断に迷った点を記載する
