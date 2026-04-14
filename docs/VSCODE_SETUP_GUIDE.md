# VS Code + Claude Code 開発環境セットアップガイド

> チーム標準エディタとしてVisual Studio Codeを推奨し、Claude Code拡張機能を活用した開発環境を構築する

## 1. なぜ VS Code + Claude Code か

VS Codeは軽量で拡張性が高く、チーム全体で統一した開発体験を実現できる。Claude Code拡張機能をVS Codeに統合することで、エディタから離れることなくAIアシスタントとの対話、コードレビュー、自動修正が可能になる。

主なメリット:

- エディタ内のパネルで直接Claudeと対話できる
- ファイルの@メンション（行番号指定も可能）でピンポイントな質問ができる
- 変更のインラインdiff表示で、提案内容を確認してから適用できる
- 複数の会話を並行して開ける（タブ/ウィンドウ分割）
- Plan Modeで実装計画をレビューしてから作業を開始できる
- CLAUDE.md、Skills、Hooks、Rulesがそのまま動作する

## 2. 前提条件

| 項目 | 要件 |
|------|------|
| VS Code | バージョン 1.98.0 以上 |
| OS | macOS 13.0+ / Windows 10 1809+ / Ubuntu 20.04+ |
| Anthropicアカウント | Pro, Max, Team, Enterprise, またはConsoleプラン |
| RAM | 4 GB 以上 |
| ネットワーク | インターネット接続が必要 |

## 3. Claude Code のインストール

### 3.1 CLIのインストール（推奨: ネイティブインストーラー）

macOS / Linux:
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

Windows PowerShell:
```powershell
irm https://claude.ai/install.ps1 | iex
```

Homebrew（macOS）:
```bash
brew install --cask claude-code
```

インストール後の確認:
```bash
claude --version
claude doctor   # 詳細な環境チェック
```

### 3.2 VS Code拡張機能のインストール

VS Code内で `Cmd+Shift+X`（Mac）または `Ctrl+Shift+X`（Windows/Linux）を押して拡張機能ビューを開き、「Claude Code」で検索して **Install** をクリックする。

または、コマンドパレット（`Cmd+Shift+P` / `Ctrl+Shift+P`）から以下を実行:
```
ext install anthropic.claude-code
```

> 注意: インストール後に拡張機能が表示されない場合は、VS Codeを再起動するか、コマンドパレットで「Developer: Reload Window」を実行する。

### 3.3 初回サインイン

1. VS Codeでエディタツールバー右上のSparkアイコン（✱）をクリック
2. サインイン画面が表示されたら **Sign in** をクリック
3. ブラウザで認証を完了

## 4. 推奨VS Code拡張機能

Claude Code以外にも、チーム開発に必要な拡張機能をインストールする。

### 4.1 必須拡張機能

| 拡張機能 | 拡張機能ID | 用途 |
|----------|-----------|------|
| Claude Code | `anthropic.claude-code` | AI開発アシスタント |
| ESLint | `dbaeumer.vscode-eslint` | JavaScriptリント |
| Prettier | `esbenp.prettier-vscode` | コードフォーマット |
| TypeScript Importer | `pmneo.tsimporter` | import文の自動補完 |

### 4.2 推奨拡張機能

| 拡張機能 | 拡張機能ID | 用途 |
|----------|-----------|------|
| GitLens | `eamodio.gitlens` | Git履歴・blame表示 |
| Error Lens | `usernamehw.errorlens` | エラーのインライン表示 |
| TODO Highlight | `wayou.vscode-todo-highlight` | TODO/FIXMEハイライト |
| Tailwind CSS IntelliSense | `bradlc.vscode-tailwindcss` | Tailwindクラス補完 |
| Path Intellisense | `christian-kohler.path-intellisense` | ファイルパス補完 |

### 4.3 一括インストールコマンド

ターミナルで以下を実行すると、すべての推奨拡張機能を一括インストールできる:

