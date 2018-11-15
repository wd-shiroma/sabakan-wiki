<!-- TITLE: Write Freely インストール手順 -->
<!-- SUBTITLE: golangなので比較的楽です -->

# 想定環境
* Ubuntu18.04LTS

# インストール手順
## aptパッケージインストール
```
$ sudo apt update && sudo apt upgrade -y
$ sudo apt install mysql-server mysql-client nginx wget
```

##  WriteFreelyダウンロード

最新のバージョンを [こちら](https://github.com/writeas/writefreely/releases) から確認するwgetする

```
$ mkdir writefreely && cd writefreely
$ wget https://github.com/writeas/writefreely/releases/download/v0.2.1/writefreely_0.2.1_linux_amd64.tar.gz
$ tar xvf writefreely_0.2.1_linux_amd64.tar.gz
```

サブディレクトリ等作られないので注意すること。

## MySQLデータベース作成

```
$ sudo mysql -u root
mysql> CREATE USER wfuser@localhost IDENTIFIED BY '適当なパスワード';
mysql> CREATE DATABASE writefreely;
mysql> USE writefreely;
mysql> GRANT ALL PRIVILEGES ON * TO wfuser@localhost;
mysql> quit
$ mysql -u wfuser -p writefreely < schema.sql
Enter password: 先程入力したパスワード
```

## アプリケーション設定

アプリケーション設定は対話形式で行われる。

```
$ ./writefreely --config
No configuration yet. Creating new.

  ✍ Write Freely Configuration ✍

  This quick configuration process will generate the application's config
file, config.ini.

  It validates your input along the way, so you can be sure any future
errors aren't caused by a bad configuration. If you'd rather configure your
server manually, instead run: writefreely --create-config and edit that
file.

 Server setup
Local port: 8080 ←利用するポート番号(デフォルト8080)

 Database setup
Username: wfuser ←MySQLの項で設定したユーザ名
Password: *********** ←MySQLの項で設定したパスワード
Database name: writefreely ←MySQLのユーザ名
Host: localhost ←MySQLのホスト名
Port: 3306 ←MySQLのポート番号

 App setup
Use the arrow keys to navigate: ↓ ↑ → ←
? Site type: ←カーソルキー上下で選択。シングルユーザモードにするとユーザを作成してくれる。
  ▸ Single user blog
    Multi-user instance

※※　ここからはシングルユーザモードにした場合の設定手順　※※

Admin username: hoge ←アカウント名
Admin password: fuga ←パスワード
Blog name: Sabakan Industries ←ブログ名()
Public URL: https://sabakan.indeustries

※※　ここからはマルチユーザモードにした場合の設定手順　※※

Blog name: Sabakan Industries ←ブログ名()
Public URL: https://sabakan.indeustries
Use the arrow keys to navigate: ↓ ↑ → ←
? Registration:
  ▸ Open
    Closed
Use the arrow keys to navigate: ↓ ↑ → ←
? Registration: ←登録許可するか(マルチユーザモードの場合、管理ユーザすら作成されないため、最初は許可しておく)
  ▸ Open
    Closed
Max blogs per user: 1 ←ユーザごとの最大ブログ数
Use the arrow keys to navigate: ↓ ↑ → ←

※※　再び共通手順　※※

? Federation: ←ActivityPubを有効にするかどうか(恐らく)
  ▸ Enabled
    Disabled
Use the arrow keys to navigate: ↓ ↑ → ←
? Federation usage stats: ←ActivityPubで流した際の公開範囲
  ▸ Public ←未収載
    Private ←非公開
Use the arrow keys to navigate: ↓ ↑ → ←
? Instance metadata privacy: インスタンスのメタデータ(？)
  ▸ Public
    Private
```

ここで設定したものは `config.ini` に出力されるため、後から編集することも可能である。
（管理ユーザを作成した場合はDBを直接修正する必要がある）

鍵ファイルの作成（生成し直すとログインできなくなるため注意）

```
$ ./writefreely -gen-keys
```

## systemd設定

```
$ sudo vim /etc/systemd/system/writefreely.service
```

以下のコンフィグを貼り付ける。

```
[Unit]
Description=write freely server
After=network.target

[Service]
Type=simple
User=WriteFreelyの実行ユーザ
Group=WriteFreelyの実行ユーザグループ
WorkingDirectory=WriteFreelyの格納パス(絶対パス)
ExecStart=WriteFreelyの格納パス(絶対パス)/writefreely
Restart=always

[Install]
WantedBy=multi-user.target
```

サービスの起動

```
$ sudo systemctl daemon-reload
$ sudo systemctl start writefreely
$ sudo systemctl enable writefreely
```

## Nginx設定

```
$ sudo vim /etc/nginx/sites-available/writefreely
```

以下のコンフィグの`server_name`, `root`, `proxy_pass` のパラメータを自分の環境に併せて編集し保存する。

```
server {
    listen 80;
    listen [::]:80;

    root /var/www/writefreely;

    server_name info.sabakan.industries;

    location ~ ^/.well-known/(webfinger|nodeinfo|host-meta) {
        allow all;
    }

    location / {
        return 301 https://$host$request_uri;
    }
}

server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;

    root /var/www/writefreely;

    server_name info.sabakan.industries;

    keepalive_timeout    70;
    sendfile             on;
    client_max_body_size 80m;

    gzip on;
    gzip_disable "msie6";
    gzip_vary on;
    gzip_proxied any;
    gzip_comp_level 6;
    gzip_buffers 16 8k;
    gzip_http_version 1.1;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript application/x-javascript image/svg+xml image/x-icon application/vnd.ms-fontobject application/font-sfnt;

    add_header Strict-Transport-Security "max-age=31536000";
    add_header Referrer-Policy 'no-referrer';

    #ssl_certificate /.../cert.pem;
    #ssl_certificate_key /.../cert.key;

    location / {
        try_files $uri @proxy;
    }

    location @proxy {
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto https;
        proxy_set_header Proxy "";
        proxy_pass_header Server;

        proxy_pass http://127.0.0.1:8080;
        proxy_buffering off;
        proxy_redirect off;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;
    }
}
```

保存して終了したらシンボリックリンクを貼る。

```
$ sudo ln -s /etc/nginx/sites-available/writefreely /etc/nginx/sites-enabled/writefreely
```

設定ファイルに間違いがないかテストを行う。

```
$ sudo nginx -t
```

最後に `success!` と出ていたらサービスを再起動する。

```
$ sudo systemctl reload nginx
```

## Let'sEnclyptの設定

今回はCloudFlareを通して設定してしまったため、また後ほど追記する。
（または誰か書いてください）