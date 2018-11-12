<!-- TITLE: Mastodonセキュリティチェックシート -->
<!-- SUBTITLE: Mastodonインスタンスを立ち上げた時、セキュリティ的にチェックしておくべき項目 -->

# ドメイン編
- [ ] Whois情報公開代行サービスを使用しているか（オプション）
	Whois情報公開代行が出来ないTLDもあります。自分の情報を公開したくない場合は、Whois情報公開代行ができるTLDを選択しましょう。

# DNS編
- [ ] DNS CAAを設定しているか（オプション）
	設定する値は証明書の発行機関により異なります。
- [ ] SPFレコードを設定しているか
	外部のメールサービスを使用する場合、利用時の初期設定で設定している場合があります。

# サーバー編
## SSH

- [ ] rootユーザーでログインが出来ないようになっているか
	sshd_config で `PermitRootLogin no`に設定し、rootユーザーでのログインが拒否されることを確認しましょう。
- [ ] パスワード認証を無効にしているか
	sshd_config で `PasswordAuthentication no` に設定し、パスワードでの認証が出来ないことを確認しましょう。
- [ ] fail2ban等で侵入を試みたIPアドレスをブロックしているか（オプション）

## ファイアウォール
- [ ] 80、443、SSHで使用するポートが外向けに開いていないか
- [ ] IPv6でアクセスした場合でも、不要なポートが外向けに開いていないか
	Ubuntuであればufwが有効になっているか、公開する必要の無いポートをallowしていないかを確認すること。

## HTTPヘッダ
- [ ] HTTP Strict Transport Security (HSTS) ヘッダを設定しているか。
- [ ] Mastodon v2.6.0より前のバージョンを使用している場合、Contents Security Policy(CSP)ヘッダを適切に設定しているか
	v2.6.0以降は、MastodonがCSPヘッダーを付与するため不要です。
- [ ] [Observatory by Mozilla](https://observatory.mozilla.org/)のスコアがA以上になっているか

## HTTPS
- [ ] Let'sEncryptを使用する場合、certbotの動作を確認したか
	証明書の更新が出来ていないと、ある日突然アクセスできなくなる事があります。`certbot renew --dry-run`で確認できます。
	
# コンプライアンス編
- [ ] ソースを改変した場合、そのソースをGithub等で公開しているか
  MastodonはAPGLであるため、ソースコードを改変した場合、開示する必要があります。
- [ ] 関連法規を確認したか
	法律を知っておくことは、何かあった時に自分を守ることにも繋がります。
	- [個人情報の保護に関する法律（個人情報保護法）](http://elaws.e-gov.go.jp/search/elawsSearch/elaws_search/lsg0500/detail?lawId=415AC0000000057)
	- [特定電気通信役務提供者の損害賠償責任の制限及び発信者情報の開示に関する法律（プロバイダ有限責任法）](http://elaws.e-gov.go.jp/search/elawsSearch/elaws_search/lsg0500/detail?lawId=413AC0000000137)

# 参考リンク・関連情報
- [プロバイダ責任制限法関連情報Webサイト](http://www.isplaw.jp/)
- [何が変わる？ 改正個人情報保護法 インフォグラフィック | データのじかん](https://data.wingarc.com/privacy-infographic-4897)
- [みんなのための著作権講座(初心者向け解説サイト)](http://kids.cric.or.jp/intro/index.html)
- [個人情報保護委員会](https://www.ppc.go.jp/)
