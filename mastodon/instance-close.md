<!-- TITLE: Instance Close -->
<!-- SUBTITLE: A quick summary of Instance Close -->

# インスタンスの閉じ方（閉鎖）について
閉じる側の作業
1. 閉じる前にできるだけ多くの鯖缶に「閉鎖のお知らせ」トゥートなどを広報する。
2. できれば、インスタンスのドメインでHTTPステータスコード410(gone)を返すようにする。410を返すサービスを提供しているところもあるので、それを利用するのも可。

連合リレーなどでつながっているインスタンス側の作業
1.  tootctlの「domain purge」で該当インスタンスのpurgeを行う。


参考記事：
* https://qiita.com/kumasun/items/7aa50a7b1b6d90e322fc
* https://snap.textfile.org/20180609220309/