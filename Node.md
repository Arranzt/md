### 環境設定
```
Macの場合

1、Homebrewのインストール
$ /usr/bin/rubye"$(curlfsSLhttps://raw.githubusercontent.com/Homebrew/install/master/install)"
$ brew update

2、Node.js npmのインストール
$ brew install node npm
```

### Expressの新規プロジェクト作成からHello worldまで
```
$ npm init
package.jsonを作成するためのコマンド
Nodeではpackage.jsonにまとめて依存する外部のパッケージとそのバージョンを記録しておき、実際にアプリケーションを動かす段階になってから、npm installにより、package.jsonに書かれた外部ライブラリをまとめてインストールするやり方が一般的

package name: (pra) sample
version: (1.0.0) 
description: 
entry point: (index.js) app.js
test command: 
git repository: 
keywords: 
author: Cluster
license: (ISC) MIT

$ npm install express --save
--saveオプション：package.jsonのdependenciesに導入したパッケージとそのバージョンが追加され、あとでpackage.jsonから必要なモジュールをチェックしたり、再導入することが簡単になる
```
続いて、app.jsというファイルを手動で新規作成
```
まず、expressをrequireする
var express = require("express");

expressは通常関数になっていることから、appという変数に置き直すことが多い
var app = express();

appはlistenというメソッドを持っており、番号を指定してあげると通信できるようになる
app.listen(3000);

ルーティングの設定を行う
app.get("/", (req, res) => {
  res.writeHead(200);
  res.write("Hello world");
  res.end();

という書き方も出来るが、
  res.status(200).send("Hello World")
});

```

### Expressについて
```
最低限しか機能提供していないNodeのフレームワーク
できること
ルーティング
レスポンスの整形

できないこと
リクエスト内容の分析（クエリパラメータ分析、クッキー分析、セッション管理）→cookie-parser/body-parser/express-session
認証認可→passport
DB接続
→ミドルウェアを拡張しながら利用していく
```
