# First step

この一連の記事では、Nestのコアの基礎を学びます。 Nestアプリケーションの基本的な構成要素に慣れるために、入門レベルで多くの分野をカバーする機能を備えた基本的なCRUDアプリケーションを構築します。

## Language
私たちはTypeScriptが大好きですが、何よりもNode.jsが大好きです。 そのため、Nestは*TypeScriptと純粋なJavaScript*の両方と互換性があります。 Nestは最新の言語機能を利用しているため、バニラJavaScriptで使用するには、*Babelコンパイラ*が必要です。

提供する例では主にTypeScriptを使用しますが、コードスニペットをバニラJavaScript構文にいつでも切り替えることができます（各スニペットの右上隅にある言語ボタンをクリックして切り替えるだけです）。

## 前提条件
Node.js（v13を除く> = 10.13.0）がオペレーティングシステムにインストールされていることを確認してください。

## セットアップ
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
main.ts

import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
```

Nestアプリケーションインスタンスを作成するには、コアの*NestFactoryクラス*を使用します。 NestFactoryは、アプリケーションインスタンスの作成を可能にするいくつかの静的メソッドを公開します。 *create（）メソッド*は、INestApplicationインターフェイスを満たすアプリケーションオブジェクトを返します。 このオブジェクトは、次の章で説明する一連のメソッドを提供します。 上記のmain.tsの例では、HTTPリスナーを起動するだけで、アプリケーションはインバウンドHTTPリクエストを待機できます。

Nest CLIでスキャフォールドされたプロジェクトは、開発者が各モジュールを専用のディレクトリに保持するという規則に従うことを奨励する初期プロジェクト構造を作成することに注意してください。

## プラットフォーム
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

基本的なコントローラーを作成するために、クラスとデコレーターを使用します。*デコレータ*はクラスを必要なメタデータに関連付け、Nestがルーティングマップを作成できるようにします（リクエストを対応するコントローラーに結び付けます）。  

*デコレータとは、関数の中身を書き換えずに関数を修飾するための仕組みです。
ライブラリ内の関数の機能を変更したいときなどに役立ちます。*  

## ルーティング
次の例では、基本的なコントローラーを定義するために必要な@Controller（）デコレーターを使用します。 catsのオプションのルートパスプレフィックスを指定します。 @Controller（）デコレータでパスプレフィックスを使用すると、関連するルートのセットを簡単にグループ化し、繰り返しのコードを最小限に抑えることができます。 たとえば、ルート/ customersの下の顧客エンティティとの相互作用を管理する一連のルートをグループ化することを選択できます。 その場合、@ Controller（）デコレータでパスプレフィックスcustomersを指定して、ファイル内のルートごとにパスのその部分を繰り返す必要がないようにすることができます。

```
cats.controller.ts

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

findAll（）メソッドの前の@Get（）HTTPリクエストメソッドデコレータは、HTTPリクエストの特定のエンドポイントのハンドラを作成するようにNestに指示します。 エンドポイントは、HTTPリクエストメソッド（この場合はGET）とルートパスに対応します。 ルートパスとは何ですか？ ハンドラーのルートパスは、コントローラーに対して宣言された（オプションの）プレフィックスと、リクエストデコレータで指定されたパスを連結することによって決定されます。 すべてのルート（cats）のプレフィックスを宣言し、デコレータにパス情報を追加していないため、NestはGET / catsリクエストをこのハンドラーにマップします。 前述のように、パスには、オプションのコントローラーパスプレフィックスと、リクエストメソッドデコレータで宣言されたパス文字列の両方が含まれます。 たとえば、デコレータ@Get（ 'profile'）と組み合わせたcustomersのパスプレフィックスは、GET / Customers / profileなどのリクエストのルートマッピングを生成します。

上記の例では、このエンドポイントに対してGETリクエストが行われると、Nestはリクエストをユーザー定義のfindAll（）メソッドにルーティングします。 ここで選択するメソッド名は完全に任意であることに注意してください。 ルートをバインドするメソッドを宣言する必要があることは明らかですが、Nestは選択したメソッド名に重要性を付けません。

このメソッドは、200ステータスコードと関連する応答（この場合は単なる文字列）を返します。 なぜそれが起こるのですか？ 説明するために、最初に、Nestが応答を操作するために2つの異なるオプションを採用するという概念を紹介します。

| オプション名 | 概要 |
|:-----------|------------:|
| Standard (recommended) | この組み込みメソッドを使用すると、リクエストハンドラーがJavaScriptオブジェクトまたは配列を返すと、自動的にJSONにシリアル化されます。 ただし、JavaScriptプリミティブ型（文字列、数値、ブール値など）を返す場合、Nestは値をシリアル化しようとせずに値だけを送信します。 これにより、応答の処理が簡単になります。値を返すだけで、残りはNestが処理します。

