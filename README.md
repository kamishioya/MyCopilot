#  汎用 Code as Doc テンプレート

AI駆動開発のための `.github` フォルダテンプレートです。  
[awesome-copilot](https://github.com/microsoft/awesome-copilot) プロジェクトのベストプラクティスを参考に構築しています。

## フォルダ構成

```
MyCopilot/
├── copilot-instructions.md          # 🎯 汎用AI指示書（プロジェクト全体のルール）
│
├── docs/                            # 📄 プログラム仕様書・設計書
│   ├── spec/                        #    仕様書（AI最適化フォーマット）
│   ├── design/                      #    設計書・アーキテクチャ決定記録
│   └── plan/                        #    実装計画書
│
├── db/                              # 🗄️ データベース関連
│   ├── tables/                      #    テーブル定義（CREATE TABLE文）
│   ├── stored-procedures/           #    ストアドプロシージャ
│   ├── views/                       #    ビュー定義
│   └── migrations/                  #    マイグレーションスクリプト
│
├── skills/                          # 🛠️ AIスキル定義
│   ├── create-spec/SKILL.md         #    仕様書作成
│   ├── create-plan/SKILL.md         #    実装計画作成
│   ├── context-map/SKILL.md         #    コンテキストマップ（影響範囲分析）
│   ├── code-review/SKILL.md         #    コードレビュー
│   ├── sql-review/SKILL.md          #    SQLレビュー
│   ├── refactor/SKILL.md            #    リファクタリング
│   ├── git-commit/SKILL.md          #    Gitコミット（Conventional Commits）
│   └── doc-writer/SKILL.md          #    ドキュメント作成（Diátaxisフレームワーク）
│
├── instructions/                    # 📏 自動適用されるコーディング規約
│   ├── security.instructions.md     #    OWASP準拠セキュリティガイドライン
│   ├── code-review.instructions.md  #    コードレビュー基準
│   └── update-docs.instructions.md  #    ドキュメント同期更新ルール
│
├── agents/                          # 🤖 カスタムエージェント
│   ├── debug.agent.md               #    デバッグモード（体系的バグ解決）
│   ├── architect.agent.md           #    アーキテクト（設計・図面のみ、コード生成なし）
│   └── critical-thinking.agent.md   #    クリティカルシンキング（前提検証）
│
├── memory-bank/                     # 🧠 プロジェクト記憶（セッション間持続）
│   ├── projectbrief.md              #    プロジェクト概要
│   ├── activeContext.md             #    現在の作業コンテキスト
│   ├── systemPatterns.md            #    アーキテクチャ・設計パターン
│   ├── techContext.md               #    技術スタック・環境情報
│   ├── progress.md                  #    進捗状況・既知の問題
│   └── tasks/                       #    タスク管理
│       └── _index.md               #    タスクインデックス
│
└── temp/                            # 📝 AIタスク計画・一時ファイル
    └── .gitkeep
```

## 使い方

### 1. プロジェクトにコピー
この `MyCopilot` フォルダの内容を、対象プロジェクトの `.github` フォルダとしてコピーします。

### 2. memory-bank を初期化
`memory-bank/projectbrief.md` にプロジェクトの概要を記入します。  
`memory-bank/techContext.md` に技術スタック情報を記入します。

### 3. 開発ワークフロー

| やること | 使うもの |
|---------|---------|
| 新機能の仕様作成 | `create-spec` スキル → `docs/spec/` |
| 実装計画の作成 | `create-plan` スキル → `docs/plan/` |
| 実装前の影響範囲分析 | `context-map` スキル |
| アーキテクチャ設計 | `architect` エージェント → `docs/design/` |
| コードレビュー | `code-review` スキル |
| SQLレビュー | `sql-review` スキル |
| リファクタリング | `refactor` スキル |
| Gitコミット | `git-commit` スキル |
| ドキュメント作成 | `doc-writer` スキル |
| デバッグ | `debug` エージェント |
| アプローチ検証 | `critical-thinking` エージェント |

## 参考にしたプロジェクト内リソース

| リソース | 参考元 | 改善ポイント |
|---------|--------|-------------|
| 仕様書テンプレート | `skills/create-specification/` | 日本語化、フォーマット簡素化 |
| 実装計画テンプレート | `skills/create-implementation-plan/` | 日本語化、ステータス管理追加 |
| コンテキストマップ | `skills/context-map/` | 日本語化、リスク評価項目追加 |
| コードレビュー | `instructions/code-review-generic` | 日本語化、優先度体系整理 |
| セキュリティ | `instructions/security-and-owasp` | 日本語化、実用的に圧縮 |
| SQLレビュー | `skills/sql-code-review/` | 日本語化 |
| リファクタリング | `skills/refactor/` | 日本語化、判断基準追加 |
| Gitコミット | `skills/git-commit/` | 日本語化、セーフティプロトコル |
| ドキュメント作成 | `skills/documentation-writer/` | 日本語化、Diátaxis対応 |
| デバッグ | `agents/debug.agent.md` | 日本語化 |
| アーキテクト | `agents/arch.agent.md` | 日本語化、出力先統一 |
| クリティカルシンキング | `agents/critical-thinking.agent.md` | 日本語化 |
| memory-bank | `instructions/memory-bank` | 日本語化、テンプレート付与 |
| ドキュメント同期 | `instructions/update-docs-on-code-change` | 日本語化、DB対応追加 |
| ワークフロー | `instructions/spec-driven-workflow-v1` | copilot-instructions.md に統合 |
