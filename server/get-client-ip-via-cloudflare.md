<!-- TITLE: Cloudflareを使う場合のクライアントIP取得方法 -->
<!-- SUBTITLE: ログに残るIPアドレスを、Cloudflareのアドレスから、実際にアクセスしているクライアントのIPにする方法 -->

# Cloudflareを通したとき、サーバーのアクセスログってどうなるの？
Cloudflareは仕組み上、クライアントからのアクセスはCloudflareのサーバーを経由します。
そのため、Mastodonのアクセス履歴や、Webサーバーのアクセスログに残るのはCloudflareのサーバーのIPアドレスになります。
そのため、荒らしが来たからと言って安直にIP BANをすると、思いがけない副作用があるかも知れません。

# アクセス元のIPアドレスをログに残したい時
nginxの場合、serverディテクティブの中に以下を追加します。
CloudflareのIPアドレスからのアクセスの場合、CloudfareにアクセスしたIPアドレスが設定されるようにします。

```text
set_real_ip_from 103.21.244.0/22;
set_real_ip_from 103.22.200.0/22;
set_real_ip_from 103.31.4.0/22;
set_real_ip_from 104.16.0.0/12;
set_real_ip_from 108.162.192.0/18;
set_real_ip_from 131.0.72.0/22;
set_real_ip_from 141.101.64.0/18;
set_real_ip_from 162.158.0.0/15;
set_real_ip_from 172.64.0.0/13;
set_real_ip_from 173.245.48.0/20;
set_real_ip_from 188.114.96.0/20;
set_real_ip_from 190.93.240.0/20;
set_real_ip_from 197.234.240.0/22;
set_real_ip_from 198.41.128.0/17;
set_real_ip_from 2400:cb00::/32;
set_real_ip_from 2606:4700::/32;
set_real_ip_from 2803:f800::/32;
set_real_ip_from 2405:b500::/32;
set_real_ip_from 2405:8100::/32;
set_real_ip_from 2c0f:f248::/32;
set_real_ip_from 2a06:98c0::/29;

real_ip_header CF-Connecting-IP;
```

CloudflareのIPアドレスからアクセスがあった場合、nginxでリクエストヘッダを本来のアクセス元IPアドレスに書き換えます。
そのため、iptablesなどサーバー側のファイアウォールではIP BANすることはできません。
CloudflareのIP Firewallの使用を検討しましょう。

# 参考
* [How do I restore original visitor IP with Nginx? – Cloudflare Support](https://support.cloudflare.com/hc/en-us/articles/200170706-How-do-I-restore-original-visitor-IP-with-Nginx-)