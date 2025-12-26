---
name: qa-test-automation
description: 仕様書とソースコードを解析して、テスト計画、テスト設計、テストケースを自動生成します。QAテスト、テストドキュメント作成、テスト自動化が必要な場合に使用します。
---

# QAテスト自動化スキル

このスキルは、仕様書とソースコードから自動的にテストドキュメントを生成します。

## 機能

### 基本機能

1. **テスト計画生成**: 目的、スコープ、リソース、スケジュールを含むテスト計画書を作成
2. **テスト設計生成**: テスト観点と確認項目を含むテスト設計書を作成
3. **テストケース生成**: 具体的な手順、データ、期待値を含むテストケースを作成

### 高度な機能（差分分析とリスクベーステスト）

4. **コード差分分析**: Git diffを使って変更箇所を自動検出し、変更の影響範囲を分析
5. **リスク評価**: 変更内容に基づいて自動的にリスクレベルを判定（Critical/High/Medium/Low）
6. **リスクベーステスト**: 高リスクの変更に対して重点的なテストを実施
7. **仕様書との照合**: 変更が仕様書の要件と一致しているかを確認

## 使用方法

### 1. 基本的な使用方法

```
/qa-test-automation
```

このコマンドを実行すると、以下のステップでテストドキュメントを生成します：

1. 仕様書とソースコードの場所を確認
2. ファイルを解析してテスト対象を特定
3. テスト計画書を生成
4. テスト設計書を生成
5. テストケースを生成

### 2. 実行手順

スキルが以下の手順で実行されます：

#### ステップ1: 仕様書とソースコードの収集

- 仕様書ファイル（.md, .txt, .pdf など）を検索
- 関連するソースコードファイルを検索
- ユーザーに対象ファイルを確認

#### ステップ2: テスト計画の生成

- 仕様書から目的とスコープを抽出
- 必要なリソースとスケジュールを推定
- `templates/test_plan_template.md` をベースに生成
- 生成結果を `test_plan.md` として保存

#### ステップ3: テスト設計の生成

- ソースコードの機能を分析
- テスト観点（機能テスト、非機能テストなど）を抽出
- 確認項目一覧を作成
- `templates/test_design_template.md` をベースに生成
- 生成結果を `test_design.md` として保存

#### ステップ4: テストケースの生成

- テスト設計の確認項目ごとにテストケースを作成
- 具体的なテスト手順を記述
- 必要なテストデータと期待値を定義
- `templates/test_case_template.md` をベースに生成
- 生成結果を `test_cases/` ディレクトリに保存

## テンプレート

スキルは以下のテンプレートを使用します：

- `templates/test_plan_template.md`: テスト計画書のテンプレート
- `templates/test_design_template.md`: テスト設計書のテンプレート
- `templates/test_case_template.md`: テストケースのテンプレート

## 出力ファイル

生成されるファイル：

```
project/
├── test_plan.md              # テスト計画書
├── test_design.md            # テスト設計書
└── test_cases/               # テストケースディレクトリ
    ├── TC001_機能名.md
    ├── TC002_機能名.md
    └── ...
```

## 例

### 例1: Webアプリケーションのログイン機能

**入力:**
- 仕様書: `specs/login_spec.md`
- ソースコード: `src/auth/login.js`

**実行:**
```
/qa-test-automation
```

**出力:**
- `test_plan.md`: ログイン機能のテスト計画
- `test_design.md`: ログイン機能のテスト設計（正常系、異常系、セキュリティなど）
- `test_cases/TC001_正常ログイン.md`
- `test_cases/TC002_パスワード誤り.md`
- `test_cases/TC003_アカウントロック.md`

### 例2: REST APIのテスト

**入力:**
- 仕様書: `docs/api_spec.yaml` (OpenAPI)
- ソースコード: `src/api/endpoints.py`

**実行:**
```
/qa-test-automation
```

**出力:**
- `test_plan.md`: API全体のテスト計画
- `test_design.md`: エンドポイントごとのテスト設計
- `test_cases/TC001_GET_users.md`
- `test_cases/TC002_POST_users.md`
- `test_cases/TC003_エラーハンドリング.md`

### 例3: 差分分析とリスクベーステスト（新機能）

**シナリオ:** feature/add-auth ブランチで認証機能を追加した後、mainブランチとの差分を分析してテストを生成

**入力:**
- 仕様書: `specs/auth_spec.md`
- ソースコード: `src/auth/`
- 差分: `git diff main...feature/add-auth`

**実行:**
```
/qa-test-automation

インプット情報：
- 仕様書: specs/auth_spec.md
- ソースコード: src/auth/
- Git差分を分析： main...feature/add-auth ブランチの差分を確認して、リスクベーステストを実施
```

**プロセス:**
1. Git diffで変更ファイルを検出:
   - `src/auth/login.js` (新規追加 - Critical Risk)
   - `src/middleware/auth.js` (新規追加 - Critical Risk)
   - `src/routes/api.js` (修正 - Medium Risk)

2. リスク評価:
   - Critical: 認証ロジックの追加（login.js, auth.js）
   - Medium: APIルート変更（api.js）

3. 仕様書との照合:
   - login.js: 仕様書の認証要件と一致 ✓
   - auth.js: トークン検証の実装を確認 ✓
   - api.js: 保護されたエンドポイントの定義 ✓

**出力:**
- `test_plan.md`: リスク評価に基づいた工数見積（Critical: 16時間、Medium: 4時間）
- `test_design.md`:
  - セクション2に差分分析とリスク評価の詳細
  - Critical Riskの変更に対する包括的なテスト観点
- `test_cases/`:
  - TC001_認証バイパステスト.md（Critical - 最優先）
  - TC002_トークン有効性検証.md（Critical - 最優先）
  - TC003_セッション管理.md（Critical - 最優先）
  - TC004_APIルート保護.md（Medium - 標準）

## カスタマイズ

### 特定のファイルのみを対象にする

仕様書とソースコードのパスを指定して実行：

```
/qa-test-automation specs/specific_feature.md src/feature.js
```

### テスト計画のみを生成

```
/qa-test-automation --plan-only
```

### テストケースのみを生成（計画・設計は既存）

```
/qa-test-automation --cases-only
```

## 注意事項

- 仕様書は日本語または英語で記述されている必要があります
- ソースコードは一般的なプログラミング言語（JavaScript, Python, Java, C#など）に対応しています
- 生成されたテストドキュメントは、必ず人間がレビューして適宜修正してください
- セキュリティやパフォーマンスなど、特殊なテスト要件は追加で記述が必要な場合があります

## トラブルシューティング

### 仕様書が見つからない場合

プロジェクトルートに仕様書を配置するか、パスを明示的に指定してください。

### テストケースが不十分な場合

テスト設計を確認し、不足している確認項目を追加してから再実行してください。

### カスタムテンプレートを使いたい場合

`.claude/skills/qa-test-automation/templates/` 内のテンプレートファイルを編集してください。
