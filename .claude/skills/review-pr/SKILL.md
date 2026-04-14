---
name: review-pr
description: PRのコードレビューを実施する。差分を確認し、バグ、セキュリティ、パフォーマンス、コード品質の観点でレビューする。
context: fork
agent: Explore
allowed-tools: Bash(gh *)
---

## PRコンテキスト

- PR差分: !`gh pr diff $ARGUMENTS 2>/dev/null || echo "PR番号を引数に指定してください"`
- PR情報: !`gh pr view $ARGUMENTS 2>/dev/null || echo ""`
- 変更ファイル一覧: !`gh pr diff $ARGUMENTS --name-only 2>/dev/null || echo ""`

## レビュー観点

以下の観点でPRをレビューし、問題点を報告する:

1. **バグ・ロジックエラー**: エッジケースの見落とし、off-by-oneエラー、null/undefinedの未チェック
2. **セキュリティ**: SQLインジェクション、XSS、認証・認可の不備、機密情報の露出
3. **パフォーマンス**: N+1クエリ、不要な再レンダリング、メモリリーク
4. **コード品質**: 命名、責務の分離、DRY原則、テストの妥当性
5. **アーキテクチャ**: 既存パターンとの一貫性、依存関係の方向

## 出力形式

各問題を以下の形式で報告する:
- 深刻度: 🔴 Critical / 🟡 Warning / 🔵 Info
- ファイル: ファイルパスと行番号
- 問題: 具体的な問題の説明
- 提案: 修正案
