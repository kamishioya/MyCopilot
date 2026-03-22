---
name: git-commit
description: 'Conventional Commits仕様に基づくGitコミットメッセージの分析・生成・実行。差分からタイプとスコープを自動検出し、標準化されたコミットメッセージを作成する。'
---

# Gitコミットスキル

## 概要

Conventional Commits仕様に基づき、標準化されたセマンティックなGitコミットを作成します。
実際の差分を分析して適切なタイプ、スコープ、メッセージを決定します。

## コミットメッセージ形式

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

## コミットタイプ

| タイプ | 目的 |
|--------|------|
| `feat` | 新機能 |
| `fix` | バグ修正 |
| `docs` | ドキュメントのみの変更 |
| `style` | フォーマット/スタイル（ロジック変更なし）|
| `refactor` | リファクタリング（機能追加/バグ修正なし）|
| `perf` | パフォーマンス改善 |
| `test` | テストの追加/修正 |
| `build` | ビルドシステム/依存関係 |
| `ci` | CI/CD設定の変更 |
| `chore` | その他の雑務 |
| `revert` | コミットの取り消し |

## 破壊的変更

```
# タイプ/スコープ後にエクスクラメーションマーク
feat!: 非推奨エンドポイントを削除

# BREAKING CHANGE フッター
feat: 設定を他の設定から拡張可能にする

BREAKING CHANGE: `extends` キーの動作が変更されました
```

## ワークフロー

### 1. 差分を分析
```bash
# ステージされたファイルがあればステージ差分を使用
git diff --staged
# なければワーキングツリー差分
git diff
# ステータス確認
git status --porcelain
```

### 2. ファイルをステージ（必要に応じて）
```bash
git add path/to/file1 path/to/file2
```

**秘密情報をコミットしないこと**（.env、credentials、秘密鍵）

### 3. コミットメッセージを生成
差分を分析して以下を決定:
- **タイプ**: どの種類の変更か？
- **スコープ**: どの領域/モジュールが影響を受けるか？
- **説明**: 変更のサマリー（現在形、命令形、72文字以内）

### 4. コミット実行
```bash
git commit -m "<type>[scope]: <description>"
```

## ベストプラクティス

- 1コミット = 1つの論理的な変更
- 現在形: 「追加した」ではなく「追加する」
- 命令形: 「修正」
- Issue参照: `Closes #123`、`Refs #456`
- 説明は72文字以内

## Gitセーフティプロトコル

- git configを変更しない
- 破壊的コマンド（--force、hard reset）はユーザーの明示的な要求なしに実行しない
- --no-verify でフックをスキップしない（ユーザーが要求した場合を除く）
- main/masterにforce pushしない
