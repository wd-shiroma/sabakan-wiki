<!-- TITLE: サーバー監視ツールを使おう -->
<!-- SUBTITLE: サーバー監視ツールを導入して安定運用を目指そう -->

# サーバー監視ツールとは？
サーバー監視ツールには主に以下の機能があります
1. CPUやメモリなどリソース消費を収集し、グラフなどで分かりやすく表示する
2. リソースの消費が設定した閾値を超えた場合にメールなどでアラートを送る
3. 指定したURLへ定期的にアクセスし、繋がらなければアラートを送る

サーバーを安定運用するには、日々状態を把握することは重要です。

## サーバー監視サービス(SaaS)

SaaSの場合、監視用のサーバーを自前で持つ必要が無く手軽に導入できる利点があります。
無料プランの場合、利用できる機能や収集した情報の保持期間に制限がある場合があります。

### mackerel 
はてなが提供するサーバー監視サービス。

* Good
	導入が簡単（ワンライナーで導入できちゃう）
	プラグインも結構揃っている
	
* Bad
	Freeプランは収集したメトリクスを1日しか保持してくれない
	Freeプランの外形監視はIPアドレスだけ
	
* 参考
	[mackerel](https://mackerel.io)
	
### StatusCake
URLやIPアドレスへ定期的（無料プランでは最短5分おき）にアクセスし、死活状況、レスポンスタイムをモニタリングします。
一般公開できるステータスページを作成できます。（独自ドメインOK）

* 参考
  [StatusCake](https://www.statuscake.com/)
	
### UptimeRobot
StatusCakeとほぼ同じ事ができます。

* 参考
	[Uptime Robot](https://uptimerobot.com/)
	
## サーバー監視ツール(OSS)

監視用のサーバーを用意する必要がありますが、機能や取得情報の保持期間の自由度は高くなります。