# react-hands-on
## 事前準備
nodeのv4以上が動く状態にしておいてください。
あと、[ReactのDeveloperToolsをインストールしておいてください](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en)

### node環境がない人向け
#### A. homebrewでnodeとnpmをインストール
http://bulblub.com/2013/04/20/install_nodejs_with_homebrew/

node -v でnodeのバージョンが4以下の場合は、
`brew install homebrew/versions/node4-lts` とかしてみてください。
http://qiita.com/kazuph/items/49db7cbaedd13afae3ec

#### B. nvmやnodebrewでバージョン管理してもOK
http://qiita.com/544/items/7237a32c68619236f446
http://qiita.com/sinmetal/items/154e81823f386279b33c

バージョン管理ツールは併用できないので、どれかひとつにしてください

ちなみに、node v6が最近LTSとなったらしいです
https://github.com/nodejs/LTS

## 前提
- es6でReact.js書いてみましょう
- 環境構築の話しているとそれだけで大幅に時間取られるので、今日はしません
 - Node.jsやRailsで環境構築する方法はQiitaにいっぱい載っているので、各々頑張ってください

## 準備
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
### render
#### renderの説明

### jsx
#### jsxの説明
#### ちょっと独特なところの説明

### Component
#### Component化することのメリット
- 使いまわせる
- props渡せる、状態管理
- App2.js足してみる

### props
- App2.jsにprops渡す
- read-only
- 単一方向のデータフロー
 - ツリーの上から下に

### state
- stateが更新されると、DOMも更新される(renderが実行される)
- App.jsのstateをApp2.jsのpropsに渡す
- App2.jsのstateを更新する
 - this.setState()

### Component Life Cycle
- Appのpropsが変わったら、App2のRecievedPropsでsetState

### HandlingEvents
- Event bind
- preventDefault

### アンチパターン
- idしていして、値を変更
- stateで管理できなくなるからダメ

