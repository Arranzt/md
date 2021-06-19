# Laravel大全

## Laravel総機能
```
DB系
マイグレーション
DBファサード
クエリビルダ
リポジトリパターン
Eloquent
リレーション

FW機能
ライフサイクル
認証
キャッシュ
エラーハンドリング
ロギング
イベント
ローカリゼーション
メール
ページネーション
セッション
サービスコンテナ
サービスプロバイダ
コントラクト
ファサード

テスト系
ユニットテスト
モック
Faker
ファンクショナルテスト
テスト駆動開発
```

## Laravel　プロジェクトの作成
①git cloneでソースを落とす
–prefer-source
デフォルト(オプション指定なしではこっち)
```
composer create-project laravel/laravel daily_report_spike

やっていること
git clone　git@github.com:laravel/laravel.git
mv laravel app_dir//多分。
cd app_dir
composer install
```
②zipファイルでソースを落とす
–prefer-dist
こっちの方が高速に動く
```
composer create-project laravel/laravel daily_report_spike –prefer-dist

やっていること
wget https://github.com/laravel/laravel/archive/master.zip
unzip laravel
mv laravel app_dir　//多分。
cd app_dir
composer install
```

## Laravelバージョン確認

```
$ php artisan --version
$ php artisan -v
```

***

## Laravel makeコマンドの原形
※作成しただけで中身のないファイルを<span style="color:blue">スケルトン</span>という

```
$ php artisan make:作りたい種類 ファイル名 --オプション名
```

この形を元に、各コマンドが作成されている。

## コントローラの作成
コントローラの役割は2つ。  
①ルートから受け取った情報をモデルに処理をお願いすること  
②モデルから受け取った情報をビューに表示すること
→モデルからDBにアクセスし、取得したデータをコントローラで加工してビューに渡すと行ったことや、ビューからのデータをモデルに渡し、DBに保存をしたりします
```
$ php artisan make:controller コントローラー名

※リソースコントローラーの作成
$ php artisan make:controller コントローラー名 --resource
```

## モデルの作成
モデルとは、データベースとの連携を担うMVCのMの部分。  
具体的には、Eloquentとビジネスロジックをもったクラスのこと。  
1テーブル1モデルが基本。
```
$ php artisan make:model モデル名
```
※Eloquentについて  
DBのデータを操作する機能のこと。

※モデルの命名規則  
テーブル名がusersの場合、Model名はUser。テーブル名を単数系にしたものがModel名になる。

## マイグレーションファイルの作成
マイグレーションとは、データベース版バージョン管理ツール
```
$ php artisan make:migration マイグレーション名


$ php artisan make:migration create_users_table

--create=テーブル名というオプションを追加すると、テーブル名を含んだ雛形のマイグレーションファイルが作成される。
$ php artisan make:migration create_users_table --create=users
```
※マイグレーションファイルとは、どんなテーブルを作るかというテーブルの設計書のこと

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

## Laravel基礎編

## Laravelの初期設定
### タイムゾーン  
```
config/app.php  
'timezone' => 'UTC'→'timezone' => 'Asia/Tokyo',  
```

### 言語設定  
```
config/app.php  
'locale' => 'en'→'locale' => 'ja'  
```

### データベースの文字コード  
```
config/database.php
'charset' => 'utf8mb4'→'charset' => 'utf8'
```
utf8mb4は絵文字も使える文字コード

### デバックバー  
```
composer require barryvdh/laravel-debugbar
```
.envのdebugbarでon/offを切り替えられる
```
.env  
APP_DEBUG=true
APP_DEBUG=false
```
Laravelのキャッシュ保存機能により、APP_DEBUGがうまく働かない場合、キャッシュクリアする
```
php artisan cache:clear
php artisan config:clear
```

### データベース設定  
.env
```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306  

データベースの名前
DB_DATABASE=pilot_database 

DB_USERNAME=root
DB_PASSWORD=3q5fgbnX7
``` 

接続の確認
```
php artisan migrate
```
Laravelでデフォルトで用意されているテーブルが３つあり、それをデータベース内に作成してくれる  
なんで、とりあえずデフォルトのテーブル作ってデータベースと接続できるかを確認する  
create_users_table  
create_password_resets_table  
create_failed_jobs_table  

### エラーメッセージの日本語化 
resources\js\lang\validation.php  
がメッセージの倉庫  
githubからlaravel-resources-lang-jaライブラリを付け加えるとOK

***

## Laravelの役割分担
Model　データベースとやり取り  
View　みため  
Controller　処理  
Routing　アクセスの振り分け  
Migration　DBテーブルの履歴管理  

```
router/web.php
Route::get('/', function () {
    return "hello";
});

resources/views/welcomeが初期ページ
```

artisanコマンドのリスト
php artisan list

***

## モデル
DBとのやり取りをphpで書けるというメリットがある

## Eloquent
ORM/ORマッパー  
Object-Relational Mapping  
オブジェクト関係マッピング
```
php artisan make:model Test
```
→app/Testという風に、appファイルの下にできるが直下にできると見辛い  
そこで、Modelsというディレクトリを作って、その下に配置することが多い
```
php artisan make:model Models/Test
```
-mcオプション  
マイグレーションファイルとコントローラファイルをまとめて作る時に利用する
```
php artisan make:model Models/Test -mc
```
## マイグレーション
DBのテーブルの履歴を管理する仕組み
```
php artisan make:migration create_tests_table
```
でマイグレーションファイルを作成し、
```
php artisan migrate
```
で設定した情報にてデータベースを改修する
```
php artisan migrate:fresh
php artisan migrate:refresh
```

モデルは単数形、マイグレーションは複数形

## マイグレーションファイル
https://readouble.com/laravel/5.7/ja/migrations.html  
カラム作成の項目で、カラムの設定を確認できる

```
app/database/migrations

public function up()
    {
        Schema::create('users', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->string('email')->unique();
            $table->timestamp('email_verified_at')->nullable();
            $table->string('password');
            $table->rememberToken();
            $table->timestamps();
        });
    }
```
## tinker
DB簡易接続するための仕組み
```
php artisan tinker
```
まずインスタンス化
```
$test = new App\Models\Test;
```
カラムに値を打ち込む
```
$test->text = "aaa";
```
保存
```
$test->save();
```
カラムの値を全表示
```
App\Models\Test::all();
```
コントローラー
```
app\Http\Controllers

php artisan make:controller TestController
```
MVCモデルの記述方法
```
Route::get('tests/test', TestController@index);

public function index(){
    return view('tests/test');
}

views/tests/test
```

モデルのデータを持ってくる
```
use App\Models\Test
```
```
$values = Test::all();
```
をコントローラ側に置く

dd($values);

viewに渡す
```
compact('values')

view側
@foreach($values as $value){
{{ $value->id }}
{{ $value->text }}
}
```

## ヘルパ関数
Laravelがデフォルトで用意してくれている便利な関数群のこと  

特によく使うもの  
route  
auth  
app  
bcrypt  
collect  
dd  
env  
factory  
old  
view  

https://readouble.com/laravel/5.7/ja/helpers.html

## コレクション型
```
\Collection
```
Laravel独自で定義されている配列を拡張した型  
https://readouble.com/laravel/5.7/ja/collections.html

コレクション型は自力で作ることもできる  
その場合、collectという関数を利用する
```
$collection = collect([1, 2, 3]);
```