```bash
# 必須
code --install-extension anthropic.claude-code
code --install-extension dbaeumer.vscode-eslint
code --install-extension esbenp.prettier-vscode
code --install-extension pmneo.tsimporter

# 推奨
code --install-extension eamodio.gitlens
code --install-extension usernamehw.errorlens
code --install-extension wayou.vscode-todo-highlight
code --install-extension bradlc.vscode-tailwindcss
code --install-extension christian-kohler.path-intellisense
```

## 5. VS Codeプロジェクト設定

### 5.1 ワークスペース設定（.vscode/settings.json）

チームで共有するVS Code設定をプロジェクトに配置する:

```json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "editor.tabSize": 2,
  "editor.insertSpaces": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
  },
  "typescript.preferences.importModuleSpecifier": "relative",
  "typescript.preferences.quoteStyle": "single",
  "files.eol": "\n",
  "files.trimTrailingWhitespace": true,
  "files.insertFinalNewline": true,
  "search.exclude": {
    "**/node_modules": true,
    "**/dist": true,
    "**/.git": true
  }
}
```

### 5.2 推奨拡張機能リスト（.vscode/extensions.json）

プロジェクトを開いた際に推奨拡張機能をインストールするよう促す:

```json
{
  "recommendations": [
    "anthropic.claude-code",
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    "pmneo.tsimporter",
    "eamodio.gitlens",
    "usernamehw.errorlens"
  ]
}
```

## 6. Claude Code 拡張機能の使い方

### 6.1 パネルの開き方

| 方法 | 操作 |
|------|------|
| エディタツールバー | 右上のSparkアイコン（✱）をクリック |
| アクティビティバー | 左サイドバーのSparkアイコンをクリック |
| コマンドパレット | `Cmd+Shift+P` → 「Claude Code」と入力 |
| ステータスバー | ウィンドウ右下の「✱ Claude Code」をクリック |

### 6.2 キーボードショートカット

| 操作 | Mac | Windows/Linux |
|------|-----|---------------|
| フォーカス切替（エディタ⇄Claude） | `Cmd+Esc` | `Ctrl+Esc` |
| 新しいタブで開く | `Cmd+Shift+Esc` | `Ctrl+Shift+Esc` |
| @メンション参照の挿入 | `Option+K` | `Alt+K` |
| 新しい会話 | `Cmd+N`（要設定） | `Ctrl+N`（要設定） |

### 6.3 パーミッションモード

| モード | 説明 |
|--------|------|
| Normal | 各アクションの前に許可を求める（デフォルト） |
| Plan | 実装計画をMarkdownで表示し、承認後に実行する |
| Auto-accept | 編集を許可なしで適用する（信頼できる環境のみ） |

プロンプトボックス下部のモード表示をクリックして切り替える。

### 6.4 @メンションでコンテキストを提供

```
@auth.ts このファイルの認証ロジックを説明して

@src/server/api/users.ts#15-30 この部分にバリデーションを追加して

@src/components/ このフォルダ内のコンポーネント一覧を教えて
```

エディタでテキストを選択した状態で `Option+K`（Mac）/ `Alt+K` を押すと、選択範囲のファイルパスと行番号が自動挿入される。

### 6.5 スキルの使い方

プロンプトボックスで `/` を入力するとコマンドメニューが開く。プロジェクトに設定済みのスキルが利用可能:

```
/fix-issue 42        # Issue #42 を修正
/review-pr 15        # PR #15 をレビュー
/commit              # 変更をコミット
/create-api users    # users APIを生成
```

### 6.6 ターミナル連携

VS Codeの統合ターミナル（`` Ctrl+` `` / `` Cmd+` ``）で `claude` を実行すると、CLI版を使うこともできる。拡張機能版とCLI版は会話履歴を共有しているため、CLI版で `claude --resume` すると拡張機能の会話を引き継げる。

ターミナル出力をプロンプトで参照:
```
@terminal:ターミナル名 でターミナルの出力をClaudeに見せられる
```

