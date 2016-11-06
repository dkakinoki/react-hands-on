# react-hands-on
## 事前準備
nodeのv4以上が動く状態にしておいてください。
あと、[ReactのDeveloperTools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en)をインストールしておいてください

### node環境がない人向け
#### A. [homebrewでnodeとnpmをインストール](http://bulblub.com/2013/04/20/install_nodejs_with_homebrew/)

node -v でnodeのバージョンが4以下の場合は、
`brew install homebrew/versions/node4-lts` とかしてみてください。([参考](http://qiita.com/kazuph/items/49db7cbaedd13afae3ec))

#### B. [nvm](http://qiita.com/544/items/7237a32c68619236f446)や[nodebrew](http://qiita.com/sinmetal/items/154e81823f386279b33c)でバージョン管理してもOK

バージョン管理ツールは併用できないので、どれかひとつにしてください。

ちなみに、node [v6が最近LTSとなった](https://github.com/nodejs/LTS)らしいです。

### es6がよくわからない人
[こちら](http://qiita.com/tuno-tky/items/74ca595a9232bcbcd727)でも一読しておいてください。

### Reactの予習
[おすすめ](http://qiita.com/advent-calendar/2014/reactjs)はこちらです。
公式のチュートリアルは、よく更新されて、今は日本語版がないので、ちょっとつらいかも。
そして個人的には、今のはわかりにくいと思う。


## 前提
- **es6** でReact.js書いてみましょう
- 環境構築の話しているとそれだけで大幅に時間取られるので、今日はしません。
 - Node.jsやRailsで環境構築する方法はQiitaにいっぱい載っているので、各々頑張ってください
- FluxやReduxとかも今日はやらないです。各自頑張ってください。
- サンプルコードは動作確認していないものもあるので、そのあたりもご勘弁を。

## 準備
gitから本日用のリポジトリをcloneして、npm installする
```
git clone https://github.com/dkakinoki/react-hands-on.git
cd my-app
npm install
```
起動してみる
```
npm start
```

## やること
### 概要(おさらい)
- facebookが開発したJavaScriptのライブラリ
- MVCの **View** を担当
- jsやjQueryが直接DOMを操作するのに対し、**仮想DOM** を介して差分でDOMを更新する
- データの流れは一方向

### render
#### renderとは
- Reactのコンポーネントを返す **省略できない** メソッド
- このメソッドにより、**仮想DOM** が作成される

```
# 仮想DOM？
- Reactのrender関数は、現在のスナップショットをもとに仮想的なページを生成する
- Reactを作成された仮想的なページをもとにDOMとの差分を計算し、更新内容をDOMに反映させる
```

![画像](http://www.funnyant.com/wp-content/uploads/2014/07/reactjs-virtual-dom.png)
- [参考](http://www.funnyant.com/reactjs-what-is-it/#Virtual DOM)

#### その他renderのルール
- 戻り値として、null,false,または、Reactコンポーネントを返す
- 単一のトップレベルの要素しか返すことができない

```
// OK
render() {
  return(
    <div id=main>
      <p>foo</p>
      <p>bar</p>
    </div>
  );
}

// NG
render() {
  return(
    <p>foo</p>
    <p>bar</p>
  );
}
```

#### ハンズオン
- App.jsを複数要素にしてエラーになるか試してみましょう


### jsx
#### jsxとは
- JavaScript XML
- HTMLに非常によく似たマークアップ言語
- 使わなくてもReactは利用出来るが、絶対に使った方がいい

```
// JSXの利点
- HTMLとほぼ同じなので、従来のテンプレートのように新規に文法を覚える必要がない
- HTMLとほぼ同じなので、見やすい
- 関心の分離(マークアップとロジック)
```
#### 動的な値
- {}で表現する
- {}内はJavaScriptの式として評価される

```
// 例

const title = "今日はReact.jsの勉強会です。";

<h1>{title}</h1>
```

#### ハンズオン
- 上記例をもとに、App.jsのh1に変数を渡して、表示させましょう
- {}内で関数を呼び出して、きちんと評価されるか確認しましょう


#### HTMLとの違い一例
- コメントは、`{/* */}`で表現する
- `class`ではなく、`className`
- labelの属性の`for`は`htmlFor`


### カスタムコンポーネント
#### コンポーネントを定義することのメリット
- 使いまわせる

```
// よくありそうなコンポーネント例
- APIから返却された値を入れて作るチェックボックスリストのコンポーネント
- エラー表示を行うinputタグのコンポーネント
- 0時~24時を選択するselectタグのコンポーネント
```

- 修正箇所の特定も簡単になる

#### コンポーネント使用例

```問い合わせフォームのコンポーネント例
import React from 'react';
import NameInputText from './NameInputText';
import EmailInputText from './EmailInputText';
import InquiryInputTextArea from './InquiryInputTextArea';

class InquiryFormView extends React.Component {
  constructor(){
    // 初期化処理
  }
  
  handleSubmit(){
    // 問い合わせフォームの送信処理
  }
  
  render() {    
    return (
      <form submit={this.handleSubmit.bind(this)}>
        <NameTextField />
        <EmailTextField />
        <InquiryInputTextArea />
      </form>
    );
  }
}
```

#### ハンズオン
- App2.jsを追加して、h1の表示部分をそちらで実装しましょう。

### props
- 単一方向のデータフローなので、親要素から子要素に渡すもの
- propsはコンポーネントにプロパティとして渡される
- `this.props` でアクセス可能
- 基本、read-only(コンポーネント内で変更しない)

```
<CountryDropDown listItems={countryList} /> 

// CountryDropDown内で、this.props.listItemsでアクセスできる
```

#### ハンズオン
App2.jsにタイトルをpropsで渡して表示させましょう

### state
- Reactは内部状態(state)をもつ
- stateが更新されると、DOMも更新される(renderが実行される)
- stateの更新は、this.setStateで更新する
- propsで渡ってきたプロパティをstateで管理する場合には、constructorでthis.stateで入れてあげるとよい

```
// selectタグが変更されると、stateのselectedValueが更新される

class CountryDropDown extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      selectedValue: this.props.listItems[0]
      listItems: this.props.listItems
    };
  }
  
  handleChangeOption(e) {
    e.preventDefault;
    
    this.setState({
      selectedValue: e.currentTarget.value
    });
  }
  
  handleSubmit(e) {
    e.preventDefault;
    
    alert(this.state.selectedValue);
  }
  
  render() {
    const options = this.state.listItems.map((item, idx) => {
      <option value={idx}>{item}</option>
    });
    
    return(
      <form onSubmit={this.handleSubmit.bind(this)}>
        <select onChange={handleChangeOption.bind(this)}>
         {options}
        </select>
        <button type="submit">送信</button>
      </form>
    );
  }
}
```

#### ハンズオン
- 以下を実装してください
 - App2.jsにinput(type=text)のformを作成
 - テキストの初期値はpropsでApp.jsから渡す
 - テキストの内容を更新すると、stateが更新される
 - 送信ボタンをクリックすると、現在のstateの値をalertで出力する
- ヒント
 - inputタグはjsxでは、`<input></input>`ではなく、`<input />`と書く必要があります
 
 
```わからない人向けの回答例
import React from 'react';

class App2 extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      textValue: this.props.initTitle
    };
  }
  
  handleChangeText(e) {
    e.preventDefault;
    
    this.setState({
      textValue: e.currentTarget.value
    });
  }
  
  handleSubmit(e) {
    e.preventDefault;
    
    alert(this.state.textValue);
  }
  
  render() {    
    return (
      <form onSubmit={this.handleSubmit.bind(this)}>
        <input type="text" onChange={this.handleChangeText.bind(this)} value={this.state.textValue} />
        <button type="submit">送信</button>
      </form>
    );
  }
}

export default App2;
``` 
  
### コンポーネントライフサイクル
- コンポーネントの作成、実行、破棄といったライフサイクルにおけるイベントごとに処理を登録できる
- [こちら](http://qiita.com/koba04/items/66e9c5be8f2e31f28461)がわかりやすい
- [公式ドキュメント](https://facebook.github.io/react/docs/react-component.html)もわかりやすい

#### ハンズオン
- `componentDidMount()`を使って、console.logで何かを出力してみましょう。
