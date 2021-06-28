### 環境構築

```
Node.jsのインストール
node v6以上

create-react-appのインストール
$ npm install -g create-react-app
```

### アプリケーションの作成と起動

```
$ npx create-react-app アプリ名

$ cd アプリ名
$ npm start
もしくは
$ yarn start
```

### JSX

```
①スコープにReactが必要
JSXはReact.createElement関数の呼び出しとして変換されることから、
import React from 'react';
を指定しなければならない

②式の埋め込み
{}

③空要素は必ず閉じること
/
```

### JSXとJavascriptの比較

```
JSX
const element = <h1 className="heading">Hello, JSX</h1>;

JS タグ→属性→子要素の順
const element = React.createElement(
  "h1",
  { className: "heading" },
  "Hello, JSX"
);
```

### React.createElement関数を利用する場合
属性や子要素が全く同じで、タグ名だけ変更したい場合
```
const level = 3;
const heading = React.createElement(`h${ level }`, ...)
```

### 配列の展開

```
const list = (
  <ul>
    {[1, 2, 3].map(num => <li>{ num }</li>)}
  </ul>
)
```

### Functional Component
```
import React from 'react';

const Hello = (props) => {
  return <div>こんにちは、{ props.name }さん</div>;
}
```

### Class Component
```
class Hello extends React.Component {
  render() { 
    return <div>こんにちは、{ this.props.name }さん</div>;
  }
}
```

### Reactエレメントの利用
Reactコンポーネントを元に作られた実体のことをReactエレメントと呼ぶ
```
Reactコンポーネント
const Hello = () => {
  return <div>こんにちは、トイレの花子さん</div>
}

Reactエレメント
ReactDOM.render(
  <div>
    <Hello />
    <Hello />
    <Hello />
  </div>,
  document.getElementById('root')
)
```

### React.Fragmentコンポーネント
Reactが提供する特殊コンポーネントで、HTMLとしての要素を持たない
```
const Hello = () => {
  return (
    <React.Fragment>
      <div>こんにちは、トイレの花子さん</div>
      <div>こんにちは、トイレの太郎くん</div>
    </React.Fragment>
  );
};

簡略記法
<>
</>
```

### propsの利用
propsはobject型
```
const Hello = (props) => {
  return (
    <div>こんにちは、{ props.name }さん</div>;
  );
};

ReactDOM.render(
  <>
    文字列の受渡しは、{}で囲むかそのまま書く
    <Hello name="トイレの花子さん" />
    <Hello name="テケテケ" />
    <Hello name={ "桐生会" } />
    
    数値、真偽値の受渡しは{}で囲む
    <Hello number={ 42 } />
    <Hello boolean={ true } />
    
    配列の受渡し
    <Hello array={['a', 'b', 'c']} />
    
    オブジェクトの受渡し
    <Hello object={{ name: 'a', date: '2021/06/28' }} />
    
    関数の受渡し
    <Hello function={(name) => console.log(name)} />
    
    変数の受渡し
    const name = "a";
    <Hello value={ name } />
  </>,
  document.getElementById("root");
);
```






