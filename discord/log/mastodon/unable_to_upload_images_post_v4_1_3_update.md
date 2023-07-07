---
title: v4.1.3へのアップグレードによる画像アップロードの不具合
description: 
published: true
date: 2023-07-07T13:00:39.191Z
tags: 
editor: markdown
dateCreated: 2023-07-07T13:00:39.191Z
---

# v4.1.3へのアップグレードによる画像アップロードの不具合

[発端ログ(discord)](https://discord.com/channels/480731529073524736/1082217921759150141/1126553682603941940)

## 事象
v4.1.3にアップデート後、画像のアップロードができなくなる。

## 原因
aptパッケージ、またはbundlerパッケージが古い？

(bundlerパッケージだけ？詳しい原因は未解決)

## 解決法
以下の作業を実施したところ解決した。

1. Ubuntuのバージョンを18.04からアップグレード
```
do-release-upgrade
```
※aptパッケージのアップデートと再起動が行われるので、再起動後に `lsb_release -a` でバージョン確認
※ひとつ上のLTSにアップグレードされるので、22.04まで上げたい場合は2回実施すること

2. bundlerパッケージの削除
```
rm -rf vendor/bundle
```

3. rubyの再インストール(いらないかもしれない)
```
RUBY_CONFIGURE_OPTS=--with-jemalloc rbenv install -f && rbenv global $(rbenv local)
gem install bundler --no-document
```

4. bundleパッケージのインストール
```
bundle config deployment 'true'
bundle config without 'development test'
bundle install -j$(getconf _NPROCESSORS_ONLN)
```

## 備考
切り分け中nginx.confの設定も古いことがわかった。
直接的な原因ではないが最新に要更新。

