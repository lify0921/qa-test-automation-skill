# QA Test Automation Skill for Claude Code

Claude Code用のQAテスト自動化スキル。仕様書とソースコードから、テスト計画、テスト設計、テストケースを自動生成します。

## 機能

- **テスト計画書の自動生成**: 目的、スコープ、リソース、スケジュールを含む包括的なテスト計画
- **テスト設計書の自動生成**: テスト観点と確認項目を抽出し、バリデーション漏れを重点的にチェック
- **テストケースの自動生成**: 具体的な手順、データ、期待値を含む実行可能なテストケース

## インストール方法

### 1. リポジトリのクローン

```bash
git clone <your-repository-url> qa-test-automation-skill
```

### 2. スキルのインストール

プロジェクトスコープ（推奨 - チーム共有）:
```bash
# プロジェクトディレクトリに移動
cd your-project

# スキルディレクトリを作成
mkdir -p .claude/skills

# スキルをコピー
cp -r /path/to/qa-test-automation-skill/qa-test-automation .claude/skills/
```

パーソナルスコープ（個人用 - 全プロジェクトで使用可能）:
```bash
# パーソナルスキルディレクトリを作成
mkdir -p ~/.claude/skills

# スキルをコピー
cp -r /path/to/qa-test-automation-skill/qa-test-automation ~/.claude/skills/
```

### 3. インストール確認

Claude Codeを起動して、スキルが利用可能か確認：

```
What skills are available?
```

`qa-test-automation` が表示されればインストール成功です。

## 使用方法

### 基本的な使い方

```
/qa-test-automation
```

このコマンドを実行すると、以下のステップでテストドキュメントを生成します：
1. 仕様書とソースコードの検索
2. テスト計画書の生成 (`test_plan.md`)
3. テスト設計書の生成 (`test_design.md`)
4. テストケースの生成 (`test_cases.md`)

### 実行例

#### 例1: API仕様書とソースコードから生成

```
/qa-test-automation

インプット情報：
- fetch でAPI仕様書を読み込んで： https://example.com/api/spec
- GitHub MCP でソースコードを読み取って： リポジトリ user/repo の src/api/users.js
```

#### 例2: ローカルファイルから生成

```
/qa-test-automation

インプット情報：
- 仕様書: docs/specification.md
- ソースコード: src/services/auth.js
```

## 生成される成果物

実行後、以下のファイルが生成されます：

```
your-project/
├── test_plan.md          # テスト計画書
├── test_design.md        # テスト設計書（バリデーション漏れを重点チェック）
└── test_cases.md         # 具体的なテストケース
```

## 主な特徴

### バリデーション漏れの自動検出

実装を分析し、以下のような見落とされがちなバリデーション漏れを自動的に検出：
- 必須パラメータのチェック漏れ
- データ型の検証不足
- 境界値のチェック漏れ

### エッジケースの考慮

一般的に見落とされがちなテストケースを自動生成：
- 重複データの処理（例: 重複タイトルでのスラッグ一意性）
- データの正規化（例: タグの大文字小文字統一）
- DoS対策（例: 巨大ペイロードの拒否）

### セキュリティテスト

セキュリティ観点のテストケースを自動生成：
- XSS攻撃への耐性
- 認証・認可のバイパステスト
- SQLインジェクション対策

## カスタマイズ

### テンプレートの編集

組織の標準フォーマットに合わせてテンプレートを編集できます：

```bash
# テスト計画書テンプレート
code .claude/skills/qa-test-automation/templates/test_plan_template.md

# テスト設計書テンプレート
code .claude/skills/qa-test-automation/templates/test_design_template.md

# テストケーステンプレート
code .claude/skills/qa-test-automation/templates/test_case_template.md
```

## 詳細ドキュメント

スキルディレクトリ内の詳細ドキュメント：
- `qa-test-automation/SKILL.md`: スキルの詳細説明
- `qa-test-automation/IMPLEMENTATION.md`: 実装ガイド（開発者向け）
- `qa-test-automation/README.md`: 使用方法ガイド

## システム要件

- Claude Code CLI
- Node.js 18以上（推奨）
- Git（リポジトリのクローン用）

## 対応している入力形式

### 仕様書
- Markdown (.md)
- テキストファイル (.txt)
- OpenAPI仕様 (.yaml, .yml)
- PDF (.pdf)

### ソースコード
- JavaScript/TypeScript (.js, .ts)
- Python (.py)
- Java (.java)
- C# (.cs)
- Go (.go)
- その他の一般的なプログラミング言語

## トラブルシューティング

### スキルが表示されない

1. インストール先が正しいか確認:
   ```bash
   ls -la .claude/skills/qa-test-automation/
   ```

2. Claude Codeを再起動

### 生成されたテストが不十分

1. より詳細な仕様書を提供
2. テスト設計書を確認し、不足している観点を手動で追加
3. テンプレートをカスタマイズ

## ライセンス

このスキルは自由に使用・改変できます。

## 貢献

改善提案やバグ報告は、Issueまたはプルリクエストでお願いします。

## サポート

質問や問題がある場合は、以下を参照してください：
- スキルの詳細: `qa-test-automation/SKILL.md`
- 実装ガイド: `qa-test-automation/IMPLEMENTATION.md`
- Claude Code公式ドキュメント: https://code.claude.com/docs