## 7. チーム開発でのベストプラクティス

### 7.1 Writer/Reviewer パターン（VS Code版）

1. **Writerセッション**: メインのClaudeパネルで機能を実装
2. **Reviewerセッション**: 「Open in New Tab」で新しいClaudeタブを開き、PRレビューを依頼

```
# Reviewerタブで実行
/review-pr 15
```

### 7.2 Plan Mode活用

複雑な実装の前にPlan Modeに切り替えて計画をレビューする:

1. プロンプトボックス下部のモード表示をクリック → Plan を選択
2. 実装したい機能を説明する
3. VS Codeが自動的に計画をMarkdownドキュメントとして表示
4. 計画にインラインコメントでフィードバックを追加
5. 承認後にNormal Modeに戻して実装を開始

### 7.3 チェックポイント（Undo機能）

Claudeのメッセージにホバーすると、巻き戻しボタンが表示される:

- **Fork conversation from here**: この地点から会話を分岐（コードは変更なし）
- **Rewind code to here**: この地点にコードを巻き戻し（会話履歴は保持）
- **Fork and rewind**: 会話を分岐しつつコードも巻き戻し

### 7.4 Git Worktreeで並行開発

別々の機能を並行して開発する場合、worktreeフラグを使うとファイルの競合を防げる:

```bash
claude --worktree feature-auth      # 認証機能の開発
claude --worktree feature-dashboard  # ダッシュボードの開発
```

## 8. 拡張機能の推奨設定

VS Code設定（`Cmd+,` / `Ctrl+,` → Extensions → Claude Code）:

| 設定 | 推奨値 | 説明 |
|------|--------|------|
| Initial Permission Mode | `default` | 初心者はdefault、慣れたらplanを推奨 |
| Autosave | `true` | Claudeが読み書きする前にファイルを自動保存 |
| Respect Git Ignore | `true` | .gitignoreのパターンを検索から除外 |
| Use Python Environment | `true` | ワークスペースのPython環境を自動検出 |

## 9. トラブルシューティング

### Sparkアイコンが表示されない

1. ファイルが開いていることを確認（フォルダだけでは表示されない）
2. VS Codeのバージョンを確認（1.98.0以上が必要）
3. 「Developer: Reload Window」を実行
4. 他のAI拡張機能を一時的に無効化

### サインインできない

- `ANTHROPIC_API_KEY` を環境変数に設定している場合、ターミナルから `code .` で起動すると環境変数が引き継がれる
- 「Developer: Reload Window」を実行して再試行

### Claudeが応答しない

1. インターネット接続を確認
2. 新しい会話を開始して再試行
3. 統合ターミナルで `claude` を直接実行して詳細なエラーメッセージを確認

### 環境チェック

```bash
claude doctor    # インストール状況と設定の詳細診断
```

## 10. 新メンバーのオンボーディング手順

新しくチームに参加するメンバーは、以下の手順で環境を構築する:

1. **VS Codeをインストール**: https://code.visualstudio.com/ からダウンロード
2. **Claude Codeをインストール**: ターミナルで `curl -fsSL https://claude.ai/install.sh | bash`
3. **リポジトリをクローン**:
   ```bash
   git clone https://github.com/monogatari-it-dept/claude-code-setup.git
   cd claude-code-setup
   ```
4. **VS Codeで開く**: `code .`
5. **推奨拡張機能をインストール**: VS Codeが.vscode/extensions.jsonに基づいて推奨表示するので、すべてインストール
6. **Claude Codeにサインイン**: Sparkアイコンをクリックしてブラウザ認証
7. **個人設定ファイルを作成**:
   ```bash
   cp CLAUDE.local.md.template CLAUDE.local.md
   ```
8. **動作確認**: Claudeパネルで「このプロジェクトの構成を教えて」と入力して応答を確認
9. **スキルの確認**: `/` を入力してfix-issue、review-pr、commit等のスキルが利用可能か確認
