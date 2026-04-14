---
name: create-api
description: 新しいRESTful APIエンドポイントを作成する。ルーティング、バリデーション、サービス層、テストを一括生成する。
disable-model-invocation: true
---

新しいAPIエンドポイントを作成する: $ARGUMENTS

## 手順

1. 既存のAPIエンドポイントのパターンを確認（src/server/api/ 内の既存ファイルを参照）
2. 以下のファイルを作成:

### ルーター（src/server/api/<resource>.ts）
- RESTfulなURL設計
- zodによる入力バリデーション
- 適切なHTTPステータスコード
- 統一エラーフォーマット

### サービス層（src/server/services/<resource>Service.ts）
- ビジネスロジックを配置
- リポジトリ層を経由したデータアクセス
- エラーハンドリング

### リポジトリ（src/server/repositories/<resource>Repository.ts）
- データベースアクセスの抽象化
- 型安全なクエリ

### テスト
- APIエンドポイントの統合テスト
- サービス層のユニットテスト
- 正常系・異常系・エッジケース

3. 型チェック: `npx tsc --noEmit`
4. テスト実行: `npm test`
5. リント: `npm run lint`