さらに、応答のステータスコードは、201を使用するPOSTリクエストを除いて、デフォルトでは常に200です。ハンドラレベルで@HttpCode（...）デコレータを追加することで、この動作を簡単に変更できます（ステータスコードを参照）。 |
| Library-specific | ライブラリ固有の（Expressなど）応答オブジェクトを使用できます。これは、メソッドハンドラシグニチャの@Res（）デコレータ（findAll（@Res（）応答）など）を使用して挿入できます。 このアプローチでは、そのオブジェクトによって公開されているネイティブの応答処理メソッドを使用できます。 たとえば、Expressでは、response.status（200）.send（）のようなコードを使用して応答を作成できます。 |

*Standardなら色々勝手にやってくれるってことっぽい。配列とオブジェクトは勝手にJSON化してくれ、文字列、数値、真偽値はそのまま返してくれる*

警告
Nestは、ハンドラーが@Res（）または@Next（）のいずれかを使用していることを検出し、ライブラリ固有のオプションを選択したことを示します。 両方のアプローチが同時に使用される場合、標準アプローチはこの単一ルートに対して自動的に無効になり、期待どおりに機能しなくなります。 両方のアプローチを同時に使用するには（たとえば、応答オブジェクトを挿入してCookie /ヘッダーのみを設定し、残りはフレームワークに任せる）、@ Res（{passthrough：true）でパススルーオプションをtrueに設定する必要があります。 }）デコレータ。

## Request object
多くの場合、ハンドラーはクライアントのリクエストの詳細にアクセスする必要があります。 Nestは、基盤となるプラットフォームのリクエストオブジェクトへのアクセスを提供します（デフォルトではExpress）。 @Req（）デコレータをハンドラの署名に追加して、リクエストオブジェクトを挿入するようにNestに指示することで、リクエストオブジェクトにアクセスできます。

```
cats.controller.ts

import { Controller, Get, Req } from '@nestjs/common';
import { Request } from 'express';

@Controller('cats')
export class CatsController {
  @Get()
  findAll(@Req() request: Request): string {
    return 'This action returns all cats';
  }
}
```

エクスプレスタイピングを利用するには（リクエスト：上記のリクエストパラメータの例のように）、@ types / expressパッケージをインストールします。

リクエストオブジェクトはHTTPリクエストを表し、リクエストクエリ文字列、パラメータ、HTTPヘッダー、および本文のプロパティがあります（詳細はこちらをご覧ください）。 ほとんどの場合、これらのプロパティを手動で取得する必要はありません。 代わりに、@ Body（）や@Query（）などの専用のデコレータを使用できます。これらはそのまま使用できます。 以下は、提供されているデコレータと、それらが表すプレーンなプラットフォーム固有のオブジェクトのリストです。

| デコレータ名 | 概要 |
|:-----------|------------:|
| @Request(), @Req() | req |
| @Response(), @Res()* | res |
| @Next() | next |
| @Session() | req.session |
| @Param(key?: string) | req.params / req.params[key] |
| @Body(key?: string) | req.body / req.body[key] |
| @Query(key?: string) | req.query / req.query[key] |
| @Headers(name?: string) | req.headers / req.headers[name] |
| @Ip() | req.ip |
| @HostParam() | req.hosts |

*基盤となるHTTPプラットフォーム（ExpressやFastifyなど）間での型指定との互換性のために、Nestは@Res（）および@Response（）デコレータを提供します。 @Res（）は、@ Response（）の単なるエイリアスです。 どちらも、基盤となるネイティブプラットフォームの応答オブジェクトインターフェイスを直接公開します。 それらを使用するときは、基盤となるライブラリ（@ types / expressなど）の型もインポートして、最大限に活用する必要があります。 メソッドハンドラーに@Res（）または@Response（）のいずれかを挿入すると、そのハンドラーに対してNestがライブラリ固有のモードになり、応答の管理を担当するようになることに注意してください。 その際、応答オブジェクト（res.json（...）やres.send（...）など）を呼び出して何らかの応答を発行する必要があります。そうしないと、HTTPサーバーがハングします。*

## Resources
以前、catsリソース（GETルート）をフェッチするエンドポイントを定義しました。 通常、新しいレコードを作成するエンドポイントも提供する必要があります。 このために、POSTハンドラーを作成しましょう。

```
cats.controller.ts

import { Controller, Get, Post } from '@nestjs/common';

@Controller('cats')
export class CatsController {
  @Post()
  create(): string {
    return 'This action adds a new cat';
  }

  @Get()
  findAll(): string {
    return 'This action returns all cats';
  }
}
```
とても簡単です。 Nestは、すべての標準HTTPメソッド（@Get（）、@ Post（）、@ Put（）、@ Delete（）、@ Patch（）、@ Options（）、および@Head（））のデコレーターを提供します。 さらに、@ All（）は、それらすべてを処理するエンドポイントを定義します。  

