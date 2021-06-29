# First step

この一連の記事では、Nestのコアの基礎を学びます。 Nestアプリケーションの基本的な構成要素に慣れるために、入門レベルで多くの分野をカバーする機能を備えた基本的なCRUDアプリケーションを構築します。

言語
私たちはTypeScriptが大好きですが、何よりもNode.jsが大好きです。 そのため、Nestは<span style="color: red; ">TypeScriptと純粋なJavaScript</span>の両方と互換性があります。 Nestは最新の言語機能を利用しているため、バニラJavaScriptで使用するには、<span style="color: red; ">Babelコンパイラ</span>が必要です。

提供する例では主にTypeScriptを使用しますが、コードスニペットをバニラJavaScript構文にいつでも切り替えることができます（各スニペットの右上隅にある言語ボタンをクリックして切り替えるだけです）。

前提条件
Node.js（v13を除く> = 10.13.0）がオペレーティングシステムにインストールされていることを確認してください。

セットアップ
Nest CLIを使用すると、新しいプロジェクトの設定は非常に簡単です。 npmをインストールすると、OS端末で次のコマンドを使用して新しいNestプロジェクトを作成できます。

```
$ npm i -g @nestjs/cli
$ nest new project-name
```

project-nameディレクトリが作成され、ノードモジュールと他のいくつかの定型ファイルがインストールされ、src /ディレクトリが作成され、いくつかのコアファイルが入力されます。
```
src
  app.controller.spec.ts
  app.controller.ts
  app.module.ts
  app.service.ts
  main.ts
```

これらのコアファイルの概要は次のとおりです。
| ファイル名 | 概要 |
|:-----------|------------:|
| app.controller.ts | 単一ルートの基本的なコントローラー。 |
| app.controller.spec.ts | ユニットはコントローラーをテストします。 |
| app.module.ts | アプリケーションのルートモジュール。 |
| app.service.ts | 単一の方法による基本的なサービス。 |
| main.ts | コア関数NestFactoryを使用してNestアプリケーションインスタンスを作成するアプリケーションのエントリファイル。 |

main.tsには、アプリケーションをブートストラップする非同期関数が含まれています。
```
main.tsJS

import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
```

Nestアプリケーションインスタンスを作成するには、コアの<span style="color: red; ">NestFactoryクラス</span>を使用します。 NestFactoryは、アプリケーションインスタンスの作成を可能にするいくつかの静的メソッドを公開します。 <span style="color: red; ">create（）メソッド</span>は、INestApplicationインターフェイスを満たすアプリケーションオブジェクトを返します。 このオブジェクトは、次の章で説明する一連のメソッドを提供します。 上記のmain.tsの例では、HTTPリスナーを起動するだけで、アプリケーションはインバウンドHTTPリクエストを待機できます。

Nest CLIでスキャフォールドされたプロジェクトは、開発者が各モジュールを専用のディレクトリに保持するという規則に従うことを奨励する初期プロジェクト構造を作成することに注意してください。

プラットフォーム
Nestは、プラットフォームに依存しないフレームワークになることを目指しています。 プラットフォームに依存しないため、開発者が複数の異なるタイプのアプリケーションで利用できる再利用可能な論理パーツを作成できます。 技術的には、アダプターが作成されると、Nestは任意のノードHTTPフレームワークと連携できます。 すぐにサポートされるHTTPプラットフォームには、expressとfastifyの2つがあります。 ニーズに最適なものをお選びいただけます。

| ファイル名 | 概要 |
|:-----------|------------:|
| platform-express | Expressは、ノード用のよく知られたミニマリストWebフレームワークです。 これは、コミュニティによって実装された多くのリソースを備えた、戦闘でテストされた、本番環境に対応したライブラリです。 @ nestjs / platform-expressパッケージがデフォルトで使用されます。 多くのユーザーはExpressのサービスを十分に受けており、Expressを有効にするために何もする必要はありません。 |
| platform-fastify | Fastifyは、最大の効率と速度を提供することに重点を置いた、高性能でオーバーヘッドの少ないフレームワークです。 ここでそれを使用する方法を読んでください。 |

どちらのプラットフォームを使用しても、独自のアプリケーションインターフェイスが公開されます。 これらは、それぞれNestExpressApplicationおよびNestFastifyApplicationと見なされます。

以下の例のように、型をNestFactory.create（）メソッドに渡すと、アプリオブジェクトにはその特定のプラットフォーム専用のメソッドがあります。 ただし、基盤となるプラットフォームAPIに実際にアクセスする場合を除いて、タイプを指定する必要はありません。

```
const app = await NestFactory.create<NestExpressApplication>(AppModule);
```
アプリケーションの実行
インストールプロセスが完了したら、OSコマンドプロンプトで次のコマンドを実行して、インバウンドHTTP要求をリッスンするアプリケーションを起動できます。
```
$ npm run start
```
このコマンドは、src /main.tsファイルで定義されたポートをリッスンしているHTTPサーバーでアプリを起動します。 アプリケーションが実行されたら、ブラウザを開いてhttp：// localhost：3000 /に移動します。 Hello Worldが表示されるはずです！

***

# Controllers

コントローラーは、リクエストを処理し、クライアントにレスポンスを返す責任があります。  

コントローラの目的は、アプリケーションに対する特定のリクエストを受信することです。  
ルーティングメカニズムは、どのコントローラーがどのリクエストを受信するかを制御します。    
多くの場合、各コントローラーには複数のルートがあり、ルートが異なれば異なるアクションを実行できます。

基本的なコントローラーを作成するために、クラスとデコレーターを使用します。 <span style="color: red; ">デコレータ</span>はクラスを必要なメタデータに関連付け、Nestがルーティングマップを作成できるようにします（リクエストを対応するコントローラーに結び付けます）。  

*デコレータとは、関数の中身を書き換えずに関数を修飾するための仕組みです。
ライブラリ内の関数の機能を変更したいときなどに役立ちます。*  

ルーティング
次の例では、基本的なコントローラーを定義するために必要な<span style="color: red; ">@Controller（）デコレーター</span>を使用します。 catsのオプションのルートパスプレフィックスを指定します。 @Controller（）デコレータでパスプレフィックスを使用すると、関連するルートのセットを簡単にグループ化し、繰り返しのコードを最小限に抑えることができます。 たとえば、ルート/ customersの下の顧客エンティティとの相互作用を管理する一連のルートをグループ化することを選択できます。 その場合、@ Controller（）デコレータでパスプレフィックスcustomersを指定して、ファイル内のルートごとにパスのその部分を繰り返す必要がないようにすることができます。

```
cats.controller.tsJS

import { Controller, Get } from '@nestjs/common';

@Controller('cats')
export class CatsController {
  @Get()
  findAll(): string {
    return 'This action returns all cats';
  }
}
```

CLIを使用してコントローラーを作成するには、`$ nest g controllercats`コマンドを実行するだけです。