データベースからデータを取得した生の状態では、コレクション型になっている  
コレクション型には専用の関数が多数用意されており、メソッドチェーンで記述が可能となっている  
all  
chunk  
get  
pluck  
whereln  
toArray

## クエリビルダ
データベースとアクセスする場合、本来ならSQLを書く必要があるが、それをphpの構文で書けるようにしたもの  
select  
where  
group by  
などSQLに近い構文となっている  
勝手にSQLインジェクション対策もしてくれる  

使い方  
DBファサードのtableメソッドを使う
```
use Illuminate\Support\Facades\DB;

DB::table('テーブル名')->
https://readouble.com/laravel/5.7/ja/queries.html

ex
DB::table('users')->get();
```

### get()
値を取得する

### select()
特定のカラムを指定する
```
DB::table('users')->select('id')->get();
```

### raw
生のSQLを使うことができる
```
DB::raw
```

複雑化して来たらクエリビルダの方が細かい条件を指定でき、それでもダメならSQLを直接指定するのが良い

### Facade
正面入り口のこと  
元々はデザインパターンで使われていた用語  
ミドルウェアのように利用される

https://shimooka.hateblo.jp/entry/20141215/1418620292

簡単にアクセスする場所は１つにしておいて、中にいろんなシステムがある  
使う側は入り口だけ知っておけばOKというような形の作り方  
デパートの入り口  
このデパート自体も結構たくさん用意されている  

https://readouble.com/laravel/5.7/ja/facades.html
```
config\app.php
```
providers  
aliases  
の箇所がfacadesの設定箇所

## Laravel起動処理
## DIとサービスコンテナについて
DI　外部でインスタンス化して、それを注入する方法。インスタンス化されたものが渡ってくるので、改めてインスタンス化しなくてもメソッドが使える状態になっている  
（普通はインスタンスはクラスの内部で生成される）  
サービスコンテナ　DIをまとめて管理してくれる機能

https://qiita.com/namizatork/items/801da1d03dc322fad70c
```
public\index.php
```
が一番最初にアクセスされているファイルになっている

## Blade
Laravelに組み込まれているテンプレートエンジン  
.blade.phpが拡張子  

{{  }}でエスケープ処理して表示を行う  
htmlspecialcharsを行ってくれる  

@csrfでCSRF対策ができる  
formタグをつくる時はこれをつけないとエラーがでるため、必須となる  

@foreach  
@endforeach  
で配列を表示可能  

@section  
@yield  
などでテンプレートを読み込む  
https://readouble.com/laravel/5.7/ja/blade.html


フロントエンドの大規模化  
webpackやbabel必須
```
laravel-ui
```
Laravel6.Xから登場
```
laravel-mix
```
webpackのラッパー  
webpack.mix.jsで定義

```
webpack.mix.js
```
laravel-mixの設定ファイル  

Node.js/npm　別途インストール  
package.json/package.lock　設定管理ファイル  

package.jsonの
```
"devDependencies": {
        "axios": "^0.19",
        "cross-env": "^7.0",
        "laravel-mix": "^5.0.1",
        "lodash": "^4.17.19",
        "resolve-url-loader": "^3.1.0",
        "sass": "^1.15.2",
        "sass-loader": "^8.0.0"
    }
```
が、フロントエンド側でインストールされている拡張ツール群を表している  

Laravel-ui  
https://readouble.com/laravel/5.7/ja/frontend.html

## スカフォールド
足場


### ルーティングの設定確認
```
php artisan route:list
```
→みにくい
```
php artisan route:list > route.txt
```
とすると、ファイルが生成されるので見やすくなる

adminログインの作成  
マルチログイン機能  
coinbaby8.comのサイトへ

### 作る順番
model controller route view  
が良い

### modelの作成

①php artisan make:model Models/ContactForm -m  

②php artisan make:migration add_title_to_contact_forms_table --table=contact_forms  
で、すでに作成したテーブルに追加の情報を与えることができる  

最初のファイルは
```
public function up()
    {
        Schema::create('contact_forms', function (Blueprint $table) {
            $table->bigIncrements('id');
            $table->string('your_name', 20);
            $table->string('email', 255);
            $table->longText('url')->nullable($value = true);
            $table->boolean('gender');
            $table->tinyInteger('age');
            $table->string('contact', 200);
            $table->timestamps();
        });
    }
```
となるが、追加のファイルは
```
public function up()
    {
        Schema::table('contact_forms', function (Blueprint $table) {
            //
        });
    }
```
と、Schemaのあとがcreateとtableで変わるので注意すること  

your_nameというカラムの次にtitleというカラムを追加したい場合
```
$table->string('title', 50)->after('your_name');
```

③php artisan migrate

```
php artisan migrate:status
```
でこれまでのマイグレーションを確認できる

１個前に戻す時
```
php artisan migrate:rollback
```

--stepをつけると、いくつ戻すか？という指定もできる  
--step=2

### Controllerの作成
Restを使って  

https://readouble.com/laravel/5.7/ja/controllers.html  

リソースコントローラ  
--resourceで作成できる
```
php artisan make:contoller コントローラ名 --resource
```

### ルーティングの設定
```
Route::resource('contacts', 'ContactFormController');
```
->only(['index', 'show']);をつけると、メソッドを限定することも可能  

Route::groupにて認証を設定可能  
prefixというものをつけると、フォルダを指定することができる  
また、middlewareのauthとすることで、認証されていたら表示するのようになる
```
Route::get('contact/index', 'ContactFormController@index');
```
は
```
Route::group(['prefix' => 'contact', 'middleware' => 'auth'], function(){
    Route::get('index', 'ContactFormController@index');
});
```
こんな風にcontact/indexをindexに短縮できる  

また、nameというものをつけると、コントローラーに名前をつけられる
```
Route::get('index', 'ContactFormController@index')->name('contact.index');
```
名前付きルートを定義した場合、次回からはグローバルなroute関数を使用することで、URLを生成したり、リダイレクトしたりできる
```
URLを作りたい場合
$url = route('contact.index');

リダイレクトを作りたい場合
return redirect()->route('contact.index')
```


※ルータはHTTP動詞に対応してルートを定義できるようになっている
```
Route::get(URI, クロージャ);
Route::post(URI, クロージャ);
Route::put(URI, クロージャ);
Route::patch(URI, クロージャ);
Route::delete(URI, クロージャ);
Route::options(URI, クロージャ);
```
複数のHTTP動詞に対応したルートの登録は、matchかanyを利用する
```
Route::match(['get', 'post'], URI, クロージャ);
Route::any(URI, クロージャ);
```
リダイレクトするルートを定義する場合  
Route::redirectメソッドを使用する
```
Route::redirect('今', 'リダイレクト先', 'ステータスコードのカスタマイズ（指定なしだと302がデフォルトで返される）');
```

言語設定を変更する場合
```
str_replace('_', '-', app()->getLocale())
```
フォームを作る場合  
CSRFを入れてないとフォームが動かないので、Laravelでは以下のものはマストで記載が必要
```
<meta name="csrf-token" content="{{ csrf_token() }}">
```

```
<form>
@csrf
</form>
```

configはヘルパ関数  
configファイルの設定を持ってくる
```
config('app.name', 'Laravel')
```

assetはwebpack.mixの記述から来ている  

@guestは認証ディレクティブと呼ばれる  
@authと対で使う  

@auth  
認証済のユーザに表示  
@endauth  

@guest  
認証されていないユーザに表示  
@endguest  

Auth::user()->name  
でログインしているユーザーの名前を持ってこれる  

