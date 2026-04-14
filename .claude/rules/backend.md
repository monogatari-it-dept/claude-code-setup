---
paths:
  - "src/server/**/*.ts"
---

# バックエンド開発ルール

- APIエンドポイントはRESTful設計に従う
- URLパスはkebab-case、JSONプロパティはcamelCase
- リストエンドポイントには必ずページネーションを実装する
- すべてのエンドポイントに入力バリデーションを実装する（zodを使用）
- エラーレスポンスは統一フォーマットで返す: `{ error: { code: string, message: string } }`
- データベースアクセスは src/server/repositories/ に集約する
- ビジネスロジックは src/server/services/ に配置する
- ミドルウェアは src/server/middleware/ に配置する
- 外部API呼び出しにはリトライとタイムアウトを必ず設定する
- 環境変数は src/server/config.ts で一元管理する（process.envを直接参照しない）
- ログ出力はロガーライブラリ経由で行う（console.logは禁止）
