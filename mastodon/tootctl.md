<!-- TITLE: tootctl -->
<!-- SUBTITLE: tootctl(MastodonのCLIツール) -->

# tootctl
tootctlとは、[Mastodon v2.5.0から実装された](https://github.com/tootsuite/mastodon/commit/793eea29823a44fd4950f87898ecf0ff3b49351d#diff-57c0a6239ec49a6a8c3cff8840996220)Mastodon管理用のCLIツールである。

## 実行方法
非docker環境(Standalone環境)
```
RAILS_ENV=production bundle exec bin/tootctl SUBCOMMAND [OPTIONS]
```
docker環境
```
docker-compose run --rm web bundle exec bin/tootctl SUBCOMMAND [OPTIONS]
```

## サブコマンド一覧
tootctlのヘルプをもとに作成しています。
また、helpコマンドに関してはすべて除外しています。
追加・編集する方は対応バージョン(とできればそのコミットのリンク)を記載するようにしてください。

* accounts [(v2.5.0~)](https://github.com/tootsuite/mastodon/commit/cabdbb7f9c1df8007749d07a2e186bb3ad35f62b#diff-8210eb1811886a2c9582cb7d37944ee0)
	* backup
	* create
	* cull
		参考：[tootctl accounts cull - noellabo's tech blog](https://noellabo.qrunch.io/entries/Qn6iRR55UCwqZdng)
	* delete
	* modify
	* refresh
	* rotate
* domains [(v2.6.0~)](https://github.com/tootsuite/mastodon/commit/6f78500d4f515c65ec66416e2d78bc9ae247f91c#diff-8210eb1811886a2c9582cb7d37944ee0)
	* purge [(v2.6.0~)](https://github.com/tootsuite/mastodon/commit/6f78500d4f515c65ec66416e2d78bc9ae247f91c#diff-8210eb1811886a2c9582cb7d37944ee0)
* emoji [(v2.5.0~)](https://github.com/tootsuite/mastodon/commit/b378b4c5c7e99de2c14e39e2733a745689de8ad1#diff-8210eb1811886a2c9582cb7d37944ee0)
	* import [(v2.5.0~)](https://github.com/tootsuite/mastodon/commit/b378b4c5c7e99de2c14e39e2733a745689de8ad1#diff-8210eb1811886a2c9582cb7d37944ee0)
* feeds [(v2.6.0~)](https://github.com/tootsuite/mastodon/commit/6a3f9b7e53e4cef0b5406676d56e904a02716ee6#diff-8210eb1811886a2c9582cb7d37944ee0)
	* build [(v2.6.0~)](https://github.com/tootsuite/mastodon/commit/6a3f9b7e53e4cef0b5406676d56e904a02716ee6#diff-8210eb1811886a2c9582cb7d37944ee0)
	* clear [(v2.6.0~)](https://github.com/tootsuite/mastodon/commit/6a3f9b7e53e4cef0b5406676d56e904a02716ee6#diff-8210eb1811886a2c9582cb7d37944ee0)
* media [(v2.5.0~)](https://github.com/tootsuite/mastodon/commit/793eea29823a44fd4950f87898ecf0ff3b49351d#diff-8210eb1811886a2c9582cb7d37944ee0)
	* remove [(v2.5.0~)](https://github.com/tootsuite/mastodon/commit/793eea29823a44fd4950f87898ecf0ff3b49351d#diff-8210eb1811886a2c9582cb7d37944ee0)
		```
		tootctl media remove [--days=NUM] [--background | --no-background] [--verbose | --no-verbose] [--dry-run | --no-dry-run]
		```
		連合済みインスタンスから取得したメディアのキャッシュを一括消去します。
		`--days=日数`で指定された日数より前の画像が消去されます。
		`--days`を指定しなかった場合、デフォルトで7日以上前の画像が消去されます。
* settings [(v2.6.0~)](https://github.com/tootsuite/mastodon/commit/186024a058d4b8765a10d87ff3d7f3bdcd2fbb3c#diff-8210eb1811886a2c9582cb7d37944ee0)
	* registrations [(v2.6.0~)](https://github.com/tootsuite/mastodon/commit/186024a058d4b8765a10d87ff3d7f3bdcd2fbb3c#diff-8210eb1811886a2c9582cb7d37944ee0)
		* close  [(v2.6.0~)](https://github.com/tootsuite/mastodon/commit/186024a058d4b8765a10d87ff3d7f3bdcd2fbb3c#diff-8210eb1811886a2c9582cb7d37944ee0)
			```
			tootctl settings registrations close
			```
			新規登録を停止します。
		* open [(v2.6.0~)](https://github.com/tootsuite/mastodon/commit/186024a058d4b8765a10d87ff3d7f3bdcd2fbb3c#diff-8210eb1811886a2c9582cb7d37944ee0)
			```
			tootctl settings registrations open
			```
			新規登録を開放します。