@section  
区切り  
@endsection


### 認証機能（ログイン/ログアウト）の作成
https://readouble.com/laravel/7.x/ja/frontend.html  
スカフォールドの記事参照
```
composer require laravel/ui
```
でまず土台を作り、
```
php artisan ui bootstrap --auth
php artisan ui vue --auth
php artisan ui react --auth
```
からどれか１つを選択し、フロント側をインストールすると完了  
web.phpに
```
Auth::routes();
```
が追加されると、認証が動いた状態になる  

register  
name a  
mail a@gmail.com  
pass aaaaaaaa  
confirm aaaaaaaa  

```
{{ __('Login') }}
```
laravel.comに記載がある


## 新規登録 Createの作成

view側で別ファイルへのアンカー設定  
いつもだったら
```
<a href="ファイルへのアドレス">
```
になるが、bladeの場合は
```
<a href="{{ route('contact.create') }}">新規登録</a>
```
になる  
route('contact.create')という書き方で、別ファイルへのパスを示すことができる  

もしくは
```
<form method="GET" action="{{ route('contact.create') }}">
    <button type="submit" class="btn btn-primary">
        新規登録
    </button>
</form>
```
こんな感じ
formファサードもアリ

```
@extends('layouts.app')

@section('content')
<div class="container">
    <div class="row justify-content-center">
        <div class="col-md-8">
            <div class="card">
                <div class="card-header">{{ __('Dashboard') }}</div>

                <div class="card-body">
                    @if (session('status'))
                        <div class="alert alert-success" role="alert">
                            {{ session('status') }}
                        </div>
                    @endif
                    
                    <form action="" method="POST">
                        氏名
                        <input type="text" name="your_name">
                        <br>
                        件名
                        <input type="text" name="title">
                        <br>
                        メールアドレス
                        <input type="email" name="email">
                        <br>
                        ホームページ
                        <input type="url" name="url">
                        <br>
                        性別
                        <input type="radio" name="gender" value="0">男性
                        <input type="radio" name="gender" value="1">女性
                        <br>
                        <select name="age" id="">
                            <option value="">選択して下さい</option>
                            <option value="1">~19歳</option>
                            <option value="2">20歳~29歳</option>
                            <option value="3">30歳~39歳</option>
                            <option value="4">40歳~49歳</option>
                            <option value="5">50歳~59歳</option>
                            <option value="6">60歳~</option>
                        </select>
                        <br>
                        お問い合わせ内容
                        <textarea name="contact"></textarea>
                        <br>

                        <input type="checkbox" name="caution" value="1">注意事項
                        <br>

                        <input type="submit" class="btn btn-info" value="登録する">
                    </form>
                </div>
            </div>
        </div>
    </div>
</div>
@endsection
```

## 保存　store

一般的なphpでは、フォームから送信されてきたデータに関しては、
```
$_POST[""];
```
で受け取っていたはず  

ただ、Laravelの場合はこれがRequestクラスというものを使って処理されるため注意が必要

```
use Illuminate\Http\Request;

public function store(Request $request)
{
    
}
```
こう書くことで、RequestクラスがDIされた結果、$requestにRequestクラスのインスタンスが注入される  
https://readouble.com/laravel/5.7/ja/requests.html  

Illuminate\Http\Requestは  
vendor/laravel/framework/src/Http/Request.php  
に存在している  

Requestの内容を保存してくれるのはinputというメソッドであるが、これはtraitの  
Concerns\InteractsWithInput  
に定義されており、次の内容となっている

```
public function input($key = null, $default = null)
    {
        return data_get(
            $this->getInputSource()->all() + $this->query->all(), $key, $default
        );
    }
```

それでは実際の保存処理
```
$name = $request->input('your_name');
```
とすることで、フォームに登録された名前をとってくることができる  

※全てのデータをとってくるなら
```
$name = $request->all();
```
だが、今回はデータベースに１つずつ値を保存させて行きたいので、これは使わない

```
Route::post('store', 'ContactFormController@store')->name('contact.store');

public function store(Request $request)
    {
        $your_name = $request->input('your_name');
        $title = $request->input('title');
        $email = $request->input('email');
        $url = $request->input('url');
        $gender = $request->input('gender');
        $age = $request->input('age');
        $contact = $request->input('contact');

        dd($request);
    }

 <form action="{{ route('contact.store') }}" method="POST">
```

引き続きDBへの保存を行っていく  
一般的なphpであれば、PDOを利用したり、プリペアードステイトメントを利用したり、バインドを使って処理を
行うのだが、Laravelはそのへんは自動で処理してくれるので、改めて書く必要はないところが便利だったりする  

まずはモデルファイルをコントローラ側で呼び出す
```
use App\Models\ContactForm;
```

次に、コントローラ側でContactFormクラスを使えるように、インスタンス化してやる
```
$contact = new ConatactForm;
```

そして、次のように書き換える  
保存はsaveメソッドを使う
```
public function store(Request $request)
    {
        $contact = new ContactForm;

        $contact->your_name = $request->input('your_name');
        $contact->title = $request->input('title');
        $contact->email = $request->input('email');
        $contact->url = $request->input('url');
        $contact->gender = $request->input('gender');
        $contact->age = $request->input('age');
        $contact->contact = $request->input('contact');

        $contact->save();

        return redirect('contact/index');
    }
```

## DBに保存したデータの取り出し

DBからデータを持ってくる場合、2種類の方法がある

①エロクアント
```
ContactForm::all();

$contact = ContactForm::all();
dd($contact);で中身確認可能
```
これだと、全部の情報を持ってこれるが、不要な情報まで持ってきてしまいパフォーマンスが下がる

②クエリビルダ
```
use Illuminate\Support\Facades\DB;
をつける

DB:table('テーブル名')->select('カラム名')->get();

例
DB::table('contact_forms')->select('id', 'your_name')->get();
```

Viewに渡す  
compact(DBから取得した値を格納した変数)

```
<table class="table">
    <thead>
        <tr>
        <th scope="col">id</th>
        <th scope="col">氏名</th>
        <th scope="col">件名</th>
        <th scope="col">登録日時</th>
        </tr>
    </thead>

    <tbody>
    @foreach($contacts as $contact)
        <tr>
        <th scope="row">{{ $contact->id }}</th>
        <td>{{ $contact->your_name }}</td>
        <td>{{ $contact->title }}</td>
        <td>{{ $contact->created_at }}</td>
        </tr>
    @endforeach
    </tbody>
</table>
```

orderbyで順番も変更可能

## show
コレクションは２箇所ある  
より深くしるのところのコレクションとeloquentのコレクション

①web.phpでルーティング設定
```
Route::get('show/{id}', 'ContactFormController@show')->name('contact.show');
```
②show.blade.php作成  
③controller書き換え
```
public function show($id)
{
    $contact = ContactForm::find($id);
    $contact->gender;
    return view('contact.show', compact('contact'));
}
```
④index.blade.php書き換え
```
<td><a href="{{ route('contact.show', ['id' => $contact->id]) }}">詳細をみる</a></td>
```

## edit

①web.phpでルーティング設定

## update

## destroy

***

## 日報管理

