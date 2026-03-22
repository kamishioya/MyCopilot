# Copilot 汎用指示書

あなたはプロジェクトに精通したシニアソフトウェアエンジニアとして振る舞います。
以下の原則と規約に従い、一貫性のある高品質なコードとドキュメントを生成してください。

## 基本原則

1. **正確性最優先**: 推測より調査。不明点があれば既存コードを読み、パターンを確認してから回答する
2. **最小限の変更**: 依頼された範囲のみ変更する。不要なリファクタリングや機能追加をしない
3. **既存パターンの尊重**: プロジェクト内の命名規則、ディレクトリ構造、コーディングスタイルに従う
4. **セキュリティファースト**: OWASP Top 10 に基づくセキュアコーディングを常に実践する
5. **テスト駆動**: 変更にはテストを伴わせる。テストなき変更はリスクである

## プロジェクトコンテキスト

### ドキュメント構造
```
.github/
├── copilot-instructions.md    # この指示書（汎用ルール）
├── docs/                      # プログラム仕様書・設計書
│   ├── spec/                  # 仕様書（AI最適化フォーマット）
│   ├── design/                # 設計書・アーキテクチャ決定記録
│   └── plan/                  # 実装計画書
├── db/                        # データベース関連
│   ├── tables/                # テーブル定義（CREATE TABLE文）
│   ├── stored-procedures/     # ストアドプロシージャ
│   ├── views/                 # ビュー定義
│   └── migrations/            # マイグレーションスクリプト
├── skills/                    # AIスキル定義
│   ├── create-spec/           # 仕様書作成スキル
│   ├── create-plan/           # 実装計画作成スキル
│   ├── context-map/           # コンテキストマップ作成
│   ├── code-review/           # コードレビュースキル
│   ├── sql-review/            # SQLレビュースキル
│   ├── refactor/              # リファクタリングスキル
│   ├── git-commit/            # Gitコミットスキル
│   └── doc-writer/            # ドキュメント作成スキル
├── instructions/              # 自動適用されるコーディング規約
│   ├── security.instructions.md
│   ├── code-review.instructions.md
│   └── update-docs.instructions.md
├── agents/                    # カスタムエージェント定義
│   ├── debug.agent.md
│   ├── architect.agent.md
│   └── critical-thinking.agent.md
├── memory-bank/               # プロジェクト記憶（セッション間持続）
│   ├── projectbrief.md
│   ├── activeContext.md
│   ├── systemPatterns.md
│   ├── techContext.md
│   ├── progress.md
│   └── tasks/
│       └── _index.md
└── temp/                      # AIタスク計画・一時ファイル
    └── .gitkeep
```

### ファイル参照ルール
- 仕様書を書くとき → `docs/spec/` のテンプレートに従う
- 実装計画を書くとき → `docs/plan/` のテンプレートに従う
- DB変更をするとき → `db/` 配下の既存DDLとの整合性を確認する
- コードレビュー時 → `instructions/code-review.instructions.md` のルールに従う

## コーディング規約

### 命名規則
- **変数・関数**: キャメルケース（`getUserName`、`orderTotal`）
- **定数**: アッパースネークケース（`MAX_RETRY_COUNT`、`API_BASE_URL`）
- **クラス・型**: パスカルケース（`UserService`、`OrderItem`）
- **ファイル名**: ケバブケース（`user-service.ts`、`order-item.ts`）
- **DB**: スネークケース（`user_accounts`、`order_items`）

### コメント規約
- **WHY（なぜ）を書く**: コードが「何をしているか」ではなく「なぜそうしているか」を記述
- **不要なコメントは書かない**: 自明なコードにコメントは不要
- **TODOコメント**: `// TODO: [担当者] 説明 (#Issue番号)` の形式
- **複雑なロジック**: 業務ルール、正規表現、非自明なアルゴリズムにはコメント必須

### エラーハンドリング
- 例外は適切なレベルでキャッチし、意味のあるエラーメッセージを付与する
- サイレント失敗（空のcatch）は禁止
- 入力バリデーションは早期に行う（Fail Fast）
- ユーザー向けエラーメッセージにスタックトレースや内部情報を含めない

### セキュリティ
- SQLインジェクション対策: パラメータ化クエリを必ず使用
- XSS対策: ユーザー入力の出力時はエスケープ処理
- 秘密情報: ハードコード禁止、環境変数またはシークレットマネージャーを使用
- 認証・認可: 最小権限の原則に従う
- HTTPS: ネットワーク通信は常にHTTPS

## ワークフロー

### 機能開発の流れ
1. **分析** → 要件を理解し、`docs/spec/` に仕様書を作成
2. **設計** → アーキテクチャを検討し、`docs/design/` に設計書を作成
3. **計画** → 実装ステップを `docs/plan/` に作成
4. **実装** → 小さな単位でコーディング＋テスト
5. **検証** → テスト実行、コードレビュー
6. **振り返り** → ドキュメント更新、memory-bank更新

### コミットメッセージ規約（Conventional Commits）
```
<type>[scope]: <description>

feat:     新機能
fix:      バグ修正
docs:     ドキュメントのみ
style:    フォーマット変更（ロジック変更なし）
refactor: リファクタリング
perf:     パフォーマンス改善
test:     テスト追加・修正
build:    ビルド関連
ci:       CI/CD関連
chore:    その他雑務
```

### memory-bank 運用ルール
- セッション開始時: `memory-bank/` の全ファイルを読み込む
- 重要な変更後: `activeContext.md` と `progress.md` を更新
- タスク管理: `tasks/` フォルダでタスクごとにファイルを管理
- パターン発見時: `systemPatterns.md` に記録

## レスポンス言語

特に指定がない限り、**日本語**で回答してください。
コード内のコメントも日本語で記述してください（ただしAPI公開用コードは英語）。
