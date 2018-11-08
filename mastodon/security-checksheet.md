<!-- TITLE: Mastodonセキュリティチェックシート -->
<!-- SUBTITLE: Mastodonインスタンスを立ち上げた時、セキュリティ的にチェックしておくべき項目 -->

# ドメイン編
- Whois情報公開代行サービスを使用しているか（オプション）
	Whois情報公開代行が出来ないTLDもあります。自分の情報を公開したくない場合は、Whois情報公開代行ができるTLDを選択しましょう。

# DNS編
- DNS CAAを設定しているか（オプション）
- SPFレコードを設定しているか

# サーバー編
## SSH

- rootユーザーでログインが出来ないようになっているか
	sshd_config で `PermitRootLogin no`になっているか
- パスワード認証を無効にしているか
	sshd_config で `PasswordAuthentication no` になっているか
- fail2ban等で侵入を試みたIPアドレスをブロックしているか（オプション）

## ファイアウォール
- 80、443、SSHで使用するポートが外向けに開いていないか
- IPv6でアクセスした場合でも、不要なポートが外向けに開いていないか
	Ubuntuであればufwが有効になっているか、公開する必要の無いポートをallowしていないかを確認すること。

## HTTPヘッダ
- HTTP Strict Transport Security (HSTS) ヘッダを設定しているか。
- Mastodon v2.6.0より前のバージョンを使用している場合、Contents Security Policy(CSP)ヘッダを適切に設定しているか
	v2.6.0以降は、MastodonがCSPヘッダーを付与するため不要です。
- [Observatory by Mozilla](https://observatory.mozilla.org/)のスコアがA以上になっているか

## HTTPS
- Let'sEncryptを使用する場合、certbotの動作を確認したか
	`certbot renew --dry-run`で確認できます。
	
# コンプライアンス
- ソースを改変した場合、そのソースをGithub等で公開しているか
- [プロバイダ責任制限法](http://www.isplaw.jp/)を一読したか