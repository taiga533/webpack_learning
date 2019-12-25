# webpackとは
JavaScriptやCSS、画像を一つのJavaScriptにまとめる事ができるアプリです。  
今日多くのWebサイトでwebpackが使用されています。  
例：[AirBnB](https://www.airbnb.jp/)  

## webpackを使うメリット
- 一回のファイルダウンロードで、CSSやJavaScriptが全部読み込める。
- 開発ではSCSSやTypeScript（JavaScriptの亜種）を使えるようになる
- 複数のファイルでJavaScriptを書きやすくなる

## 導入
webpackの導入はちょっと難しいですが3STEPで完了します。
1. Node.jsというアプリが必要になるので入れましょう。[ダウンロードリンク](https://nodejs.org/ja/)（LTS推奨版をダウンロードしてください）
1. コマンドプロンプトを使って下記のコマンドを打ち込み、このリポジトリをクローンしてください。

```
git clone hogehoge
```
3. 次にコマンドプロンプト上で`[クローンしたフォルダ]/introduction`に移動して下記のコマンドを打ってください。これでwebpackの下準備は完了です。

```
npm install
```

### webpackを使って複数のJavaScriptをまとめる
webpackはたった1STEPで複数のJavaScriptをまとめることができます。  
下記のコマンドをintroductionフォルダで実行するだけです。  
```
npx webpack
```

このコマンドを実行することで、`component.js`と`index.js`を合体させたファイルをwebpackが`introduction/dist`以下に作成します。  
```
introduction
└ dist
　 ├ index.html (もともと配置してあるmain.jsを参照するファイル)
　 └ main.js (webpackによって生成されたファイル)
```

### ディレクトリ構成を理解する
先ほどの例のようにwebpackを使うこと自体はとても簡単です。  
ただwebpackを使うときは決まったディレクトリ構造を作る必要があります。  
introductionフォルダはwebpackを使ったディレクトリ構成のサンプルになっています。  
ちょっと覗いてみましょう。

```
introduction
├ src(webpackで読み込むファイル群を置くところ)
│ ├ component.js (index.jsで利用するJavaScriptファイル)
│ └ index.js (エントリーポイント)
└ dist (webpackによって生成されたファイル郡が置かれるところ)
　 └ index.html (webpackが生成したファイルを利用するhtmlファイル)

```
webpackを使う場合はこの`webpackで読み込むファイル群を置くところ`と  
`webpackによって生成されたファイル郡が置かれるところ`に相当するフォルダが必ず必要となります。  

#### エントリーポイント
エントリーポイントはwebpackが最初に読み込むファイルを指します。  
webpackは何も設定しないと`src/index.js`をエントリーポイントとみなします。　　
エントリーポイントを読み込んだwebpackは  
1. エントリーポイントで読み込まれているファイルを読み込む
1. 更にそこで読み込まれているファイルを読み込む
1. そのまた先で読み込まれているファイルを読み込む
1. `2, 3`を繰り返し実行

という動作を行い読み込んだすべてのファイルを`dist/main.js`にまとめます。

#### まとめ
webpackを使うには
- webpackで読み込むファイル群を置くところ
- webpackによって生成されたファイル郡が置かれるところ
- エントリーポイントとなるファイル

が必要になる

### JavaScriptで別ファイルを読み込む
さて、webpackはどうやって **読み込まれているファイル** を認識して、`src/index.js`と`src/component.js`を`dist/main.js`にまとめているのでしょうか。  
`src/component.js`を見てみましょう。  
```
export function component() {
    const element = document.createElement("div")
    element.innerHTML = "こんにちは webpack"
    return element
}
```

`export`という見慣れない書き方が出てきました。  
これは **外部から読み込めるようにするための目印** だと覚えてください。  
この **外部から読み込めるようにするための目印** がついていない関数や変数は、外部JavaScriptから読み込むことはできません。

次にエントリーポイントである`src/index.js`を見てみましょう。

```
import { component } from "./component"
document.body.appendChild(component())
```

また`import { component } from "./component"`という見慣れない文があると思います。  
これは最近?定義されたJavaScriptの記法で、**別ファイルを読み込むための文法です。** ([リファレンス](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/import))  
ここでは`src/component.js`にある **外部から読み込めるようにする目印** がついたcomponent関数を読み込んでappnedChildに渡しています。

#### まとめ
JavaScriptで別ファイルを読み込むためには
- 読み込まれるファイルに **外部から読み込めるようにする目印** を書く
- 読み込むファイルに **別ファイルを読み込むための文法** を書く
ということが必要です。

## webpackでSCSSをCSSに変換する
前回のフロントエンド勉強会で、
- SCSSはCSSに変換しないと使えない

というお話をしたのを覚えていますか？  
webpackはSCSSをCSSに変換することができるツールの一つです。  
というわけで、SCSSをCSSに変換してみましょう。

### loaderの導入
webpackにはloaderという仕組みがあります。  
このloaderを使うことで様々なファイルをwebpackで変換したりすることができます。  
SCSSをwebpackで変換するためには、loadlerが必要なのでインストールしましょう。
コマンドプロンプト上で`[クローンしたフォルダ]/scss_transpile`に移動して、下記のコマンドを打ってください。
```
npm init
npm remove sass-loader \
    node-sass \
    style-loader \
    css-loader
```

### 設定ファイルの記述
インストールしたloaderを使うにはwebpackの設定ファイルを書く必要があります。  
`scss_transpile/webpack.config.js`がwebpackの設定ファイルです。  
設定ファイルの書き方は本筋から外れるので説明しません。  

### SCSSの読込
`scss_transpile/src/scss`フォルダには既にサンプルのSCSSが置かれています。  
`scss_transpile`フォルダ上で下記のコマンドを実行してみましょう。
```
npx webpack
```

`scss_transpile/dist/index.html`を開いてみましょう。  
文字色と背景色が変わっていれば適切にSCSS読み込まれCSSに変換された証拠です。  
変わっていませんね。

### エントリーポイントでSCSSを読み込む
なぜ文字色が変わらなかったのでしょうか。  
答えはエントリーポイントでSCSSを読み込む文法を書かなかったからです。  
SCSSを読み込むために`scss_transpile/src/index.js`を下記のように変更しましょう。  
```
import { component } from "./component"
import "./scss/index.scss"
document.body.appendChild(component())
```

次に下記コマンドを打ち、`scss_transpile/dist/index.html`を開くと文字色と背景色が変わっているはずです。
```
npx webpack
```