①DBの整備
```
php artisan migrate
でDBとの接続確認
↓

php artisan make:model Models/dairy_report -mc
これでモデル、コントローラー、マイグレーションファイルの３つをまとめて作成する
が、コントローラーはあとでリソースコントローラとして作成した方がいいので、-mオプションで作るのが良さそう

↓

public function up()
{
    Schema::create('dairy_reports', function (Blueprint $table) {
        $table->increments('id');
        $table->integer('user_id')->unsined();
        $table->string('title');
        $table->text('content');
        $table->date('reporting_time');
        $table->timestamps();
        $table->softDeletes();
    });
}

$table->increments('id');
符号なしINTを使用した自動増分ID（主キー）
勝手にプライマリーキーに設定してくれるし、自動増分もやってくれる

$table->string('name', 100);
桁数を指定する場合は、第二引数に指定
デフォルトは255文字

$table->softDeletes('deleted_at', 0);
ソフトデリートのためにNULL値可能で有効（全体）桁数指定のdeleted_at TIMESTAMPカラム追加
```

②コントローラーの作成
```
```

***

## イベント処理





***


##　エラー系
```
SQLSTATE[42000]: Syntax error or access violation: 1253 COLLATION 'utf8mb4_unicode_ci' is not valid for CHARACTER SET 'utf8' (SQL: select * from information_schema.tables where table_schema = daily_spike and table_name = migrations and table_type = 'BASE TABLE')

データベースの文字設定をいじると出るので元に戻すと直る
```

```
Trying to get property 'user' of non-object

ルーティングの設定で、show.blade.phpのルーティング設定の記述順序が早く、そちらが優先されていたのが原因
Route::get('question/mypage', 'QuestionController@mypage')->name('question.mypage');
Route::resource('question', 'QuestionController');
Route::post('question/comment', 'QuestionController@comment')->name('question.comment');
Route::post('question/confirm/{id?}', 'QuestionController@confirm')->name('question.confirm');
Route::get('question/ranking', 'QuestionController@ranking')->name('question.ranking');
という記述を
Route::get('question/mypage', 'QuestionController@mypage')->name('question.mypage');
Route::get('question/ranking', 'QuestionController@ranking')->name('question.ranking');
Route::resource('question', 'QuestionController');
Route::post('question/comment', 'QuestionController@comment')->name('question.comment');
Route::post('question/confirm/{id?}', 'QuestionController@confirm')->name('question.confirm');
に変更することで、question/rankingのルーティングがRoute::resource('question', 'QuestionController');よりも早く適用されるようになり、解決する
```

```
ERROR 1052 (23000): Column 'id' in field list is ambiguous
```

## DB系

## DDL
テーブルやデータベースの設定を操作するようなクエリを総称して<span style="color:blue">DDL(Data Definition Language)</span>と呼ぶ  

### データベースの確認
```
SHOW DATABASES;

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
```
データベースの作成は行っていないにも関わらず、information_schema、mysql、performance_schema、sysの4つのデータベースが存在しているが、これらはシステムで使用するテーブルなので、不用意にいじらない  

### データベースの作成
```
CREATE DATABASE データベース名

mysql> CREATE DATABASE db_lesson;
```

### データベースの使用
```
USE データベース名

mysql> USE db_lesson;
```

### 現在使用しているデータベースのテーブルを確認する
```
SHOW TABLES;

mysql> SHOW TABLES;
Empty set (0.00 sec)
```

### テーブルの作成  
テーブルを作るためには少なくとも1つ以上のカラムを用意する必要があるので注意  
CREATE TABLE テーブル名 (カラム名 型(桁数)　オプション, カラム名 型(桁数)　オプション...);  
また、カラム名はバッククオートで囲む必要があるので注意
```
CREATE TABLE テーブル名(
    `カラム名`　カラム型　オプション,
    `カラム名`　カラム型　オプション,
    …
);

mysql> CREATE TABLE `people`(
    -> `id` INT,
    -> `name` VARCHAR(255),
    -> `email` VARCHAR(255),
    -> `password` CHAR(8)
    -> );

mysql> CREATE TABLE people (
    -> person_id INT AUTO_INCREMENT PRIMARY KEY,
    -> name VARCHAR(20) NOT NULL,
    -> email VARCHAR(255) UNIQUE,
    -> age INT,
    -> gender TINYINT COMMENT '1が男、2が女',
    -> created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    -> updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
    -> );
```

### データ型
```
Type	Description
INT	「integer」の略で整数値に使用
BIGINT	INTよりも更に大きな整数値に使用
TINYINT	小さな整数値に使用
VARCHAR	長さが可変の文字列に使用
CHAR	予め長さが決まっている文字列に使用
TEXT, BLOB	文字列の長さがどれだけ長くなるか分からないものに使用
DECIMAL	正確な小数点を計算するための数字データに使用
FLOAT, DOUBLE	浮動小数点のデータに使用。厳密な計算に使用してはいけない
DATE	日付のデータに使用
DATETIME	日付と時間のデータに使用
TIMESTAMP	時間を基準日からの経過秒数で表現する
```

### オプション
```
Option	Description
NOT NULL	NULLが保存できないようにする
AUTO_INCREMENT	カラムの値を指定せずにレコード作成した場合、自動で一意の連続的な数値を生成する
PRIMARY KEY	主キー制約。値の重複を許可しない。また、NULLを入れることができない
UNIQUE	ユニークキー制約。値の重複を許可しない。NULLは挿入可能
DEFAULT	データの挿入時に値が指定されなかった場合に入れる初期値を指定することができる
UNSIGNED	整数型のカラムにつけることができるオプション。負の値が入れられなくなる
COMMENT	カラムに対してコメントを残す。SQLの実行には何の影響もないので、補足説明として使う
```

### テーブルの詳細を確認する
```
DESCRIBE テーブル名;
※DESCやSHOW COLUMNS FROM テーブル名でも同じ

mysql> DESCRIBE people;
+----------+--------------+------+-----+---------+-------+
| Field    | Type         | Null | Key | Default | Extra |
+----------+--------------+------+-----+---------+-------+
| id       | int(11)      | YES  |     | NULL    |       |
| name     | varchar(255) | YES  |     | NULL    |       |
| email    | varchar(255) | YES  |     | NULL    |       |
| password | char(8)      | YES  |     | NULL    |       |
+----------+--------------+------+-----+---------+-------+
4 rows in set (0.01 sec)
```

### テーブル自体を直接削除する
```
DROP TABLE テーブル名

mysql> DROP TABLE people;
```

### テーブルに格納したデータを削除する
```
TRUNCATE TABLE テーブル名

mysql> TRUNCATE TABLE people;
```

### テーブルの情報を変更する  
カラム名の変更や追加、削除を行ったり、オプション内容の変更など様々な操作を行うことができる
```
ALETER TABLE　テーブル名　MODIFY　カラム名　変更後の内容

mysql> ALTER TABLE people MODIFY age TINYINT UNSIGNED;
mysql> ALTER TABLE people MODIFY person_id INT UNSIGNED;
mysql> ALTER TABLE people MODIFY person_id INT UNSIGNED AUTO_INCREMENT;
```

## DML

テーブルではなく実際のデータを操作するクエリを<span style="color:blue">DML(Data Manipulation Language)</span>と呼ぶ。  

