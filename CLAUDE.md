# CLAUDE.md

このファイルは、このリポジトリでコードを作業する際のClaude Code (claude.ai/code) のガイダンスを提供します。

## コマンド

### テスト実行
```bash
# 全テストを実行
./vendor/bin/phpunit
# または
composer test
```

### インストールとセットアップ
```bash
# Laravel-adminをプロジェクトにインストール
php artisan vendor:publish --provider="Encore\Admin\AdminServiceProvider"
php artisan admin:install
```

### Artisanコマンド
```bash
# Adminコントローラー生成
php artisan admin:make UserController --model=App\\User

# Adminメニュー生成
php artisan admin:menu

# Admin権限管理
php artisan admin:permission

# ユーザー作成
php artisan admin:create-user

# パスワードリセット
php artisan admin:reset-password

# 拡張機能作成
php artisan admin:extend <extension-name>
```

## アーキテクチャ概要

### コアコンポーネント
- **Grid**: データグリッド表示システム（`src/Grid.php`、`src/Grid/`ディレクトリ）
- **Form**: 管理画面フォームシステム（`src/Form.php`、`src/Form/`ディレクトリ）
- **Show**: データ詳細表示システム（`src/Show.php`、`src/Show/`ディレクトリ）
- **Tree**: 階層構造データ管理（`src/Tree.php`、`src/Tree/`ディレクトリ）

### サービスプロバイダー
- `AdminServiceProvider`: 全てのAdmin機能を登録・設定（`src/AdminServiceProvider.php`）
- 認証、権限、ルーティング、ミドルウェアを管理

### 認証・権限システム
- データベース認証: `src/Auth/Database/`ディレクトリ
- Permission trait: `src/Auth/Database/HasPermissions.php`
- ミドルウェア: `src/Middleware/`ディレクトリ

### コントローラー
- ベースコントローラー: `src/Controllers/AdminController.php`
- 認証: `src/Controllers/AuthController.php`
- CRUD操作: `src/Controllers/HasResourceActions.php`を利用

### フィールドとディスプレイ
- フォームフィールド: `src/Form/Field/`ディレクトリ（40+のフィールドタイプ）
- グリッドディスプレイ: `src/Grid/Displayers/`ディレクトリ（表示形式）
- フィルター: `src/Grid/Filter/`ディレクトリ（データフィルタリング）

### アセット管理
- フロントエンドアセット: `resources/assets/`ディレクトリ
- AdminLTE、Bootstrap、各種jQuery プラグインを含む
- CSS/JSバンドル: `src/Traits/HasAssets.php`で管理

### レイアウトシステム
- レイアウト: `src/Layout/`ディレクトリ（Content、Row、Column）
- ウィジェット: `src/Widgets/`ディレクトリ（再利用可能なUI要素）
- Bladeテンプレート: `resources/views/`ディレクトリ

### 拡張性
- Extension system: `src/Extension.php`
- Console commands: `src/Console/`ディレクトリ（コード生成）
- カスタマイズ用のstubファイル: `src/Console/stubs/`ディレクトリ

### 設定
- 設定ファイル: `config/admin.php`
- 多言語対応: `resources/lang/`ディレクトリ（15言語）
- データベースマイグレーション: `database/migrations/`ディレクトリ

## 重要な規約

### ファイル命名
- コントローラー: `src/Controllers/`に配置、`AdminController`を継承
- フォームフィールド: `src/Form/Field/`に配置、`Field`クラスを継承
- グリッドディスプレイ: `src/Grid/Displayers/`に配置、`AbstractDisplayer`を継承

### コーディングスタイル
- PSR-4オートロード: `Encore\Admin`名前空間
- Laravelのコーディング規約に準拠
- Eloquent Relationshipパターンを利用

### テスト構造
- テストケース: `tests/`ディレクトリ
- ベースクラス: `tests/TestCase.php`
- モデル: `tests/models/`、コントローラー: `tests/controllers/`

---

## Laravel 11アップグレード作業進行状況

### 📋 アップグレード計画全体
- **目標**: Laravel-adminをLaravel 11/12対応にアップグレード
- **計画書**: `LARAVEL11_UPGRADE_PLAN.md`
- **作業ログ**: `UPGRADE_WORK_LOG.md`

### ✅ 完了済みフェーズ

#### フェーズ1: 基盤整備（完了）
**1-1 依存関係更新**
- composer.json更新（PHP 8.2+、Laravel 11、Symfony 7対応）
- 18個追加、42個更新、9個削除
- セキュリティ脆弱性なし

**1-2 実際のアップデート実行**
- composer update成功（Laravel 11.45.1）
- TestCaseをOrchestra Testbenchに移行
- 基本的な互換性問題を修正

**1-3 テストフレームワーク完全移行**
- Model Factories現代化（Laravel 11構文）
- BrowserKitTesting → HTTPテスト完全移行
- AuthTest 4/4テストパス

#### フェーズ2-1: データベース層現代化（完了）
**主要な更新**:
- マイグレーション現代化: `increments()` → `id()` 11箇所
- 外部キー制約強化: 7つの関係テーブル
- SQLite完全対応（Laravel 11デフォルトDB）
- Eloquentモデル5つにHasFactory追加
- テーブル削除順序最適化

**更新ファイル**:
- `database/migrations/2016_01_04_173148_create_admin_tables.php`
- `tests/migrations/2016_11_22_093148_create_test_tables.php`
- `src/Auth/Database/*.php`（5モデル）

### 🚧 次回作業予定

#### フェーズ2-2: 認証・権限システム統合
**作業内容**:
1. Laravel 11認証との統合確認
2. 権限システムの強化
3. ユーザー管理の改善

#### フェーズ2-3: ルーティング・ミドルウェア見直し
**作業内容**:
1. 新しいルート構造への対応
2. ミドルウェアグループの更新
3. ServiceProviderの最適化

### 🔧 作業方法論

#### TodoWrite活用
```bash
# 作業項目をTodoとして管理
# 各フェーズの開始時にTodoWriteでタスク作成
# 進捗に応じてstatus更新（pending → in_progress → completed）
```

#### 作業ログ管理
```bash
# 全作業をUPGRADE_WORK_LOG.mdに詳細記録
# フェーズごとに開始時刻、作業項目、進捗、結果を記録
# 問題と解決策を文書化
```

#### 段階的アプローチ
1. **分析**: 現状確認とバックアップ作成
2. **更新**: 段階的な修正実施
3. **テスト**: 各段階でのテスト実行
4. **記録**: 作業ログとサマリー更新

### 🎯 現在の状態
- **Laravel**: 11.45.1（最新）
- **PHP**: 8.2+対応
- **テストフレームワーク**: Orchestra Testbench
- **データベース**: SQLite対応（Laravel 11デフォルト）
- **テスト成功率**: AuthTest 4/4パス

### 📚 重要な学習ポイント
1. **BrowserKitTesting廃止**: HTTPテストへの移行必須
2. **Model Factories構文変更**: HasFactory traitとdatabase/factories/
3. **マイグレーション構文更新**: increments() → id()、foreignId()活用
4. **外部キー制約**: SQLiteでの適切な制約設定重要

### 🔄 次回セッション開始時の手順
1. `UPGRADE_WORK_LOG.md`で現在の進捗確認
2. `LARAVEL11_UPGRADE_PLAN.md`でフェーズ2-2の詳細確認
3. TodoWriteで新しいタスクセット作成
4. フェーズ2-2（認証・権限システム）開始