<!-- TITLE: Addon -->
<!-- SUBTITLE: hubzillaのアドオンリストを紹介します。 -->

# アドオン？？
hubzillaではアドオンを追加することによって機能を強化することができます。
ここでは公開されているリポジトリを表示していこうと思います。
# 導入方法
基本的な導入方法は、hubzillaのインストールしたディレクトリに移動しそこから
`sudo util/add_addon_repo リポジトリのURL 保存名`
これだけです。
アップデートをする時は
`sudo util/update_addon_repo 保存名`
でアップデートができます。
ファイル自体は/extend/addonに保存名のフォルダで保存されているのでその中で直接gitコマンドで弄ることもできます。
# リポジトリ一覧
* addons
https://framagit.org/hubzilla/addons
公式のaddonリポジトリになります。インストールの過程で導入していることが多いです。これによってActivitypubなどの接続が使えるようになっています。

* harukin's personal addon
https://gitlab.haruk.in/harukin/addon
非公式のaddonリポジトリです。ぐちゃぐちゃなコードでGoogle+擬きを作成していたりします。(plusfuture)