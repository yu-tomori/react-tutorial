# React Guide

## JSXの導入
```JavaScript
const element = <h1>Hello, world!</h1>;
```
- JSXはJavaScriptの機能を全て備えている。

### JSXを使う理由
- 表示のためのロジックは、イベントへの応答や刑事的な状態の変化、画面hy9王子のためのデータを準備する方法といった、他のUIロジックと本質的に結合したものであり、Reactはその事実を受け入れます。
- マークアップとロジックを別々のファイルに書いて人為的に技術を分離するのではなく、Reactはマークアップとロジック両方を含む疎結合の「コンポーネント」という単位を用いて関心を分離します。

### JSXに式を埋め込む
- 以下の例では、nameという変数を宣言して、それを中括弧に囲んでJSX内で使用しています。

```JavaScript
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

- あらゆる有効なJavaScriptの式をJSX内で中括弧に囲んで使用できます。
- 以下の例では、formatName(user)というJavaScript関数の結果を\<h1>要素内に埋め込んでいます。

```JavaScript
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
}

const element = {
  <h1>Hello, {formatName(user)}!</h1>
};

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

- 読みやすさのためにJSXを複数行に分けています。必須ではありませんが、複数行に分割する場合には、自動セミコロン挿入の落とし穴にはまらないようにカッコで囲むことをお勧めします。

### JSXもまた式である
- コンパルの後、JSXの式は普通のJavaScriptの関数呼び出しに変換され、JavaScriptオブジェクトへと評価されます。
- これはJSXをif文やforループの中で使用したり、変数に代入したり、引数として受け取ったり、関数から返したりすることができるということです。

```JacaScript
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

### JSXで属性を指定する
- 文字列リテラルを属性として指定するために引用符を使用できます。

```JavaScript
const element = <div tabIndex="0"></div>;
```

- 属性にJavaScript式を埋め込むために中括弧を使用することもできます。

```JavaScript
const element = <img src={user.avatarUrl}></img>;
```

- 属性にJavaScript式を埋め込むときに中括弧を囲む引用符を書かないでください。
- (文字列の場合は)引用符、もしくは(式の場合は)中括弧のどちらか一方を使用するようにし、同じ属性に対して両方を使用するべきではありません。
- JSXはHTMLよりもJavaScriptに近いものですので、ReactDOMはHTMLの↓奥性ではなくcamelCaceのプロパティ命名規則を使用します。JSXでは例えば、<font color="Green">class</font>は<font color="Green">className</font>となり、<font color="Green">tabindex</font>は<font color="Green">tabIndex</font>となります。

### JSXで子要素を指定する
- ヤグがからの場合、XMLのように />でタグを閉じることができます。

```JavaScript
const element = <img src={user.avatarUrl} />;
```

- JSXのタグは子要素を持つことができます。

```JavaScript
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

### JSXはインジェクション攻撃を防ぐ

- JSXにユーザの入力を埋め込むことは安全です。

```JavaScript
const title = response.potentiallyMaliciousInput;
// This is safe
const element = <h1>{title}</h1>;
```

- デフォルトでは、ReactDOMはJSXに埋め込まれた値をレンダー前にエスケープします。このため、自分のアプリケーションで明示的に書かれてものではないあらゆるコードは、注入できないことが保証されます。
- レンダー前の全てが文字列に変換されます。これはXSS攻撃の防止に役立ちます。

### JSXはオブジェクトの表現である
- BabelはJSXをReact.createElement()の呼び出しへとコンパルします。以下の2つの例は等価です。

```JavaScript
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

```JavaScript
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

- React.createElement()はバグの混入を防止するために行くつかのチェックを行いますが、本質的には以下のオブジェクトを生成します。

```JavaScript
// Note: this structure is simplified
const element = (
  type: 'h1',
  props: (
    className: 'greeting',
    children: 'Hello, world!'
  )
);
```

- このようなオブジェクトは"React要素"と呼ばれます。これらは画面に表示したいものの説明書きとして考えることができます。 
- Reactはこれらのオブジェクトを読み取り、DOMを構築して最新に保ちます。

## 要素のレンダー