### レコードを追加する
```
INSERT INTO テーブル名 (カラム1, カラム2, ...カラムn) VALUES (カラム1の値, カラム2の値, ...カラムnの値);

mysql> INSERT INTO people (name, email, age, gender) VALUES ('鈴木たかし', 'suzuki@gizumo.jp', 20, 1);

mysql> INSERT INTO people (name, email, age, gender)
    -> VALUES
    -> ('田中ゆうこ', 'tanaka@gizumo.jp', 25, 2),
    -> ('福田だいすけ', 'fukuda@gizumo.jp', 42, 1),
    -> ('豊島はなこ', 'toyoshima@gizumo.jp', 34, 2),
    -> ('早坂てつお', 'hayasaka@gizumo.co.jp', 61, 1),
    -> ('不思議沢みちこ', NULL, NULL, NULL);
```
1つのクエリで複数のレコードを作成することを<span style="color:blue">バルクインサート</span>と呼ぶ。  
1行ずつ複数のINSERTを実行するよりも実行速度が優れているのが特徴。  

### レコードの確認
```
SELECT カラム名 FROM テーブル名;

mysql> SELECT * FROM people;
+-----------+-----------------------+-----------------------+------+--------+---------------------+---------------------+
| person_id | name   | email  | age  | gender | created_at | updated_at |
+-----------+-----------------------+-----------------------+------+--------+---------------------+---------------------+
|1| 鈴木たかし| suzuki@gizumo.jp | 20 | 1 | 2020-05-25 18:47:26 | 2020-05-25 18:47:26 |
|2| 田中ゆうこ | tanaka@gizumo.jp |25 | 2 | 2020-05-25 18:48:16 | 2020-05-25 18:48:16 |
|3 | 福田だいすけ|fukuda@gizumo.jp |42 |1 | 2020-05-25 18:48:16 | 2020-05-25 18:48:16 |
+-----------+-----------------------+-----------------------+------+--------+---------------------+---------------------+
5 rows in set (0.00 sec)
```

### 特定のカラムの確認
```
mysql> SELECT name, email, age, gender FROM people;

+-----------------------+-----------------------+------+--------+
| name                  | email                 | age  | gender |
+-----------------------+-----------------------+------+--------+
| 鈴木たかし            　| suzuki@gizumo.jp      |   20 |      1 |
| 田中ゆうこ            　| tanaka@gizumo.jp      |   25 |      2 |
| 福田だいすけ       　　　| fukuda@gizumo.jp      |   42 |      1 |
| 豊島はなこ            　| toyoshima@gizumo.jp   |   34 |      2 |
| 早坂てつお            　| hayasaka@gizumo.co.jp |   61 |      1 |
| 不思議沢みちこ       　　| NULL                  | NULL |   NULL |
+-----------------------+-----------------------+------+--------+
5 rows in set (0.00 sec)
```

### 特定のレコードの確認
```
WHERE カラム名 = 値と記述することで、カラムの値と指定した値が一致しているレコードだけが取得できる

mysql> SELECT name, email, age, gender FROM people WHERE gender = 1;
+--------------------+-----------------------+------+--------+
| name               | email                 | age  | gender |
+--------------------+-----------------------+------+--------+
| 鈴木たかし         　| suzuki@gizumo.jp      |   20 |      1 |
| 福田だいすけ    　　　| fukuda@gizumo.jp      |   42 |      1 |
| 早坂てつお         　| hayasaka@gizumo.co.jp |   61 |      1 |
+--------------------+-----------------------+------+--------+
1 row in set (0.00 sec)
```

### レコードの更新
```
UPDATE テーブル名 SET カラム名 = 値;

mysql> UPDATE people SET email = 'hayasaka@gizumo.jp' WHERE person_id = 5;
```

### レコードの削除
レコードを指定しないとテーブルの中の値が全て削除されてしまう。  
全件削除する時以外は必ずWHEREでレコードを指定するのを忘れないように。
```
DELETE FROM テーブル名;

mysql> DELETE FROM people WHERE person_id = 5;
```

### 完全一致するレコードの取得
```
WHERE カラム名 = 値

mysql> SELECT * FROM people WHERE person_id = 1;
```

### 一致しないレコードの取得
```
!=　か　<>を利用する

mysql> SELECT * FROM people WHERE name != '福田だいすけ';
```

### 範囲に一致するレコードの取得
```
>や<の利用

mysql> SELECT * FROM people WHERE age > 40;  -- ageカラムが40よりも大きいレコードを取得

mysql> SELECT * FROM people WHERE age >= 40; -- ageカラムが40以上のレコードを取得

mysql> SELECT * FROM people WHERE age < 40;  -- ageカラムが40未満のレコードを取得

mysql> SELECT * FROM people WHERE age <= 40; -- ageカラムが40以下のレコードを取得
```

### 複数条件に一致する/しないレコードの取得
```
WHERE カラム名 IN (値1, 値2, 値3...値N);

mysql> SELECT * FROM people WHERE name IN ('鈴木たかし', '豊島はなこ');

一致しないレコードの取得
NOT INの利用

mysql> SELECT * FROM people WHERE name NOT IN ('鈴木たかし', '豊島はなこ');
```

### 論理演算子による条件設定
```
mysql> SELECT * FROM people WHERE age > 40 AND gender = 1;  

mysql> SELECT * FROM people WHERE age > 40 OR age < 20;

mysql> SELECT * FROM people WHERE 20 <= age AND age <= 40;

BETWEEN
WHERE カラム名 BETWEEN 値1 AND 値2;

mysql> SELECT * FROM people WHERE 20 <= age AND age <= 40;
↓書き換え
mysql> SELECT * FROM people WHERE age BETWEEN 20 AND 40;

mysql> SELECT * FROM people WHERE age NOT BETWEEN 20 AND 40;
```

### NULL検索
```
mysql> SELECT * FROM people WHERE age IS NULL;
```

### 部分一致
```
LIKEと%を用いる

mysql> SELECT * FROM people WHERE name LIKE '%こ';

mysql> SELECT * FROM people WHERE name LIKE '田%';

mysql> SELECT * FROM people WHERE name LIKE '%田%';
```

### レコードの順番を変更する
```
昇順
ORDER BY カラム名 ASC

降順
ORDER BY カラム名 DESC

mysql> SELECT * FROM people ORDER BY age ASC;
mysql> SELECT * FROM people ORDER BY age DESC;
```

### 取得件数を制限する
```
LIMIT

mysql> SELECT * FROM people LIMIT 3;
```

### 取得範囲をズラす
```
OFFSET

mysql> SELECT * FROM people LIMIT 3 OFFSET 3;
```

### LIMITとOFFSETの利用  
一覧情報をページ情報で分割して表示することができる  
1ページ目はLIMIT 10 OFFSET 0で、2ページ目はLIMIT 10 OFFSET 10でデータを取得してあげればいくらでもページを作れる
```
mysql> SELECT * FROM people LIMIT 3 OFFSET 3;
```

### OFFSETの省略  
LIMIT オフセット, 取得件数
```
mysql> SELECT * FROM people LIMIT 3, 3;
```

### 重複データの除外
データの中から、全てのパターンだけを取得したい場合に使用
```
SELECT DISTINCT カラム名

mysql> SELECT DISTINCT gender FROM people;
```

## テーブル結合

他のテーブルと紐付けるためのカラムを<span style="color:blue">外部キー</span>と呼ぶ。  
この外部キーを使って、２つのテーブルを結合していく。  
内部結合はどちらかのデータが欠けている場合は除外して取得するが、外部結合の場合は基準になっているテーブルを元に結合する。  
そのため、結合先のレコードが存在しない場合はNULLで埋めるようになるという違いがある。  
対応する値がない場合に除外したいかどうかで使い分ける。  
どちらでも同じ結果が得られる場合は、内部結合にしたほうがパフォーマンスが良くなる。

### 内部結合

