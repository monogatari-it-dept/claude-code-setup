# プロジェクト設定

## ビルド・実行コマンド

- パッケージマネージャ: `npm`（yarnやpnpmは使用しない）
- 開発サーバー: `npm run dev`
- ビルド: `npm run build`
- リント: `npm run lint`
- フォーマット: `npm run format`
- テスト全体: `npm test`
- 単一テスト: `npm test -- --grep "テスト名"`
- 型チェック: `npx tsc --noEmit`

## コードスタイル

- ES Modules（import/export）を使用。CommonJS（require）は禁止
- import時はdestructureを推奨（例: `import { foo } from 'bar'`）
- インデントは2スペース
- セミコロンは使用する
- 文字列はシングルクォート
- 変数・関数名はcamelCase、クラス名はPascalCase、定数はUPPER_SNAKE_CASE
- 型定義はinterfaceを優先（typeは合併型や交差型に限定）
- any型は禁止。unknown + 型ガードを使用する

## ワークフロー

- IMPORTANT: コード変更後は必ず型チェック（`npx tsc --noEmit`）を実行すること
- テスト実行時はパフォーマンスのため単一テストを推奨。全体テストはコミット前のみ
- コミットメッセージは `<type>(<scope>): <description>` 形式
- ブランチ名は `feature/xxx`、`fix/xxx`、`refactor/xxx` 形式

## アーキテクチャ

- フロントエンド: src/client/ 配下
- バックエンド: src/server/ 配下
- 共有型定義: src/shared/types/ 配下
- APIエンドポイント: src/server/api/ 配下（RESTful設計）
- テスト: 各ソースファイルと同階層に `.test.ts` を配置

## 注意事項

- .env ファイルは絶対にコミットしない
- package-lock.json は変更しない（`npm install` 以外で）
- データベースマイグレーションファイルは手動で確認する
- 外部API呼び出しには必ずエラーハンドリングとリトライを実装する

## チーム開発ルール

- 開発ガイドライン: @docs/TEAM_DEVELOPMENT_GUIDE.md