## Route wildcards

パターンベースのルートもサポートされています。 たとえば、アスタリスクはワイルドカードとして使用され、文字の任意の組み合わせに一致します。

```
@Get('ab*cd')
findAll() {
  return 'This route uses a wildcard';
}
```

'ab * cd'ルートパスは、abcd、ab_cd、abecdなどと一致します。 文字？、+、*、および（）は、ルートパスで使用でき、対応する正規表現のサブセットです。 ハイフン（-）とドット（。）は、文字列ベースのパスによって文字通り解釈されます。

## Status code

前述のように、レスポンスのステータスコードはデフォルトで常に200ですが、POSTリクエストは201です。ハンドラーレベルで@HttpCode（...）デコレーターを追加することで、この動作を簡単に変更できます。
```
@Post()
@HttpCode(204)
create() {
  return 'This action adds a new cat';
}
```
@ nestjs / commonパッケージからHttpCodeをインポートします。  

多くの場合、ステータスコードは静的ではありませんが、さまざまな要因によって異なります。 その場合、ライブラリ固有の応答（@Res（）を使用して挿入）オブジェクトを使用できます（または、エラーの場合は例外をスローします）。

## Headers
カスタムレスポンスヘッダーを指定するには、@ Header（）デコレータまたはライブラリ固有の応答オブジェクトを使用できます（そしてres.header（）を直接呼び出します）。

```
@Post()
@Header('Cache-Control', 'none')
create() {
  return 'This action adds a new cat';
}
```
@ nestjs / commonパッケージからHeaderをインポートします。

## Redirection
レスポンスを特定のURLにリダイレクトするには、@ Redirect（）デコレータまたはライブラリ固有のレスポンスオブジェクトを使用できます（そしてres.redirect（）を直接呼び出します）。

@Redirect（）は、urlとstatusCodeの2つの引数を取りますが、どちらもオプションです。 省略した場合、statusCodeのデフォルト値は302（Found）です。
```
@Get()
@Redirect('https://nestjs.com', 301)
```
HTTPステータスコードまたはリダイレクトURLを動的に決定したい場合があります。 これを行うには、ルートハンドラーメソッドから次の形状のオブジェクトを返します。
```
{
  "url": string,
  "statusCode": number
}
```

戻り値は、@ Redirect（）デコレータに渡された引数を上書きします。 例えば：
```
@Get('docs')
@Redirect('https://docs.nestjs.com', 302)
getDocs(@Query('version') version) {
  if (version && version === '5') {
    return { url: 'https://docs.nestjs.com/v5/' };
  }
}
```

## Route parameters
リクエストの一部として動的データを受け入れる必要がある場合、静的パスを持つルートは機能しません（たとえば、ID1のcatを取得するにはGET / cats / 1）。 パラメータを使用してルートを定義するために、ルートのパスにルートパラメータトークンを追加して、リクエストURLのその位置で動的な値をキャプチャできます。 以下の@Get（）デコレータの例のルートパラメータトークンは、この使用法を示しています。 この方法で宣言されたルートパラメータには、メソッドシグネチャに追加する必要がある@Param（）デコレータを使用してアクセスできます。

