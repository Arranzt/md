
## TypeScriptとは
TypeScriptは、マイクロソフトが開発した静的型付け言語です。

静的型付け言語に分類されるプログラミング言語（CやJava）があることに対して動的型付け言語に分類されるプログラミング言語（Rubyや、Python、PHP、JavaScript）があります。
静的型付け言語はコンパイル時に型が決定することに対して、動的型付け言語は実行時に型が決定します。

コンパイルとは、コンピューターが実行できる機械語（バイナリコード）もしくは元のプログラムよりも低い水準のコードに変換をすることを指します。
コンパイルをしてくれるツールやプログラムのことをコンパイラと呼びます。

TypeScriptのコンパイラには、tscがあります。
開発時はnpmパッケージのtypescriptにtscが含まれているのでこちらをインストールして開発するのが主流となっています。
typescriptは、コンパイル時のルールをtsconfig.jsonに記述しプロジェクト内で管理し、プロジェクト内の同一ルールを設定することもできます。

## どんなプロジェクトで使用されているのか？

tscは、TypeScriptをJavaScriptに変換します。
TypeScriptコードの変更をフックにコンパイルの実行を設定することも可能です。

TypeScript誕生の背景にはJavaScriptの歴史が密接に関わっています。
JavaScriptは当初、簡単なアニメーションやフォームのバリデーションを行うくらいのHTMLの補助的な位置づけに過ぎず、大規模な開発を想定した言語設計にはなっていませんでした。
しかしJavaScriptを使用して開発されたWEBアプリケーションとしてGoogle Mapsが公開されたことによりJavaScriptは再注目されるようになりました。
そんな経緯があり、大規模な開発にも耐えうるようにと2012年に誕生したプログラミング言語がTypeScriptです。
※TypeScriptはAltJSの一種で最終的にはJavaScriptにコンパイルされて使用されます。

先述したように、TypeScriptは大規模開発によく用いられます。
TypeScriptを導入している事例としては以下があげられます。

Yahoo! JAPAN トップページを Atomic Design と React・Redux・TypeScriptで作り変えたお話
120億PVの巨大サービス「LINE NEWS」をTypeScript化した話
「TypeScript」を使わない手はない READYFORのUIリニューアルを支えたフロントエンドの技術

上記の事例にあったように、コンパイラのルール次第では、JavaScriptで導入しているプロジェクトを、段階的に移行してくことも可能です。

## TypeScriptの特徴
JavaScriptに任意の型システムを追加すること
※TypeScriptを導入する理由として一番にあげられる傾向があります
ES6以降の多くの機能を使うことができる（クラス構文やモジュール構文等）
はじめに、型システムの一部を紹介します。

## 型アノテーション
さっそく簡単なコードに触れてみたいと思います。
コンパイルする環境を構築する前に、playgroundが用意されているのでこちらを利用しましょう。

*
annotation アノテーションについて
あるデータに対して関連する情報（メタデータ=型）を注釈として付与すること
*

```
function sayAnything(message: string): string 
{
  return message
}

const sayHello: string = sayAnything('Hello TypeScript');
console.log(sayHello);
```
上記のコードがJavaScriptのコードと違う点としては、

sayAnything関数の仮引数名messageという記述の後に、: stringという記述がある
sayAnything関数の閉じかっこ)という記述の後に、: stringという記述がある
sayHello変数名を記述した後に、: stringという記述がある
これらが型情報になります。
型情報を記述することを、型アノテーションといいます。
stringというのは、JavaScriptでいうプリミティブ型のString型を示しています。

1.でいうと、message引数には、string型を指定してくださいということを型アノテーションしています。
2.はsayAnything関数の戻り値はstring型を返してくださいということを型アノテーションしています。
3.はsayHello変数にはstring型を代入してくださいということを型アノテーションしています。

簡単な例ですが、この時点でsayAnything関数がどのような使われ方を期待しているか、型情報を見るだけでJavaScriptで定義するときより把握しやすくなっていることがわかります。
試しにsayAnything関数の引数に対して1と指定してみましょう。
赤い波線が表示されマウスホバーしてみると、下記メッセージが記載されているツールチップが表示されると思います。
```
Argument of type 'number' is not assignable to parameter of type 'string'
```
引数には、number型ではなく、string型で指定してくださいという内容のエラーが表示されました。

