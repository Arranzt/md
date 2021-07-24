## はじめ方
```
2種類存在

①nestCLIを用いる方法
$ npm i -g @nestjs/cli
$ nest new project-name
※$ nest -vでバージョン確認可能。確認できる場合、わざわざ改めてインストールする必要はないので、nest プロジェクト名で新規プロジェクトを始めてしまって良い。

②Gitからスタータープロジェクトをインストールする方法
$ git clone https://github.com/nestjs/typescript-starter.git project
$ cd project
$ npm install
$ npm run start
```

## コアファイルについて
プロジェクトを新規作成した場合、５つのコアファイルが作成される。
１．app.controller.ts	簡便なシングルルート用コントローラ  
２．app.controller.spec.ts	ユニットテスト用コントローラ  
３．app.module.ts	アプリケーションのルートモジュール  
４．app.service.ts	シングルメソッドの簡便なサービス  
５．main.ts	NestFactory機能を使いNest アプリケーションインスタンスを作る為のファイル  
