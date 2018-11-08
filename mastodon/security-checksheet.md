<!-- TITLE: Mastodonセキュリティチェックシート -->
<!-- SUBTITLE: Mastodonインスタンスを立ち上げた時、セキュリティ的にチェックしておくべき項目 -->

書きかけ
# ドメイン
- [ ] Whois情報公開代行サービスを使用しているか（オプション）

# DNS
- [ ] DNS CAAを設定しているか（オプション）
- [ ] SPFレコードを設定しているか

# サーバー
## SSH

- [ ] rootユーザーでログインが出来ないようになっているか
				sshd_config で `PermitRootLogin no`になっているか
- [ ] パスワード認証を無効にしているか
				sshd_config で `PasswordAuthentication no` になっているか

## ファイアウォール
- [ ] 80、443、SSHで使用するポートが外向けに開いていないか
- [ ] IPv6でアクセスした場合でも、不要なポートが外向けに開いていないか
				Ubuntuであればufwが有効になっているか、公開する必要の無いポートをallowしていないかを確認すること。

## HTTPヘッダ
- [ ] HTTP Strict Transport Security (HSTS) ヘッダを設定しているか。
- [ ] Mastodonv2.6.0以前の場合、Contents Security Policy(CSP)ヘッダを適切に設定しているか。
- [ ] [Observatory by Mozilla](https://observatory.mozilla.org/)のスコアがA以上になっているか。

## HTTPS
- [ ] Let'sEncryptを使用する場合、certbotの動作を確認したか
				`certbot renew --dry-run`で確認できます。