<span style="color:blue">JOIN</span>を利用する  
どのテーブルの何カラムとどのテーブルの何カラムを紐付けるかという条件を<span style="color:blue">結合条件</span>という  
カラム名を指定する際に、<span style="color:blue">テーブル名.カラム名</span>という風にテーブル名とカラム名をドットで連結して書く  
```
SELECT カラム名 FROM テーブルA INNER JOIN テーブルB ON 結合条件

mysql> SELECT * FROM people INNER JOIN reports ON people.person_id = reports.person_id;
```

### 外部結合
<span style="color:blue">LEFT OUTER JOIN</span>か<span style="color:blue">RIGHT OUTER JOIN</span>を利用する  
LEFTやRIGHTのキーワードでどちらのテーブルを基準にするかを指定する
```
FROM 左のテーブル名 LEFT OUTER JOIN 右のテーブル名 条件

mysql> SELECT p.person_id, p.name, r.content FROM people p LEFT OUTER JOIN reports r USING (person_id);

RIGHT OUTER JOIN

mysql> SELECT p.person_id, p.name, r.content FROM people p RIGHT OUTER JOIN reports r USING (person_id);
```

### グループを作成する  
グループ化したものに関しては、<span style="color:blue">集計関数</span>というものを使っていろいろな集計を行うことができるようになる
```
GROUP BY カラム名

mysql> SELECT gender FROM people GROUP BY gender;
```

### テーブルにレコードが何件あるかをカウントする
```
COUNT関数

mysql> SELECT gender, COUNT(*) AS people_count FROM people GROUP BY gender;
+--------+--------------+
| gender | people_count |
+--------+--------------+
|      1 |            3 |
|      2 |            2 |
|   NULL |            1 |
+--------+--------------+
3 rows in set (0.00 sec)
```

### レコードの最大値・最小値を取得する
グループ化していない状態で実行すると、全体の中から最大値や最小値を探すようになるので使い分け必要
```
MAX関数
MIN関数

mysql> SELECT gender, MAX(age) AS max_age, MIN(age) AS min_age FROM people GROUP BY gender;
+--------+---------+---------+
| gender | max_age | min_age |
+--------+---------+---------+
|      1 |      61 |      20 |
|      2 |      34 |      25 |
|   NULL |    NULL |    NULL |
+--------+---------+---------+
```

### 平均値の算出
```
AVG関数

mysql> SELECT AVG(age) AS average_age FROM people GROUP BY gender;
+-------------+
| average_age |
+-------------+
|     41.0000 |
|     29.5000 |
|        NULL |
+-------------+
3 rows in set (0.00 sec)
```

### 合計値の算出
グループ化しない場合は全レコードの合計値を計算することができる
```
SUM関数

mysql> SELECT SUM(age) AS total_age FROM people GROUP BY gender;
+-----------+
| total_age |
+-----------+
|       123 |
|        59 |
|      NULL |
+-----------+
3 rows in set (0.00 sec)
```

### 条件分岐
<span style="color:blue">CASE</span>を使うが、書き方が2種類あるので注意
```
CASE

-- パターン1

CASE 比較対象
    WHEN 値A THEN 処理A
    WHEN 値B THEN 処理B
    ...
    WHEN 値N THEN 処理N
    ELSE 処理X
END

-- パターン2

CASE
    WHEN 条件A THEN 処理A
    WHEN 条件B THEN 処理B
    ...
    WHEN 条件N THEN 処理N
    ELSE 処理X
END

mysql> SELECT name, age,
    -> CASE gender
    -> WHEN 1 THEN '男性'
    -> WHEN 2 THEN '女性'
    -> ELSE '不明'
    -> END AS gender
    -> FROM people;

mysql> SELECT name, age,
    -> CASE
    -> WHEN gender = 1 THEN '男性'
    -> WHEN gender = 2 THEN '女性'
    -> ELSE '不明'
    -> END AS gender
    -> FROM people;
```

### サブクエリ

クエリの中に含まれている別のクエリをのことを<span style="color:blue">サブクエリ</span>と呼ぶ。  
処理が２回走ることになるのでパフォーマンスが悪化することが多いので注意
```
mysql> SELECT * FROM people WHERE department_id IN (
    -> SELECT department_id FROM departments WHERE name = '営業' OR name = '人事'
    -> );
↓これをやっている
mysql> SELECT * FROM people WHERE department_id IN (1, 4);
```

### UNION
取得したレコードを縦に結合するためのクエリ  
まず、SELECTを使用してデータを取得すると、レコードが縦に並ぶ
```
mysql> SELECT content FROM reports LEFT JOIN people USING (person_id) WHERE age < 30;
+--------------------------------------------------------------+
| content                                                      |
+--------------------------------------------------------------+
| 今日は平和でした                                              　|
| 上司に飲みに誘われました。                                    　　|
| そういえば彼は今頃何をしているのだろう                          　　|
| 苦しい…！                                                  　　|
| 苦しくない…！                                                  |
| お昼ごはんにとんかつを食べた気がします                          　　|
| お水にはとりあえず氷                                         　　|
| こ、こいつ…！                                             　　　|
+--------------------------------------------------------------+

mysql> SELECT content FROM reports LEFT JOIN people USING (person_id) WHERE gender = 2;
+--------------------------------------------------------------+
| content                                                      |
+--------------------------------------------------------------+
| 上司に飲みに誘われました。                                    　　|
| そういえば彼は今頃何をしているのだろう                          　　|
| 苦しくない…！                                                  |
| お水にはとりあえず氷                                         　　|
+--------------------------------------------------------------+
```
これをUNIONすると、縦に結合される
```
mysql> SELECT content FROM reports LEFT JOIN people USING (person_id) WHERE age < 30
       UNION ALL
       SELECT content FROM reports LEFT JOIN people USING (person_id) WHERE gender = 2;
+--------------------------------------------------------------+
| content                                                      |
+--------------------------------------------------------------+
| 今日は平和でした                                              　|
| 上司に飲みに誘われました。                                    　　|
| そういえば彼は今頃何をしているのだろう                          　　|
| 苦しい…！                                                  　　|
| 苦しくない…！                                                  |
| お昼ごはんにとんかつを食べた気がします                          　　|
| お水にはとりあえず氷                                         　　|
| こ、こいつ…！                                             　　　|
| 上司に飲みに誘われました。                                    　　|
| そういえば彼は今頃何をしているのだろう                          　　|
| 苦しくない…！                                                  |
| お水にはとりあえず氷                                         　　|
+--------------------------------------------------------------+
```

UNIONのオプション  
<span style="color:blue">ALL</span>　レコードが重複しているかどうかに関係なく、上下のレコードを結合します。  
<span style="color:blue">DISTINCT</span>　重複するレコードするレコードを排除して結合

### インデックス

テーブルのカラムがどのような順番でデータを格納しているのかという情報を別途テーブルのような形で保存することで、辞書の索引のようにデータを検索できるようになる。  
データベースからのデータ取得をより高速化するために使われるもの。  
MySQLでは主に、<span style="color:blue">B木インデックス</span>と呼ばれる種類のインデックスを使用している。  
インデックスというのは、INSERT、UPDATE、DELETEのようなレコードに変更を加えるクエリのパフォーマンスを犠牲にした上で、SELECT文を高速化させている。なので、不要ならやらない。
```
mysql> ALTER TABLE テーブル名 ADD INDEX インデックス名(カラム名);
```

