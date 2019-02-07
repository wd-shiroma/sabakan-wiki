<!-- TITLE: ノベル投稿サイトとして利用する -->
<!-- SUBTITLE: カスタムCSSを利用して、記事を小説風のレイアウトに変更します。 -->

# カスタムCSSを追加する

結論から。ブログの管理ページにあるカスタムCSSを登録すればよいだけ。

以下、CSSサンプル

```
body#post article {
    margin: 0em !important;
    max-width: none;
}

.p-name {
    text-align: center;
}

.e-content {
    width: 100%;
    column-gap: 5em;
    -webkit-writing-mode: vertical-rl;
    -ms-writing-mode: tb-rl;
    writing-mode: vertical-rl;
    column-span: all;
}

.e-content p {
    padding: 0em;
    margin: 0em 1em;
}

body#post .e-content {
    height: 30em;
    column-width: 30em;
}

body#collection .e-content {
    height: 15em;
    column-width: 15em;
}

body#collection section {
    overflow-y: hidden;
}

body#post footer {
    width: 20em;
    margin-right: -18em;
    text-align: left;
    position: fixed;
    top: 0;
    right: 0;
    transition: all 300ms 0s ease;
    padding: 0em;
}

body#post footer:hover {
    margin-right: 0em;
}

body#post footer hr {
    background: none;
    margin: 0;
}

body#post footer nav {
    background-color: #f5f5f5;
    margin: 0;
    padding: 0.5em;
    border-radius: 0.5em;
}

body#post footer nav p:before {
    vertical-align: middle;
    margin-right: 0.5em;
    content: url('https://cdnjs.cloudflare.com/ajax/libs/twemoji/11.3.0/16x16/2755.png');
}
```

## 各ブロック解説

### 記事全般の設定

```
body#post article {
    margin: 0em !important;
    max-width: none;
}
```

記事の表示領域を画面幅いっぱいのサイズにしています。
左右に余白を作りたい場合は `margin: 0em 5em !important;` としてみてください。
最大幅を指定したい場合は、 `margin: 0em auto !important;` `max-width: 640px;` としてみてください。

```
.p-name {
    text-align: center;
}
```

記事タイトルを中央揃えにします。

### 本文の設定

```
.e-content {
    -webkit-writing-mode: vertical-rl;
    -ms-writing-mode: tb-rl;
    writing-mode: vertical-rl;
    width: 100%;
    column-span: all;
    column-gap: 5em;
}
```

縦書きにするための設定です。
`～writing-mode` が各ブラウザごとの縦書きスタイルを指定する設定です。
`width: 100%;` で段組みの幅を指定し、`column-spam: all;` とすることで次の段組みに移行します。
段組み間の余白は `column-gap: 5em` で指定しています。

```
.e-content p {
    padding: 0em;
    margin: 0em 1em;
}
```

段落の行間の余白を設定します。
WriteFreelyでは、一般的なマークダウンを使用しているため、改行を2回挟むことで段落を作ることが出来ます。

```
body#post .e-content {
    height: 30em;
    column-width: 30em;
}
```

記事個別のページにおける1行毎の文字数上限を設定します。
スマートフォンで記事を表示する場合、30文字はギリギリか、少しはみ出す程度の文字数です。

```
body#collection .e-content {
    height: 15em;
    column-width: 15em;
}
```

記事一覧ページにおける1行毎の文字数上限を設定します。
縦の表示を長くしたくないため、少なめに設定しています。
本文を表示したくない場合は `display: none;` を指定します。

```
body#collection section {
    overflow-y: hidden;
}
```

記事一覧ページの本文を、最初の1段組目だけ表示させます。

```
body#post footer {
    width: 20em;
    margin-right: -18em;
    padding: 0em;
    text-align: left;
    position: fixed;
    top: 0;
    right: 0;
    transition: all 300ms 0s ease;
}
```

CSSの仕様上、縦書きの段組み表示スタイルに変更した場合、フッターを最下部へ持っていくことが出来ません。
そのため、フッターを画面右上に固定表示するように位置を変更してあげます。
スクロールしても画面に追随せずに、ページの右上に固定表示したい場合は、 `position: absolute;` に変更してください。
`transition` でカーソルホバー時のアニメーション設定をしています。

```
/* フッターにカーソルを合わせると表示される */
body#post footer:hover {
    margin-right: 0em;
}
```

右上に隠したフッターにカーソルを合わせると、左にスライドして内容が表示されるようになります。

```
body#post footer hr {
    background: none;
    margin: 0;
}
```

標準で表示されているフッターの区切り線を非表示にします。

```
body#post footer nav {
    background-color: #f5f5f5;
    margin: 0;
    padding: 0.5em;
    border-radius: 0.5em;
}
```

フッターの背景を設定します。

```
body#post footer nav p:before {
    vertical-align: middle;
    margin-right: 0.5em;
    content: url('https://cdnjs.cloudflare.com/ajax/libs/twemoji/11.3.0/16x16/2755.png');
}
```

フッターを右上に隠れている時に表示されるアイコンを設定しています。
CloudFlareのCDNからTwemojiの感嘆符（！）画像を取得しています。

## WriteFreely標準のスタイルとして縦書きを採用する

サーバのスタイルシートを編集して `make ui` をすれば組み込めそうだけど未検証のため追加情報求む

# 縦書きをする上での注意点

## 英数字を縦書きにする

半角英数字を縦書きにすると、文字が右に90度回転してしまい、寝そべった表示になってしまいます。
これを解消するには、2通りの方法があります。

* 全角英数字で入力する
全角英数字は横に倒れないで縦の表示になります。
* CSSを設定する
縦書き設定をしているブロックに `text-orientation: upright;` を設定します。

参考：[CSSで縦書き表記にする方法 ｜ UQUICO.com](http://uquico.com/coding/writing-mode/)

## 三点リーダを真ん中に持ってくる

普通に三点リーダを入力すると、「……」のように、真ん中に表示されません。
文字コードが異なる三点リーダを利用することで、真ん中に持ってくることが出来ます。
「⋯⋯」←こちらの文字を辞書登録するなりすると良いでしょう。