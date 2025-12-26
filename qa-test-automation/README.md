# QAテスト自動化スキル - 使用ガイド

このスキルは、仕様書とソースコードを自動的に解析して、テスト計画、テスト設計、テストケースを生成します。

## クイックスタート

### 1. スキルの確認

Claude Codeで以下のように入力して、スキルが利用可能か確認します：

```
What skills are available?
```

`qa-test-automation` が表示されることを確認してください。

### 2. 基本的な使用方法

プロジェクトディレクトリで以下のコマンドを実行：

```
/qa-test-automation
```

スキルが自動的に以下を実行します：

1. 仕様書ファイルを検索
2. ソースコードファイルを解析
3. テスト計画書を生成 (`test_plan.md`)
4. テスト設計書を生成 (`test_design.md`)
5. テストケースを生成 (`test_cases/` ディレクトリ)

### 3. 生成されるファイル

実行後、以下のファイルが生成されます：

```
your-project/
├── test_plan.md              # テスト計画書
├── test_design.md            # テスト設計書
└── test_cases/               # テストケースディレクトリ
    ├── TC001_機能A_正常系.md
    ├── TC002_機能A_異常系.md
    ├── TC003_機能B_正常系.md
    └── ...
```

## 詳細な使用例

### 例1: Webアプリケーションのテスト

プロジェクト構成：
```
web-app/
├── docs/
│   └── specification.md      # 仕様書
├── src/
│   ├── components/
│   │   └── Login.jsx
│   ├── services/
│   │   └── auth.js
│   └── utils/
│       └── validation.js
└── README.md
```

実行手順：
```bash
cd web-app
/qa-test-automation
```

結果：
- ログイン機能のテスト計画が生成される
- 認証、バリデーション、エラーハンドリングのテスト設計が作成される
- 具体的なテストケースが10-20件生成される

### 例2: REST APIのテスト

プロジェクト構成：
```
api-server/
├── docs/
│   └── api-spec.yaml         # OpenAPI仕様
├── src/
│   ├── routes/
│   │   ├── users.js
│   │   └── posts.js
│   └── middleware/
│       └── auth.js
└── package.json
```

実行手順：
```bash
cd api-server
/qa-test-automation
```

結果：
- API全体のテスト計画が生成される
- エンドポイントごとのテスト設計（GET, POST, PUT, DELETE）
- リクエスト/レスポンスのテストケース
- 認証・認可のテストケース

### 例3: 特定のファイルのみを対象にする

仕様書とソースコードのパスを指定：

```
/qa-test-automation docs/user-management.md src/services/user.js
```

結果：
- user-management機能に特化したテストドキュメントが生成される

## カスタマイズ

### テンプレートのカスタマイズ

組織の標準フォーマットに合わせてテンプレートを編集できます：

```bash
# テスト計画書テンプレートを編集
code ~/.claude/skills/qa-test-automation/templates/test_plan_template.md

# テスト設計書テンプレートを編集
code ~/.claude/skills/qa-test-automation/templates/test_design_template.md

# テストケーステンプレートを編集
code ~/.claude/skills/qa-test-automation/templates/test_case_template.md
```

編集後、次回の実行時から新しいテンプレートが使用されます。

### プロジェクト固有のテンプレート

プロジェクトごとに異なるテンプレートを使いたい場合：

```bash
# プロジェクトローカルのスキルディレクトリにコピー
mkdir -p .claude/skills/qa-test-automation/templates
cp ~/.claude/skills/qa-test-automation/templates/* .claude/skills/qa-test-automation/templates/

# プロジェクト固有のテンプレートを編集
code .claude/skills/qa-test-automation/templates/test_plan_template.md
```

## 高度な使用方法

### オプション指定（カスタム引数）

スキル実行時に特定のオプションを指定できます：

```
/qa-test-automation --plan-only
```
→ テスト計画書のみを生成

```
/qa-test-automation --design-only
```
→ テスト設計書のみを生成

```
/qa-test-automation --cases-only
```
→ テストケースのみを生成（test_design.mdが既に存在する必要があります）

```
/qa-test-automation --priority=high
```
→ 優先度が高いテストケースのみを生成

### 段階的な生成

大規模プロジェクトの場合、段階的に生成することを推奨します：

