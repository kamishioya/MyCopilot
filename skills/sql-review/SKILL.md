---
name: sql-review
description: 'SQLコードのセキュリティ、パフォーマンス、保守性を包括的にレビューする。MySQL、PostgreSQL、SQL Server、Oracle対応。'
---

# SQLレビュースキル

対象のSQLコード ${selection}（未選択の場合はプロジェクト全体）に対して、セキュリティ、パフォーマンス、保守性の観点から包括的なレビューを実施してください。

## 🔒 セキュリティ分析

### SQLインジェクション防止
```sql
-- ❌ 危険: SQLインジェクション脆弱性
query = "SELECT * FROM users WHERE id = " + userInput;

-- ✅ 安全: パラメータ化クエリ
-- PostgreSQL/MySQL
PREPARE stmt FROM 'SELECT * FROM users WHERE id = ?';
EXECUTE stmt USING @user_id;

-- SQL Server
EXEC sp_executesql N'SELECT * FROM users WHERE id = @id', N'@id INT', @id = @user_id;
```

### アクセス制御と権限
- **最小権限の原則**: 必要最小限の権限のみを付与
- **ロールベースアクセス**: ユーザー直接権限ではなくDBロールを使用
- **スキーマセキュリティ**: 適切なスキーマ所有権とアクセス制御

### データ保護
- **機密データ露出**: 機密カラムを含むテーブルで SELECT * を避ける
- **監査ログ**: 機密操作のログが取られているか確認
- **暗号化**: 機密データが暗号化保存されているか確認

## ⚡ パフォーマンス最適化

### クエリ構造分析
```sql
-- ❌ 非効率なクエリパターン
SELECT DISTINCT u.*
FROM users u, orders o, products p
WHERE u.id = o.user_id
AND o.product_id = p.id
AND YEAR(o.order_date) = 2024;

-- ✅ 最適化された構造
SELECT u.id, u.name, u.email
FROM users u
INNER JOIN orders o ON u.id = o.user_id
WHERE o.order_date >= '2024-01-01'
AND o.order_date < '2025-01-01';
```

### インデックス戦略
- **不足インデックス**: インデックスが必要なカラムの特定
- **過剰インデックス**: 未使用または冗長なインデックスの発見
- **複合インデックス**: 複雑なクエリ向けの複数カラムインデックス

### JOIN最適化
- **JOIN種類**: 適切なJOIN種類の確認（INNER vs LEFT vs EXISTS）
- **JOIN順序**: 小さい結果セットを先にする最適化
- **デカルト積**: JOIN条件の欠落の特定と修正

## 🛠️ コード品質と保守性

### SQLスタイルとフォーマット
```sql
-- ❌ 悪い例: 可読性の低いフォーマット
select u.id,u.name,o.total from users u left join orders o on u.id=o.user_id where u.status='active';

-- ✅ 良い例: 読みやすいフォーマット
SELECT u.id,
       u.name,
       o.total
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
WHERE u.status = 'active'
  AND o.order_date >= '2024-01-01';
```

### アンチパターン検出
- SELECT * の使用（本番コード）
- 未使用のカラムの取得
- WHERE句でのカラムに対する関数適用（インデックス無効化）
- 不要なサブクエリ（JOINやウィンドウ関数で代替可能）
- 暗黙的な型変換
