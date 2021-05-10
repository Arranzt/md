# Laravelまとめ

## Laravelバージョン確認

```
$ php artisan --version
$ php artisan -v
```

***

## Laravel makeコマンド集
※作成しただけで中身のないファイルをスケルトンという

```
$ php artisan make:作りたい種類 ファイル名 --オプション名
```

## コントローラの作成
```
$ php artisan make:controller コントローラー名

※リソースコントローラーの作成
$ php artisan make:controller コントローラー名 --resource
```

## モデルの作成
```
$ php artisan make:model モデル名
```

## マイグレーションファイルの作成
```
$ php artisan make:migration マイグレーション名


$ php artisan make:migration create_users_table

--create=テーブル名というオプションを追加すると、テーブル名を含んだ雛形のマイグレーションファイルが作成される。
$ php artisan make:migration create_users_table --create=users
```

## リクエストの作成
```
$ php artisan make:request リクエスト名
```

## シーダーの作成
```
$ php artisan make:seeder シーダー名
```

## サービスプロバイダの作成
```
$ php artisan make:provider サービスプロバイダ名
```

## ミドルウェアの作成
```
$ php artisan make:middleware ミドルウェア名
```

## 認証のスキャフォールディングの作成
```
$ php artisan make:auth
```

***

## Laravelマイグレーション集

## マイグレーションの実行
```
$ php artisan migrate
```

## マイグレーションファイルの実行状況の確認
```
$ php artisan migrate:status
```

## マイグレーションを１つ前に戻す
```
$ php artisan migrate:rollback

戻す数を指定したい場合、--step=戻したい数というオプションを追加する
$ php artisan migrate:rollback --step=1
```

## テーブルのリフレッシュ
すべてリセットした後にマイグレーションを実行するコマンド
```
$ php artisan migrate:refresh
```

## テーブルのリセット
```
$ php artisan migrate:reset
```

***

## キャッシュクリア集

## キャッシュをクリアする
```
$ php artisan cache:clear
```

## 設定キャッシュをクリアする
```
$ php artisan config:clear
```

## ルーティングのキャッシュをクリアする
```
$ php artisan route:clear
```

## Viewのキャッシュをクリアする
```
$ php artisan view:clear
```

***
## メンテナンスモード

## 通常モードからメンテナンスモードへの移行
```
$ php artisan down
```

## メンテナンスモードから通常モードへの移行
```
$ php artisan up
```

***
## シーダーの実行
```
$ php artisan db:seed

オプションで--classを使うと、特定のシーダーだけ実行する
$ php artisan db:seed --class=UsersTableSeeder
```

***

## ルーティングのキャッシュ
```
$ php artisan route:cache
```

***

## アプリケーションキーの設定
```
$ php artisan key:generate
```

***

## PHPビルトインサーバの起動
```
$ php artisan serve
```

***

## ルートの一覧確認
```
$ php artisan route:list
```

***