```
@Get(':id')
findOne(@Param() params): string {
  console.log(params.id);
  return `This action returns a #${params.id} cat`;
}
```

@Param（）は、メソッドパラメーター（上記の例ではparams）を装飾するために使用され、メソッドの本体内でその装飾されたメソッドパラメーターのプロパティとしてルートパラメーターを使用できるようにします。 上記のコードに見られるように、params.idを参照することでidパラメーターにアクセスできます。 特定のパラメータートークンをデコレーターに渡してから、メソッド本体でルートパラメーターを名前で直接参照することもできます。  

@ nestjs / commonパッケージからParamをインポートします。
```
@Get(':id')
findOne(@Param('id') id: string): string {
  return `This action returns a #${id} cat`;
}
```

## Sub-Domain Routing

@Controllerデコレータは、ホストオプションを使用して、着信したリクエストのHTTPホストが特定の値と一致することを要求できます。
```
@Controller({ host: 'admin.example.com' })
export class AdminController {
  @Get()
  index(): string {
    return 'Admin page';
  }
}
```
警告
Fastifyはネストされたルーターをサポートしていないため、サブドメインルーティングを使用する場合は、代わりに（デフォルトの）Expressアダプターを使用する必要があります。  

ルートパスと同様に、hostsオプションはトークンを使用して、ホスト名のその位置で動的な値をキャプチャできます。 以下の@Controller（）デコレータの例のホストパラメータトークンは、この使用法を示しています。 この方法で宣言されたホストパラメータには、@ HostParam（）デコレータを使用してアクセスできます。デコレータはメソッドシグネチャに追加する必要があります。
```
@Controller({ host: ':account.example.com' })
export class AccountController {
  @Get()
  getInfo(@HostParam('account') account: string) {
    return account;
  }
}
```

## Scopes

さまざまなプログラミング言語のバックグラウンドを持っている人にとって、Nestではほとんどすべてが着信リクエスト間で共有されていることを知るのは意外かもしれません。 データベースへの接続プール、グローバル状態のシングルトンサービスなどがあります。Node.jsは、すべてのリクエストが個別のスレッドによって処理されるリクエスト/レスポンスマルチスレッドステートレスモデルに準拠していないことに注意してください。 したがって、シングルトンインスタンスの使用は、アプリケーションにとって完全に安全です。

ただし、コントローラーのリクエストベースのライフタイムが望ましい動作である場合があります。たとえば、GraphQLアプリケーションでのリクエストごとのキャッシュ、リクエストトラッキング、マルチテナンシーなどです。 ここでスコープを制御する方法を学びます。

## Asynchronicity（非同期性）

私たちは最新のJavaScriptが大好きで、データ抽出はほとんど非同期であることを知っています。 そのため、Nestは非同期関数をサポートし、うまく機能します。
非同期/待機機能の詳細については、こちらをご覧ください  

すべての非同期関数はPromiseを返す必要があります。 これは、Nestがそれ自体で解決できる遅延値を返すことができることを意味します。 この例を見てみましょう：

```
cats.controller.ts

@Get()
async findAll(): Promise<any[]> {
  return [];
}
```

上記のコードは完全に有効です。 さらに、Nestルートハンドラーは、RxJSの監視可能なストリームを返すことができるため、さらに強力です。Nestは、下のソースに自動的にサブスクライブし、最後に発行された値を取得します（ストリームが完了すると）。

```
cats.controller.ts

@Get()
findAll(): Observable<any[]> {
  return of([]);
}
```
上記の両方のアプローチが機能し、要件に合ったものを使用できます。

## Request payloads
POSTルートハンドラーの前の例では、クライアントパラメーターを受け入れませんでした。 ここに@Body（）デコレータを追加してこれを修正しましょう。

ただし、最初に（TypeScriptを使用する場合）、DTO（データ転送オブジェクト）スキーマを決定する必要があります。 DTOは、データがネットワークを介して送信される方法を定義するオブジェクトです。 TypeScriptインターフェイスを使用するか、単純なクラスを使用して、DTOスキーマを決定できます。 興味深いことに、ここでクラスを使用することをお勧めします。 どうして？ クラスはJavaScriptES6標準の一部であるため、コンパイルされたJavaScriptでは実際のエンティティとして保持されます。 一方、TypeScriptインターフェースは変換中に削除されるため、Nestは実行時にそれらを参照できません。 パイプなどの機能は、実行時に変数のメタタイプにアクセスできる場合に追加の可能性を可能にするため、これは重要です。

CreateCatDtoクラスを作成しましょう。
```
create-cat.dto.ts

export class CreateCatDto {
  name: string;
  age: number;
  breed: string;
}
```
基本的なプロパティは3つだけです。 その後、CatsController内で新しく作成されたDTOを使用できます。

```
cats.controller.ts

@Post()
async create(@Body() createCatDto: CreateCatDto) {
  return 'This action adds a new cat';
}
```
## Handling errors
ここには、エラーの処理（つまり、例外の処理）に関する別の章があります。

## Full resource sample
以下は、利用可能なデコレータのいくつかを利用して基本的なコントローラを作成する例です。 このコントローラーは、内部データにアクセスして操作するためのいくつかのメソッドを公開します。

```
cats.controller.ts

import { Controller, Get, Query, Post, Body, Put, Param, Delete } from '@nestjs/common';
import { CreateCatDto, UpdateCatDto, ListAllEntities } from './dto';

@Controller('cats')
export class CatsController {
  @Post()
  create(@Body() createCatDto: CreateCatDto) {
    return 'This action adds a new cat';
  }

  @Get()
  findAll(@Query() query: ListAllEntities) {
    return `This action returns all cats (limit: ${query.limit} items)`;
  }

  @Get(':id')
  findOne(@Param('id') id: string) {
    return `This action returns a #${id} cat`;
  }

  @Put(':id')
  update(@Param('id') id: string, @Body() updateCatDto: UpdateCatDto) {
    return `This action updates a #${id} cat`;
  }

  @Delete(':id')
  remove(@Param('id') id: string) {
    return `This action removes a #${id} cat`;
  }
}
```