### EXPLAIN
出力された情報のことを<span style="color:blue">実行計画</span>と呼ぶ。
MySQLには<span style="color:blue">クエリオプティマイザ</span>というプログラムが搭載されており、ここではクエリを実行する前にどのようにテーブルをチェックすれば最も効率よくデータを取得できるかを推測している。  
具体的にはどのインデックスを使用するかを決定したり、同じ結果が得られる別のクエリに差し替えたりなどをしている。  
その過程を見える化したものが、EXPLAINで出力した実行計画。  

リリース前の性能調査  
不具合（主にスロークエリ）の調査  
調査やデータメンテのために、稼働中の本番環境にてやむを得ずDMLを実行する際の事前確認  
などで使う
```
EXPLAIN 解析したいクエリ

mysql> EXPLAIN SELECT * FROM city_demo WHERE city = 'Tegal'\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: city_demo
   partitions: NULL
         type: ref
possible_keys: idx_city
          key: idx_city
      key_len: 202
          ref: const
         rows: 249
     filtered: 100.00
        Extra: NULL
1 row in set, 1 warning (0.00 sec)

id	SELECTの順番	1
select_type	SELECTの種類	SIMPLE, UNION, PRIMARY, SUBQUERY, その他多数
table	クエリが参照するテーブル	city_demoなどのテーブル名
partitions	一致するパーティション	パーティションを使用している場合パーティション名
type	結合型	ref, const, eq_refなど多数
possible_keys	選択可能なインデックス	idx_cityなどのインデックス名
key	実際に選択されたインデックス	idx_cityなどのインデックス名
key_len	選択されたキーの長さ。小さいと速いインデックスになる	202などの整数値
ref	インデックスと比較されるカラム	const, もしくはカラム名
rows	調査される行の見積もり	249などの行数
filtered	テーブル条件によってフィルタ処理される行の割合	100.00などの数値
Extra	追加情報	NULL, Using where, Using temporaryなど多数
```

### テーブルの複製

```
-- actor_bkという名前でactorバックアップテーブルを作成する
mysql> CREATE TABLE actor_bk LIKE actor;
Query OK, 0 rows affected (0.01 sec)

-- 新しく作ったテーブルにもとのテーブルのデータを全て挿入する
mysql> INSERT INTO actor_bk SELECT * FROM actor;
Query OK, 200 rows affected (0.01 sec)
Records: 200  Duplicates: 0  Warnings: 0
```

### データベースのバックアップ
<span style="color:blue">mysqldump</span>というコマンドを使用する  
MySQLのクエリではなくコマンドなので、一度MySQLからログアウトしてから実行する
```
mysqldump -u ユーザー名 -p データベース名 > 保存先のパス/ファイル名

$ mysqldump -u root -p db_lesson > /Users/matsushima/Desktop/dumps/db_bk.dump


```

***

## ECサイト構築

マルチログインの場合、機能の分別がまず必要になる  

| サイトの種類 | 提供側（販売側） | 利用者側（購入側） |
|:-----------|------------:|:------------:|
| 物販 | 商品の登録 | 商品を探す・買う |
| 不動産 | 物件の登録 | 物件を探す |
| 求人 | 求人情報の登録 | 求人情報を探す |
| 副業 | スキルの登録 | 依頼する |
| 家電修理 | エアコンなどの修理内容の登録 | 探す・依頼する |

### Laravelのバージョンによる変更内容
```
6.0 LTS
Laravel UIを追加

7.0
Sanctum,Bladeコンポーネントを追加

8.0
Jetstream,livewire/inortiatailwind.css採用,model/factory/routingの記述方法変更

9.0 LTS
2021.9~
```
### PHPの年表
```
1995 1.0
2004 5.0
2014 5.6
2015 7.0
2018 7.3
2019 7.4
2020 8.0
```

### PHPのインストール
```
$ brew install php@7.3
```

PATH通し  
通常PHPはそれぞれがインストールされているディレクトリからしか使えませんが、それらを使用して開発を行うにはどのディレクトリからも実行できるようにする必要がある  
PCに、PHPがインストールされている場所を覚えさせることによって、どのディレクトリでPHPを実行してもその場所までPCが探しにいけるようにしてあげている  
インストール後、以下の記述を探し、ターミナルにコピペし実行すれば終わり
```
If you need to have php@7.3 first in your PATH run:
 echo 'export PATH="/usr/local/opt/php@7.4/bin:$PATH"' >> ~/.bash_profile
 echo 'export PATH="/usr/local/opt/php@7.4/sbin:$PATH"' >> ~/.bash_profile

$ echo 'export PATH="/usr/local/opt/php@7.4/bin:$PATH"' >> ~/.bash_profile
$ echo 'export PATH="/usr/local/opt/php@7.4/sbin:$PATH"' >> ~/.bash_profile
```

composerのアップデートとダウングレード
```
$ composer self-update

バージョン1.Xに戻す場合
$ composer self-update --rollback
```

laravelプロジェクトの作成
```
$ composer create-project laravel/laravel umarche "8.*" --prefer-dist

Application key set successfully.
と出ればOK
```

DBと環境設定
```
①MySQLでDBを作っておく&接続確認
mysql > create database データベース名
$ php artisan migrate

②タイムゾーン、言語設定をいじる　config/app.php
'timezone' => 'Asia/Tokyo',
'locale' => 'ja',

③.env設定をいじる
④バリデーションの言語ファイルをいじる
⑤デバックバーを入れておく
$ composer require barryvdh/laravel-debugbar
```

### Laravelの認証ライブラリの比較  

| Laravel/UI | Laravel Breeze | Fortify | Jetstream |
|:-----------|------------:|:------------:|:------------:|
| Blade | Blade | - | Livewire+Blade |
| Vue.js/React.js | Alpine.js | - | Inertia.js/Vue.js |
| Bootstrap | Tailwind.css | - | Tailwind.cs |
| （追加ファイル）View/Controller/Route | View/Controller/Route | - | View/Controller/Route |
| （機能１）ログイン、ユーザ登録、パスワードリセット、メール検証、パスワード確認 | 同 | 同 | - |
| （機能２）- | - | 二要素認証、プロフィール管理、チーム管理 | APIサポート（Sanctum） |

### Laravel Breezeのインストール

```
①
$ composer require laravel/breeze "1.*" --dev

Package manifest generated successfully.
76 packages you are using are looking for funding.
Use the `composer fund` command to find out more!
が出る

②
$ php artisan breeze:instal

Breeze scaffolding installed successfully.
Please execute the "npm install && npm run dev" command to build your assets.
が出る

③
$ npm install && npm run dev

Laravel Mix v6.0.23
Compiled Successfully in 6940ms
webpack compiled successfully
が出る

※　npm install
package.jsonのファイルに書かれているdevDependenciesのソフトを一括でダウンロードするためのコマンド

※　npm run dev
cssやjsをコンパイルするためのコマンド

インストールができたら、画面右上に
Login Register
が出てくるようになるので、そこから判別すること
```

### Laravel Breezeをインストールした後追加されるファイル

```
app/Http/Controllers/Auth
app/Http/Controllers/Requests/Auth
app/View/Components
routes/web.php
routes/auth.php
resources/views/auth
resources/views/components
```

### index.phpでやっていること

