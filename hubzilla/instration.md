<!-- TITLE: Hubzillaのインストール -->
<!-- SUBTITLE: Hubzillaのインストール手順です。 -->

# インストール前に
ドメインは入手していますか？
Ubuntuサーバーはありますか？
英文は読めますか？
CUIに抵抗は無いですか？
問題無い？よし。はじめよう！
# 使用する環境
* Ubuntu Server 18.04
* Nginx
* MariaDB
* PHP
* dnsレコードは記述済みの前提で書いております。
# 1 nginxのインストール、起動

```text
sudo apt update
sudo apt install nginx
sudo systemctl stop nginx.service
sudo systemctl start nginx.service
sudo systemctl enable nginx.service

```
この状態で目標のサイトにアクセスすると何か表示されるはずです。何も表示されない場合(ブラウザがエラーを吐く場合)はDNSの設定などを見直しましょう。
# 2 mariaDB(mysql)のインストール、起動

```text
sudo apt-get install mariadb-server mariadb-client
sudo systemctl stop mariadb.service
sudo systemctl start mariadb.service
sudo systemctl enable mariadb.service
```
# 3 mariaDBの設定

```text
sudo mysql_secure_installation
```
すると次のような文字が表示されるので従ってください。
    Enter current password for root (enter for none): **（何も入力せずにEnter）**
    Set root password? [Y/n]:** Y**
    New password:**パスワードの入力(新規作成です。sudo等とは関係ありません。)**
    Re-enter new password: **パスワードの再入力(上と同じパスワードを入力)**
    Remove anonymous users? [Y/n]: **Y**
    Disallow root login remotely? [Y/n]: **Y**
    Remove test database and access to it? [Y/n]: ** Y**
    Reload privilege tables now? [Y/n]:  **Y**
# 4 PHPのインストール

```text
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:ondrej/php
```

```text
sudo apt update
sudo apt install php7.2-fpm php7.2-common php7.2-sqlite3 php7.2-curl php7.2-intl php7.2-mbstring php7.2-xmlrpc php7.2-mysql php7.2-gd php7.2-xml php7.2-cli php7.2-zip
```

# 5 PHPの設定
`sudo nano /etc/php/7.2/fpm/php.ini`
でnanoが開かれますので下のデータを探して書き変えてください。

> file_uploads = On
> allow_url_fopen = On
> short_open_tag = On
> memory_limit = 256M
> cgi.fix_pathinfo = 0
> upload_max_filesize = 100M
> max_execution_time = 360
> date.timezone =Asia/Tokyo

ctl+x、で終了できます。データの保存をお忘れなく。

# 6 データベースの作成
まずデータベースにログインします。
`sudo mysql -u root -p`

データベースを作成します。このhubzillaというのがデータベースの名前(メモ1)となります。
`CREATE DATABASE hubzilla;`

データベースにアクセスするユーザーを作成します。hubzillauserというのがユーザー名(メモ2)、passwordは自分の自由に入力(メモ3)してください。
`CREATE USER 'hubzillauser'@'localhost' IDENTIFIED BY 'password';`

ユーザーのアクセス権限の設定です。hubzillaというのは(メモ1)の名前となり、hubzillauserは(メモ2)、passwordは(メモ3)を入力してください。
`GRANT ALL ON hubzilla.* TO 'hubzillauser'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;`

最後に適用して終了してください。
```text
FLUSH PRIVILEGES;
EXIT;
```
# 7 hubzillaのダウンロード、インストール

```text
sudo apt install git
cd /var/www/html
sudo git clone https://framagit.org/hubzilla/core.git hubzilla
cd ./hubzilla
sudo mkdir -p "store/[data]/smarty3"
sudo chmod -R 777 store
sudo util/add_addon_repo https://framagit.org/hubzilla/addons.git hzaddons
sudo sudo util/update_addon_repo hzaddons
```
# 8 アクセス権限の設定
webブラウザに利用する権限を設定します。

```text
sudo chown -R www-data:www-data /var/www/html/hubzilla/
sudo chmod -R 755 /var/www/html/hubzilla/
```

# 9 nginxの設定
`sudo nano /etc/nginx/sites-available/hubzilla`
またnanoが開くので、以下を入力。example.comは自分のドメインを入力してください。
> server {
>     listen 80;
>     listen [::]:80;
>     root /var/www/html/hubzilla;
>     index  index.php index.html index.htm;
>     server_name  example.com www.example.com;
> 
>     client_max_body_size 100M;
> 
>     location / {
>         if ($is_args != "") {
>         rewrite ^/(.*) /index.php?q=$uri&$args last;
>       }
>     rewrite ^/(.*) /index.php?q=$uri last;
>       }
> 
> 
>     location ~ \.php$ {
>          include snippets/fastcgi-php.conf;
>          fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
>          fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
>          include fastcgi_params;
>     }
> }

保存して終了してください。
最後に設定を有効化して終了です。

```text
sudo ln -s /etc/nginx/sites-available/hubzilla /etc/nginx/sites-enabled/
sudo systemctl restart nginx.service
```

# 10 初期設定
使うドメインにアクセスしてみてください。問題無く成功しているようであれば初期設定画面が表示されるはずです。
![Hubzilla Ubuntu Install](/uploads/hubzilla-ubuntu-install.png "Hubzilla Ubuntu Install")

