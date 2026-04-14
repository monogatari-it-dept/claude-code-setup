# GitHub CLI セットアップ & リポジトリ作成ガイド

## 1. GitHub CLIのインストール

### macOS（Homebrew）

```bash
brew install gh
```

### Windows

```bash
winget install --id GitHub.cli
```

### Linux（Ubuntu/Debian）

```bash
(type -p wget >/dev/null || (sudo apt update && sudo apt-get install wget -y)) \
  && sudo mkdir -p -m 755 /etc/apt/keyrings \
  && out=$(wget -nv -O- https://cli.github.com/packages/githubcli-archive-keyring.gpg) \
  && echo "$out" | sudo tee /etc/apt/keyrings/githubcli-archive-keyring.gpg > /dev/null \
  && sudo chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg \
  && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
  && sudo apt update \
  && sudo apt install gh -y
```

## 2. GitHub CLIの認証

```bash
gh auth login
```

対話的に以下を選択:
1. `GitHub.com` を選択
2. `HTTPS` を選択
3. 認証方法: `Login with a web browser` を選択
4. 表示されるワンタイムコードをブラウザで入力

認証の確認:
```bash
gh auth status
```

## 3. リポジトリの作成とプッシュ

```bash
# テンプレートフォルダに移動
cd ~/Documents/claude-code-team-template

# ブランチ名をmainに変更
git branch -m main

# 初回コミット
git commit -m "feat: Claude Code チーム開発環境テンプレートを追加

- CLAUDE.md: プロジェクト共通ルール（コードスタイル、ビルドコマンド、アーキテクチャ）
- .claude/settings.json: パーミッション、Hooks（自動フォーマット、通知、タスク完了チェック）
- .claude/rules/: パス別ルール（フロントエンド、バックエンド、テスト）
- .claude/skills/: カスタムスキル（fix-issue, review-pr, commit, create-api）
- .claude/agents/: サブエージェント（security-reviewer, test-writer）
- .github/: PRテンプレート、Issueテンプレート
- docs/: チーム開発ガイドライン"

# GitHubにプライベートリポジトリを作成してプッシュ
gh repo create monogatari.co.jp/IT-department --private --source=. --push
```

## 4. チームメンバーの初期セットアップ

チームメンバーがリポジトリをクローンした後に実行する手順:

```bash
# リポジトリをクローン
git clone https://github.com/monogatari.co.jp/IT-department.git
cd IT-department

# 個人設定ファイルを作成
cp CLAUDE.local.md.template CLAUDE.local.md

# CLAUDE.local.md を編集して個人の好みを記入
# （このファイルは .gitignore に含まれるのでチームには共有されない）

# 依存パッケージのインストール（package.json作成後）
npm install
```

## 5. Claude Codeの推奨初期設定（各メンバー）

ユーザーレベルの設定（`~/.claude/settings.json`）に以下を追加すると便利:

```json
{
  "autoMemoryEnabled": true
}
```

ユーザーレベルのCLAUDE.md（`~/.claude/CLAUDE.md`）の例:

```markdown
# 個人設定
- 回答は日本語で行う
- コミットメッセージは日本語で書く
```

## 6. 動作確認

```bash
# Claude Codeでプロジェクトを開く
cd IT-department
claude

# 以下を確認:
# 1. /memory でCLAUDE.mdが読み込まれていることを確認
# 2. /hooks でHooksが設定されていることを確認
# 3. /fix-issue や /commit などのスキルが利用可能か確認
```