```
<?php

use Illuminate\Contracts\Http\Kernel;
use Illuminate\Http\Request;

define('LARAVEL_START', microtime(true));

if (file_exists(__DIR__.'/../storage/framework/maintenance.php')) {
    require __DIR__.'/../storage/framework/maintenance.php';
}
メンテナンスモードの場合に読み込む
メンテナンスモードでない場合には以下の処理に移る

require __DIR__.'/../vendor/autoload.php';
autoload.phpは全てのクラスを読み込んでくれるファイル

$app = require_once __DIR__.'/../bootstrap/app.php';
bootstrap/app.phpはサービスコンテナなど、様々な機能を読み込んでくれるファイル

$kernel = $app->make(Kernel::class);

$response = tap($kernel->handle(
    $request = Request::capture()
))->send();

$kernel->terminate($request, $response);
```

### 認証機能を司るファイル

サービスプロバイダ  
config/app.phpの
providers,aliasesにAuthと記載
※aliasesは別名という意味

ルーティング
routes/web.php
```
Route::get('/dashboard', function () {
    return view('dashboard');
})->middleware(['auth'])->name('dashboard');

middleware(['auth'])が認証されているかどうかのチェックをしてくれている
もし認証されていれば、dashboardを表示するというプログラムになっている

require __DIR__.'/auth.php';
__DIR__は現在のディレクトリを表す

なので、現在のディレクトリのauth.phpを読み込みますよ〜って意味になる
```

routes/auth.php
たくさんのコントローラーを読みこんでくれるファイル
```
use Illuminate\Support\Facades\Route;
Routeを読み込む

use App\Http\Controllers\Auth\RegisteredUserController;
コントローラを読み込む

Route::get('/register', [RegisteredUserController::class, 'create'])
        ->middleware('guest')
        ->name('register');
Route::get(url,[コントローラ名, メソッド名]) []は配列なので、配列でコントローラとメソッドを記述する。前は@でつなげてたものが仕様変更された。
->middleware('guest') guestだったら＝まだログインしてなかったら
->name('register');　名前つきルート
```

### Laravel Breezeで追加されるコントローラ群
```
app/Http/Controllers/Auth

AuthenticatedSessionController　認証セッションを司る
ConfirmablePasswordController　パスワード確認を司る
EmailVerificationNotificationController　メール検証通知を司る
EmailVerificationPromptController　メール検証プロンプトを司る
NewPasswordController　新しいパスワードの設定を司る
PasswordResetLinkController　パスワードリセットリンクを司る
RegisteredUserController　ユーザー登録を司る
VerifyEmailController　メール検証を司る

```

### Laravel Breezeで追加されるビュー群
```
resources/views/auth

confirm-password.blade.php　パスワード確認のビュー
forgot-password.blade.php　パスワード忘れた時のビュー
login.blade.php　ログインのビュー
register.blade.php　新規登録のビュー
reset-password.blade.php　パスワードリセットのビュー
verify-email.blade.php　メール検証のビュー

```

### バリデーションの日本語化
```
resources/lang/validation.php


:attributeはこのファイルの一番下の
'attributes' => [],
を編集することで、バインドされる文字を変更することが出来る

'attributes' => [
    'name' => '名前',
    'email' => 'メールアドレス',
    'password' => 'パスワード'
]

"Whoops..."の変更

①resources/langの下に
js.json
など、jsonファイルを作る

②jsonファイルの構成は
{"":""}
こうなっている

{"Whoops":"おっと、サーバーで何か問題が発生しました。"}
と書き換え、保存する
```

### tailwind.cssについて
```
tail（追い風）
CSSフレームワーク

X UIキット
○ ユーティリティクラス
カスタマイズしやすい

Lowレベル（＝CSSに近い）
クラス名→CSSプロパティに近い
レスポンシブ対応、Flexbox、Grid採用

モバイルファーストでできている
sm 640px
md（タブレット） 768px
lg（ノートPC） 1024px
xl 1280px
2xl 1536px

作る順番は
全ての幅で共通の部分→md→lgという順番になる

bg-green-300 md:bg-green-300 lg:bg-green-300
全ての幅のcss md以上で適用するcss lg以上で適用するcss
md:flexで md以上でflexboxを有効化という意味になる

```

### tailwind.config.cssについて
```
purge
利用していないクラス以外を自動的に排除してファイルサイズを軽くしてくれる仕組みをパージと呼ぶ
```

### Bladeコンポーネント
これまでコントローラ側が担ってきた役割の一部を、コンポーネントクラスやBladeコンポーネントが担うことから、サブコントローラと呼ばれる

```
x-ファイル名
というタグを使って作る

<x-auth-card>みたいな
```

### マルチログイン関連ファイル
```
providers/RouteServiceProvider.php
ログイン後のURLを設定する

config/auth.php
Guardの設定、ログイン方法の設定、どのテーブルを使うかの設定を行う

middleware/Authenticate.php
未認証時のリダイレクト処理を行う

middleware/RedirectIfAuthenticated.php
ログイン済みユーザーのリダイレクトを行う
```

### ライフサイクル
画面に表示されるまでの全体の流れのこと  

どんな流れかというと  
１　webサーバがpublic/index.phpにリダイレクトする（エントリポイント）   
２　autoload読み込み  
autoloadとは、requireなしで別ファイルのクラスを利用可能にしてくれる仕組みのこと
名前空間やuse文が使えるようになる
vendor/autoload.php
```
<?php

// autoload.php @generated by Composer

require_once __DIR__ . '/composer/autoload_real.php';
ここでcomposerを読み込んでいる

return ComposerAutoloaderInit79e5a34f467db3c90bcd6ea1ea253925::getLoader();
```

```
index.php

$app = require_once __DIR__.'/../bootstrap/app.php';
bootstrap/app.phpを読みに行っている
ここのbootstrapはcssの意味ではなく、サービスコンテナの意味

ここがサービスコンテナの読み込み部分
$app = new Illuminate\Foundation\Application(
    $_ENV['APP_BASE_PATH'] ?? dirname(__DIR__)
);
```

３　Applicationインスタンス作成（＝サービスコンテナ）  
Illuminate\Foundation\Applicationのインスタンスをサービスコンテナと呼ぶ  


４　HttpKernelインスタンス作成  
５　Requestインスタンス作成  
６　HttpKernelがリクエストを処理してResponse取得  
７　レスポンス送信  
８　terminate()で後片付け  

### サービスコンテナ
コンテナとは大きな荷物を運ぶ箱の意味  
箱に登録する方法と箱から取り出す方法の大きく2種類のメソッドが用意されている  
箱はapp()という名前で表現する  
依存関係の解決機能などを持つ  

箱に登録する  
app()->bind()  
app()->singleton()  
でコンテナ側にデータを送る  

箱から取り出す  
app()->make()  

dd(app());で中身を確認出来る  
bindingsの項目がサービスの登録数

bindings: array:65  
→65のサービスが設定されていると読む  

サービスコンテナに登録する
```
app()->bind('lifeCycleTest', function(){  
    return 'ライフサイクルテスト';  
})  
引数（取り出す時の名前、機能）
```
こうするとbindings: array:66になる  

サービスコンテナから取り出す
```
$test = app()->make('lifeCycleTest');

他の書き方
$test = app('lifeCycleTest');
$test = resolve('lifeCycleTest');
$test = App::make('lifeCycleTest');
```

依存関係の解決  
依存した２つのクラスがあるとする  
それぞれをインスタンス化した後に実行する  
```
サービスコンテナを使わないパターン
$message = new Message();
$sample = new Sample($message);
$sample->run();

サービスコンテナを使ったパターン
app()->bind('sample', Sample::class);
$sample = app()->make('sample');
$sample->run();
```
