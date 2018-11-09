<!-- TITLE: Hubzillaの基本情報 -->
<!-- SUBTITLE: Hubzillaの基本的な情報です -->


Hubzillaは分散型プラットフォームを利用した多彩なサービスを提供するためのフリーソフトウェア、またこれが提供する連合型のサービスである。

https://project.hubzilla.org/page/hubzilla/hubzilla-project

# 機能の概略
* Google+風ポスト/コメントで構成されるSNS
* 使用プロトコルはzot、その他ユーザーがアプリを追加することでActivitypub(Mastodon系)、diaspora、OStatus(GNU Social系)に相互に接続、会話することができる
* BBCodeやMarkdownを利用した文字装飾
* SNS以外にもカレンダーやWebDAV(簡易クラウドストレージ)、ブログ作成、webページ作成等が可能
* 独自の"ノマディックアイデンティティ(遊牧民のアイデンティティ)"によりサーバーに縛られない柔軟な運用が可能
# ノマディックアイデンティティとは
* zotに搭載される最大の機能。
* 一般的にサービスを利用する側は提供する側にアカウントを作り、そこに束縛されておりデータのお引っこしなどはできない。
* zotではこれを解決するために、"アカウントとユーザーデータの分離"をしている。
* 登録するメールアドレスやパスワードはそれぞれのサーバーに必要だが、中のユーザーデータは必ずしも別ではないということ。
* そうするとユーザーが鯖Aにアカウントとユーザーデータを作成し、そのデータを鯖Bに全て持っていくことができるようになる。
* この利点は他の鯖にユーザーデータのミラーリング等ができるためサーバーが停止した時でもユーザーは問題無くミラー先でSNSを使用し続けることができることだ。
* 例 : [ここ](https://plus.haruk.in/channnel/harukin)と[ここ](https://start.hubzilla.org/channel/harukin6323)と[ここ](https://gerzilla.de/channel/harukin)は見た目同じに見えるが全て別のサーバーであり、どのサーバーにログインして投稿したとしてもどのサーバーにも反映される。サーバーがメンテナンス時等アクセスできないタイミングでもユーザーはミラー先でSNSを利用できる、ということなのだ。
# 推薦動作環境

* Ubuntu 18.04

# Hubzillaの入手
* [GitLab(ソースコード)](https://framagit.org/hubzilla/core/)

# 公式ドキュメント
* [Hubzilla Development](https://project.hubzilla.org/page/hubzilla/hubzilla-project)

# 稼働中の日本向けサーバー
* [Harukin+!](https://plus.haruk.in) ボッチで寂しいからみんな建てて！(懇願)
