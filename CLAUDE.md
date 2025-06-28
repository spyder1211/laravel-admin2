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