---
paths:
  - "src/client/**/*.{ts,tsx}"
  - "src/client/**/*.css"
---

# フロントエンド開発ルール

- Reactコンポーネントは関数コンポーネントで作成する（クラスコンポーネントは禁止）
- 状態管理にはReact Hooks（useState, useReducer, useContext）を使用する
- コンポーネントファイル名はPascalCase（例: UserProfile.tsx）
- CSSモジュールまたはTailwind CSSを使用する。インラインスタイルは避ける
- ページコンポーネントは src/client/pages/ に配置
- 共通コンポーネントは src/client/components/ に配置
- カスタムHooksは src/client/hooks/ に配置
- APIコールは src/client/api/ に集約する（コンポーネント内で直接fetchしない）
- すべてのユーザー入力にバリデーションを実装する
- エラーバウンダリを適切に配置する
