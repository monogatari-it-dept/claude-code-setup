---
name: commit
description: 現在の変更をステージングしてコミットする。差分を分析して適切なコミットメッセージを生成する。
disable-model-invocation: true
allowed-tools: Bash(git *)
---

現在の変更をコミットする。

## 手順

1. `git status` で変更ファイルを確認
2. `git diff` でステージされていない変更を確認
3. `git diff --cached` でステージ済みの変更を確認
4. 変更内容を分析して、以下の形式でコミットメッセージを生成:

```
<type>(<scope>): <日本語で簡潔な説明>

<変更の詳細（必要に応じて）>
```

type: feat, fix, refactor, test, docs, chore
scope: 変更対象の領域（例: auth, api, ui）

5. 関連するファイルをステージング（.envや機密ファイルは除外）
6. コミットを実行

$ARGUMENTS が指定されている場合は、その内容をコミットメッセージのヒントとして使用する。
