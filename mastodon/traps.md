---
title: マストドン構築・運用時の罠
description: マストドン構築・運用時の罠の紹介
published: true
date: 2023-08-17T05:15:30.617Z
tags: 
editor: markdown
dateCreated: 2020-05-18T06:41:25.155Z
---

# assets:precompileの罠

## メモリを確保できるようにする

マストドンではアップデートのたびに assets:precompile を行い、JavaScriptやHTMLのデータを生成する必要がありますが、この作業は大量にメモリーを使用するため、
メモリーが少ない環境の場合(メモリ1GBなど)、失敗する可能性があります。実行前にswapを追加するなどして、十分にメモリを確保できるようにすることをおすすめします。

### 一時的にディスクに1GBのスワップ領域を作成して有効にする方法

再起動時は無効となります。

現在のメモリ状態を確認

	$ free -h

追加

	$ sudo fallocate -l 2G /swap
	$ sudo mkswap /swap
	$ sudo swapon /swap

削除

	$ sudo swapoff /swap
	$ sudo rm /swap
	
# インスタンス同士が連携するときの罠
マストドンを外部のインスタンスと連携している状態（ユーザがインスタンスを越えてフォローしあっている状態）になると、
相手側のインスタンスに自分のインスタンスの情報を登録することになります。
自分のインスタンスでデータの細かな修正を行うことはできますが、相手方のインスタンスのデータベースを変更してもらうことは極めて困難です。
そうなる前に他のインスタンスと連携している状態になるまえに、十分に設定を確認しましょう。
以下は、その観点から特に注意しておくべき点です。

## LOCAL_DOMAINにUpper Case(大文字英数字)を使用しない
DNSのドメイン名はcase insensitive(大文字・小文字を区別しない)ですが、マストドン内部ではcase sensitive(大文字・小文字を区別する)です。
マストドンsetup時にドメイン名を大文字・小文字が混在した状態でインスタンスを公開した場合、思わぬ問題を招きますのでおすすめできません。
また、あとからLOCAL_DOMAINをlower-case(すべて小文字英数字)に変えることもできません。
ドメイン名はマストドン設定初期状態からすべてlower-caseで統一することをおすすめします。設定する際は十分注意しましょう。

## データベースを消してしまうと同じドメインでは復活できない
サーバ間でやりとりされるトゥートには鍵が埋め込まれており、ユーザの同一性を保証しているようです。
そのため、他のインスタンスと連携状態になったあと、そのドメインで運用していたインスタンスのデータベースを削除した場合、
その前に存在していた同じドメイン・同じユーザは、他のインスタンスからは違うユーザと認識されるため、
他のインスタンスと正しく通信できなくなる可能性ができなくなり、自力での復活がとても困難になります。
いったん公開したあとにデータベースを削除する際は十分に注意しましょう。


# ユーザの罠

## 大量のフォロワーインポート

大量のフォロワーインポートが行われると、初期の規模に見合わないサーバの負荷に見舞われることになります。
もし既存マストドン利用者の引っ越しが想定される場合、十分なサーバリソースを事前に用意しておく必要があります。

# YugabyteDBの罠
[公式の手順](https://docs.joinmastodon.org/admin/install/)に従ってセットアップを進めるとmastodon:setupの途中でデータベース関係のエラーが発生します｡
## 回避策
[これ](https://dev.to/yugabyte/mastodon-on-yugabytedb-10o2)を参考に
mastodon:setup実行前に
```bash
sed -e '/"index_unique_conversations"/d' -i db/schema.rb
sed -e '/"index_ip_blocks_on_ip"/d' -i db/schema.rb
```
mastodon:setup実行後にpsqlなりysqlshでMastodonのデータベースに繋いで
```SQL
create or replace function array_signature(a bigint[]) returns text as 'select array_agg(unnest order by unnest)::text from unnest(a);' immutable language sql;
CREATE UNIQUE INDEX index_unique_conversations ON public.account_conversations (account_id, conversation_id, (array_signature(participant_account_ids)));
```