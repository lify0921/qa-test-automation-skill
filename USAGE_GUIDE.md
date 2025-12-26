# QA Test Automation Skill - チーム共有ガイド

このガイドでは、qa-test-automationスキルをチームメンバーと共有する方法を説明します。

## 🚀 リポジトリの公開（管理者向け）

### オプション1: GitHub に公開（推奨）

#### 1. GitHubでリポジトリを作成

1. GitHubにログイン
2. 右上の「+」→「New repository」をクリック
3. リポジトリ名: `qa-test-automation-skill` または任意の名前
4. 「Public」または「Private」を選択（チーム内共有なら Private）
5. 「Create repository」をクリック

#### 2. ローカルリポジトリをGitHubにプッシュ

```bash
# 現在のディレクトリを確認
pwd
# /Users/ueda/qa-test-automation-skill であることを確認

# リモートリポジトリを追加
git remote add origin https://github.com/your-username/qa-test-automation-skill.git

# mainブランチにプッシュ
git push -u origin main
```

#### 3. チームメンバーに共有

GitHubのリポジトリURLをチームメンバーに共有：
```
https://github.com/your-username/qa-test-automation-skill
```

### オプション2: 社内Gitサーバーに公開

社内のGitサーバー（GitLab、Bitbucket等）を使用する場合：

```bash
# リモートリポジトリを追加
git remote add origin https://your-git-server.com/your-team/qa-test-automation-skill.git

# プッシュ
git push -u origin main
```

### オプション3: ファイル共有（Git不使用）

Gitを使わない場合、ZIPファイルとして共有：

```bash
# 現在のディレクトリで実行
cd /Users/ueda
tar -czf qa-test-automation-skill.tar.gz qa-test-automation-skill

# またはZIPで圧縮
zip -r qa-test-automation-skill.zip qa-test-automation-skill
```

生成された `qa-test-automation-skill.tar.gz` または `qa-test-automation-skill.zip` をチームメンバーに共有。

---

## 📥 スキルのインストール（チームメンバー向け）

### 方法1: Gitリポジトリからインストール（推奨）

#### ステップ1: リポジトリをクローン

```bash
# ホームディレクトリまたは任意の場所でクローン
cd ~
git clone https://github.com/your-username/qa-test-automation-skill.git
```

#### ステップ2: スキルをClaude Codeにインストール

**プロジェクトスコープ（推奨）**:
```bash
# プロジェクトディレクトリに移動
cd /path/to/your-project

# スキルディレクトリを作成
mkdir -p .claude/skills

# スキルをコピー
cp -r ~/qa-test-automation-skill/qa-test-automation .claude/skills/
```

**パーソナルスコープ（全プロジェクトで使用）**:
```bash
# パーソナルスキルディレクトリを作成
mkdir -p ~/.claude/skills

# スキルをコピー
cp -r ~/qa-test-automation-skill/qa-test-automation ~/.claude/skills/
```

#### ステップ3: インストール確認

Claude Codeを起動：
```bash
claude
```

スキルが利用可能か確認：
```
What skills are available?
```

`qa-test-automation` が表示されればインストール成功！

### 方法2: ZIPファイルからインストール

#### ステップ1: ZIPファイルを解凍

```bash
# ダウンロードしたZIPを解凍
unzip qa-test-automation-skill.zip
# または
tar -xzf qa-test-automation-skill.tar.gz
```

#### ステップ2以降

方法1の「ステップ2」と「ステップ3」と同じ手順でインストール。

---

## 💡 使用方法

### 基本的な使い方

Claude Codeセッション内で：

```
/qa-test-automation
```

### 実際の使用例

**例: API仕様書とソースコードからテスト生成**

```
/qa-test-automation

インプット情報：
- fetch でAPI仕様書を読み込んで： https://realworld-docs.netlify.app/docs/specs/backend-specs/endpoints
- GitHub MCP でソースコードを読み取って： リポジトリ gothinkster/node-express-realworld-example-app の src/routes/api/articles.js
```

**生成される成果物：**
- `test_plan.md`: テスト計画書
- `test_design.md`: テスト設計書（バリデーション漏れチェック）
- `test_cases.md`: 具体的なテストケース

---

## 🔄 スキルの更新

### チームメンバーが更新を取得する方法

スキルが更新された場合：

```bash
# リポジトリディレクトリに移動
cd ~/qa-test-automation-skill

# 最新版を取得
git pull origin main

# プロジェクトのスキルを更新
cp -r qa-test-automation /path/to/your-project/.claude/skills/

# またはパーソナルスキルを更新
cp -r qa-test-automation ~/.claude/skills/
```

---

## 🛠️ カスタマイズ

### テンプレートの編集

組織の標準に合わせてテンプレートをカスタマイズ：

```bash
# プロジェクトのテンプレートを編集
code /path/to/your-project/.claude/skills/qa-test-automation/templates/test_plan_template.md
code /path/to/your-project/.claude/skills/qa-test-automation/templates/test_design_template.md
code /path/to/your-project/.claude/skills/qa-test-automation/templates/test_case_template.md
```

**注意**: プロジェクトスコープで編集した場合、その変更はそのプロジェクトにのみ適用されます。

### カスタマイズを共有

1. テンプレートを編集
2. リポジトリにコミット＆プッシュ
3. チームメンバーが `git pull` で更新取得

---

## ❓ トラブルシューティング

### Q: スキルが表示されない

**A**: インストール先を確認：

```bash
# プロジェクトスコープの場合
ls -la .claude/skills/qa-test-automation/

# パーソナルスコープの場合
ls -la ~/.claude/skills/qa-test-automation/
```

`SKILL.md` が存在することを確認。存在しない場合は再インストール。

### Q: スキルが古いバージョンのまま

**A**: リポジトリを更新してコピーし直す：

```bash
cd ~/qa-test-automation-skill
git pull origin main
cp -r qa-test-automation /path/to/your-project/.claude/skills/
```

### Q: GitHubへのプッシュ時に認証エラー

**A**: Personal Access Token (PAT) を使用：

1. GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)
2. "Generate new token" をクリック
3. `repo` にチェックを入れて生成
4. プッシュ時にパスワードの代わりにトークンを使用

---

## 📚 関連ドキュメント

- [README.md](README.md): プロジェクト概要とインストール方法
- [qa-test-automation/SKILL.md](qa-test-automation/SKILL.md): スキルの詳細説明
- [qa-test-automation/IMPLEMENTATION.md](qa-test-automation/IMPLEMENTATION.md): 実装ガイド
- [qa-test-automation/README.md](qa-test-automation/README.md): 使用方法ガイド

---

## 🤝 貢献

スキルの改善提案やバグ報告は以下の方法で：

1. GitHubでIssueを作成
2. プルリクエストを送信
3. チーム内で議論

---

## 📝 ライセンス

このスキルはMITライセンスの下で公開されています。詳細は [LICENSE](LICENSE) を参照。

---

## 📧 サポート

質問や問題がある場合：
- リポジトリのIssueに投稿
- チーム内のSlack/Teamsチャンネルで質問
- ドキュメントを参照

Happy Testing! 🎉