JavaScriptで同じようなコードを記述すると、関数単体だとそもそもString値を引数に設定すべきかどうかわからないので、誤った使われ方をしてしまうことにもつながっていますね。
JavsScriptにはこういったことを回避するためにJSDOCのルールに基づきコメントアウトを用いて型アノテーションできますが、あくまでコメントアウトなので実装に対してコメントを残すことを強制することができません。

強制するとしたら、たとえばeslint-plugin-jsdocのようなプラグインを導入して、gitのpercommitフックでLint走らせてエラーでたらコミットさせないというような仕組みも別途用意する必要があります。

プロダクトレベルのコードは上記の例のようにシンプルなコードで構成されているわけではないので、定義されている関数の中身を追って推測したり、実際に関数がコールされている箇所を確認して推測したり、正しい使い方を確認するためにコストをかけると思います。

TypeScriptの場合は先ほど表示されたエラーのようにJavaScriptよりかなり早い段階でエラーを検出できていることにお気づきでしょうか。
型のチェックが入ることにより、エラーの早期発見につながり結果開発が向上します。

## 型推論

上記のサンプルは、あくまで例として表記しているので、過剰に型アノテーションしています。
実際にコーディングする際は下記の型アノテーションで、型のチェックに関して言えば十分です

```
function sayAnything(message: string) {
  return message
}

const sayHello = sayAnything('Hello TypeScript');
console.log(sayHello);
```

playground上でsayAnything関数にマウスホバーしてみると、

```
function sayAnything(message: string): string
```

と表示されていることがわかります。
sayAnything関数に型アノテーションしていないのに、ツールチップでは表示されているのかというと、
TypeScriptがsayAnythingの戻り値messageの型アノテーションをもとに戻り値の型を推測しているからです。

sayHello変数にもマウスホバーしてみてください。

```
const sayHello: string
```

とツールチップで表示されているかと思います。
こちらもTypeScriptがsayAnythingの戻り値messageの型アノテーションをもとに戻り値の型を推測してくれています。
この機能を型推論と呼び、そのうち一部のルールを紹介しました。
型推論を理解することで不要な型アノテーションは書かずに型チェックの恩恵を受けることができます。

## ES6+のJavaScriptを使用可能
```
const increment = (x) => x + 1;
```

気づいた方もいるかもしれませんが、処理の中で() => {}のような最新のJavaScriptの記法（ES6の記法）で記述されていると思います。「TypeScriptの特徴」として先述しましたが、TypeScriptではES6+記法を用いて記述が可能になっています。

## 構造的型付け
TypeScriptのオブジェクトの型チェックは、構造的型付けというスタイルになっています。
下記サンプルをplayground上で書いてみましょう。

```
interface IDuck {
  quack: Function;
  walk: Function;
}

interface IPenguin {
  coldResistance: true;
  quack: Function;
  walk: Function;
}

const penguin: IPenguin = {
  coldResistance: true,
  quack: () => {
    console.log('ガァー');
  },
  walk: () => {
    console.log('ペタペタ');
  }
}

const duck: IDuck = penguin;
```
オブジェクトの構造を定義するために、interfaceというTypeScriptの機能を使用しています。
duck変数には、IDuckという型アノテーションをしていますが、IPenguinで型アノテーションしているpenguin変数を代入しても、エラーは出ません。
なぜかというと、TypeScriptのオブジェクトは、構造があっているかどうかで型チェックをしているからです。

この特徴を構造的型付け（Structural Subtyping）といいます。

これに対して、JavaやSwiftは、型の名称でチェックしており、記名的型付け（Nominal Subtyping）といいます。

## よくやること・できること
特徴として「型」について触れましたが、「型」を定義することでこんなメリットが得られます。

コードの静的チェックが可能
JavaScriptは動的型付け言語に分類され、その性質上プログラムを実行してはじめて正しく動作するのか確認が可能になります。しかし、型があることで静的チェックが可能、つまりプログラムを実行せずともコードの欠陥を発見することができるようになります。コーディング中にエラーを発見できるような仕組みがあれば開発の規模が大きくなっても安心して開発に集中できますよね。
可読性向上
エディターの補完機能を活かせる
次のセクションから具体的に見ていきましょう。


