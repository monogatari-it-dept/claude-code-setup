---
name: fix-issue
description: GitHub Issueを分析して修正するワークフロー。Issueの内容を読み、原因を特定し、テスト付きで修正してPRを作成する。
disable-model-invocation: true
allowed-tools: Bash(gh *) Bash(git *) Bash(npm test *) Bash(npx tsc *)
---

GitHub Issue #$ARGUMENTS を修正する。

## 手順

1. `gh issue view $ARGUMENTS` でIssueの詳細を取得する
2. Issue内容を分析し、関連するコードを特定する
3. 修正用ブランチを作成: `git checkout -b fix/$ARGUMENTS-<簡潔な説明>`
4. 問題を再現するテストを作成する（テストが失敗することを確認）
5. 修正を実装する
6. テストが通ることを確認: `npm test`
7. 型チェック: `npx tsc --noEmit`
8. リント: `npm run lint`
9. 説明的なコミットメッセージで変更をコミット: `fix(#$ARGUMENTS): <説明>`
10. PRを作成: `gh pr create --title "fix: #$ARGUMENTS <説明>" --body "Closes #$ARGUMENTS"`
