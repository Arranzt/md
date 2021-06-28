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






