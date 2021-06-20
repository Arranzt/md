### ファイル構成

```
app
アプリケーションのプログラム部分がまとめられるところ。
ここに必要なファイルを追加していく。

bootstrap
アプリケーション実行時に最初に行われる処理がまとめられている

config
設定関係のファイルをまとめている

database
データベース関連のファイルをまとめている

public
公開フォルダ。JSやCSSなど、外部にそのまま公開されるファイルをまとめている

resources
リソース関連の配置場所。テンプレートファイルなどをまとめている

routes
ルート情報の保存場所。アクセスするアドレスに割り当てられるプログラムの情報などが記されている

storage
ファイルの保存場所。アプリケーションのプログラムが保存するファイルなどが配置される。
ログファイルもここに保管される

tests
ユニットテスト関係のファイルをまとめている

vendor
フレームワーク本体のプログラムをまとめている
```

### ルーティング

GETアクセス
```
Route::get(アドレス, 関数)

Route::get('/', function(){
  return view('welcome');
})
```

viewメソッド  
指定したテンプレートファイルをロードし、レンダリングして返す働きをしてくれる  
そのため、viewの引数にテンプレートを指定すると、それがレンダリングされて返され、ブラウザに表示される仕組みとなっている  
viewsフォルダからの検索となるので、パスの指定に注意
```
view(テンプレート名)

view('フォルダ名.ファイル名')

値をview側に渡したい時
view(テンプレート名, 配列)
```

ルートパラメータ  
アクセスする際にパラメータを設定し、値を渡したい時に使う
パラメータは必須パラメータと任意パラメータに分けられ、下記は必須パラメータの記述方法
```
Route::get('/アドレス/{パラメータ}', function(){});

Route::get('/welcome/{msg}', function(){
   return view('welcome');
})

この状態で/welcome/this_is_test
のようにURLを指定してリクエストを飛ばすと、URLにパラメータが付加される
```

任意パラメータ
パラメータの末尾に?をつけて宣言する方法  
パラメータを付けずともアクセスできるようにする仕組み
```
Route::get('/アドレス/{パラメータ?}', function(){});
```

### コントローラ

アクションとルートを割り当てる
```
Route::get(アドレス, コントローラ名@アクション名)
```

アクションとアドレスの関係
```
http://アプリケーションのアドレス/コントローラ/アクション
```

シングルアクションコントローラ
```
class コントローラ extends Controller
{
  public function __invoke(){
    アクション処理
  }
}

ルーティングの設定方法
Route::get(アドレス, コントローラ名)
```

### リクエストとレスポンス

RequestクラスとResponseクラスを用いる
```
use Illuminate\Http\Request
use Illuminate\Http\Response

使い方
public function index(Request $request, Response $response){処理}
```

### Requestクラスで用意されている主なメソッド  

アクセスしたURLを返す（クエリ文字を除く）
```
$request->url();
```

アクセスしたURLを返す（クエリ文字を含む）
```
$request->fullurl();
```

ドメインのパス部分だけを返す
```
$request->path();
```

### Responseクラスで用意されている主なメソッド

アクセスに関するステータスコードを返す
```
$this->status();
```

コンテンツの取得を行う
```
$this->content();
```

引数の値にコンテンツを変更する
```
$this->setContent(値);
```

### ビュー

値をviewに渡す
```
値をview側に渡したい時
view(テンプレート名, 配列)

$data = ['msg' => 'テストメッセージ'];
return view('hello.index', $data);
```

アクセスするアドレスの情報を利用して値を渡す場合  
クエリ文字列を使う
```
public function index(Request $request){
  $data = [
    'msg' => 'テストメッセージ',
    'id' => $request->id
  ]
  return view('hello.index', $data);
}

Route::get('hello', HelloController@index);

?id=メッセージとつけてアクセスすると、その内容が$request->キー名で取得できるようになる
http://localhost:8000/hello?id=fjrgjoagrigag
```

### フォーム送信

フォームの作成
```
<form method="POST" action="/hello">
  @csrf
  <input type="text" name="msg">
  <input type="submit">
</form>
```

アクションの用意
```
public function post(Request $request)
{
  $msg = $request->msg;
  $data = [
    'msg' = 'こんにちは' . $msg . 'さん',
  ];
  return view('hello.index', $data);
}
```

ルーティングの用意
```
Route::post('hello', 'HelloController@post');
```

### Bladeで利用するディレクティブ群

値の表示  
{{}}の間に文を書くことで、その文が返す値をその場に書き出す  
HTMLのエスケープ処理が自動的に行われる  
そのため、HTMLタグなどを書いてもテキストとしてのタグとして処理され、HTMLタグとして処理されない  
エスケープ処理をさせたくない場合は、{!! !!}の間に文を書く
```
{{ 値・変数・式・関数 }}

{!! 値・変数・式・関数 !!}
```

@ifディレクティブ
```
@if(条件)

@elseif

@else

@endif
```

@unlessディレクティブ
条件が非成立の場合に、処理を動かす
```
@unless(条件)

@endunless
```

@emptyディレクティブ
変数が空の場合に処理を動かす
```
@empty(条件)

@endempty
```

@issetディレクティブ
変数そのものが定義されている場合、処理を動かす
```
@isset

@endisset
```

@forディレクティブ
繰り返し処理を行う
```
@for(初期化;　条件;　後処理)

@endfor
```

@foreachディレクティブ
配列から値を取り出して処理をする
```
@foreach($配列 as $変数)

@endforeach
```

@forelseディレクティブ
配列から順に値を取り出し処理を繰り返すが、値を全て取り出し終えた時、@emptyにある処理を実行して繰り返しを終える
```
@forelse($配列 as $変数)

@empty

@endforelse
```

@whileディレクティブ
```
@while(条件)

@endWhile
```

@breakディレクティブ  
PHPのbreakに相当。これが出力されると、その時点で繰り返しのディレクティブが中断される  

@continueディレクティブ  
PHPのcontinueに相当。これより後は表示せず、すぐに次の繰り返しに進む  

### ループ変数
繰り返しディレクティブには$loopという特別な変数が用意されており、繰り返しに関する情報を保持してくれている
```
$loop->index
現在のインデックスを取り出す

$loop->iteration
現在の繰り返し数を取り出す

$loop->remaining
後何回繰り返すかという残り回数を取りだす

$loop->count
繰り返しで使っている配列の要素数を取り出す

$loop->first
最初の繰り返しかどうかを判定する

$loop->last
最後の繰り返しかどうかを判定する

$loop->depth
繰り返しのネスト数を判定する

$loop->parent
ネストしている場合、親の繰り返しのループ変数を示す
```

### @PHPディレクティブ

PHPのスクリプトを直接実行したい時に利用する
```
@PHP

@endphp
```

### Bladeの文法

継承  
すでにあるテンプレートのレイアウトを継承して新しいテンプレートを作成する  

セクション  
継承でページをデザインする時に、ページ内の要素として活用される  

@section  
ページに表示されるコンテンツの区画を定義する  
このセクションは、@yieldに嵌め込まれ、表示される
```
@section(名前)

@endsection
```

@yield  
セクションの内容をはめ込んで表示するためのディレクティブ  
@yieldは配置場所を示すもの
```
@yield(名前)
```

@extends  
レイアウトの継承設定を行
```
@extends('親レイアウトのbladeファイル名')

@extends('layouts.helloapp')
layoutsフォルダ内のhelloapp.blade.phpというレイアウトファイルをロードし、親レイアウトとして継承しますという意味になる
```

@parent  
親レイアウトのセクションを示す  

@component
```
@component(名前)

@endcomponent
```