```bash
# ステップ1: まずテスト計画を生成
/qa-test-automation --plan-only

# レビューと調整
code test_plan.md

# ステップ2: テスト設計を生成
/qa-test-automation --design-only

# レビューと調整
code test_design.md

# ステップ3: テストケースを生成
/qa-test-automation --cases-only
```

### 機能ごとの分割生成

特定の機能ごとにテストを生成：

```bash
# ログイン機能
/qa-test-automation docs/login-spec.md src/auth/login.js

# ユーザー登録機能
/qa-test-automation docs/signup-spec.md src/auth/signup.js

# 決済機能
/qa-test-automation docs/payment-spec.md src/payment/processor.js
```

## ベストプラクティス

### 1. 仕様書を整備する

スキルが高品質なテストを生成するには、明確な仕様書が必要です：

**良い仕様書の例：**
```markdown
# ログイン機能仕様

## 目的
ユーザーが安全にシステムにアクセスできるようにする

## 機能要件
- メールアドレスとパスワードで認証
- 3回失敗後にアカウントをロック
- ログイン成功後にダッシュボードにリダイレクト

## 非機能要件
- 応答時間: 2秒以内
- SQLインジェクション対策を実施
- パスワードはbcryptでハッシュ化
```

### 2. 生成後のレビューを忘れずに

自動生成されたテストドキュメントは、必ず人間がレビューしてください：

- [ ] 仕様書の要件がすべて網羅されているか
- [ ] テストケースの手順が実行可能か
- [ ] テストデータが適切か
- [ ] エッジケースが考慮されているか

### 3. 段階的に改善する

最初の生成結果が完璧でなくても問題ありません：

1. まず自動生成で基礎を作る
2. レビューして不足している項目を追加
3. 実際にテストを実行して改善
4. テンプレートをカスタマイズして次回に活かす

### 4. 既存のテストと統合する

既にテストコードがある場合：

1. 既存テストのカバレッジを確認
2. 不足している観点を特定
3. スキルで補完的なテストケースを生成
4. 既存テストと統合

## トラブルシューティング

### 問題: 仕様書が見つからない

**症状:**
```
仕様書ファイルが見つかりませんでした
```

**解決策:**
1. 仕様書のパスを明示的に指定：
   ```
   /qa-test-automation docs/spec.md
   ```

2. 仕様書がない場合は、README.mdから推測するよう指示：
   ```
   Please generate tests based on README.md and source code analysis
   ```

### 問題: テストケースが少なすぎる

**症状:**
```
生成されたテストケースが3件しかない
```

**解決策:**
1. テスト設計書を確認し、確認項目を追加：
   ```
   code test_design.md
   ```

2. 追加の観点を指定して再生成：
   ```
   Please add test cases for:
   - Security (XSS, CSRF, SQL injection)
   - Performance (response time, concurrent users)
   - Edge cases (empty input, special characters)
   ```

### 問題: テストケースが汎用的すぎる

**症状:**
```
テストケースの手順が「機能をテストする」など抽象的
```

**解決策:**
1. より詳細な仕様書を提供する
2. 既存のテストコードを参考にするよう指示：
   ```
   Please analyze existing test code in tests/ directory and generate similar detailed test cases
   ```

### 問題: 生成に時間がかかる

**症状:**
```
大規模プロジェクトで生成に10分以上かかる
```

**解決策:**
1. 機能ごとに分割して生成：
   ```
   /qa-test-automation src/auth/
   /qa-test-automation src/payment/
   ```

2. 優先度の高い機能から生成：
   ```
   /qa-test-automation --priority=high
   ```

## 関連ファイル

- `SKILL.md`: スキルの詳細説明と使用方法
- `IMPLEMENTATION.md`: 実装の詳細ガイド（開発者向け）
- `templates/`: テンプレートファイル
  - `test_plan_template.md`
  - `test_design_template.md`
  - `test_case_template.md`

## サポート

問題が発生した場合：

1. スキルのSKILL.mdを確認
2. IMPLEMENTATION.mdで詳細な実装手順を確認
3. テンプレートファイルをカスタマイズ
4. Claude Codeに質問（例: "How do I customize test templates?"）

## 更新履歴

- **2025-12-25**: 初版リリース
  - テスト計画、テスト設計、テストケースの自動生成機能
  - 仕様書とソースコードの自動解析
  - カスタマイズ可能なテンプレート
