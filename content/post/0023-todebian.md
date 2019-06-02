---
title: "FreeBSD から Debian Buster へ移行"
date: 2019-05-17T23:00:00+09:00
categories: ["Linux", "Debian", "自宅サーバー", "Hugo"]
tags: ["Linux", "Debian", "Buster", "自宅サーバー", "Hugo"]
topics: ["Debian への移行"]
draft: false
---

[FreeBSD R12.0](https://www.freebsd.org/releases/12.0R/announce.html) へ移行してしばらくは問題なく動作していた自宅鯖だが、パッケージの更新に伴って、あちこち不具合が出て、まともに動かなくなってしまった。

そこで、ここは一念発起。OS を再インストールするべえと思い立ち、それなら慣れてる [Debian](https://www.debian.org/) でよくね？となって、移行した次第。

で、毎日自動でバックアップしてるし、データは大丈夫だよね、と思って、さくっと鯖に使ってる Mac mini を初期化。今更 [Stretch](https://www.debian.org/releases/stable/) を入れるのもなあと思って、[Buster](https://www.debian.org/releases/testing/) をインストールした。正式リリースも近いしね。:)

で、データを復旧するべく、バックアップデータを確認したら、MySQL のデータベースがバックアップされていない。orz

このブログ以外は、テストデータばかりだったし、ブログの文章自体はローカルの Mac に残してあったので、ええい、更新が面倒な Wordpress はぽいっじゃあ、と [Hugo](https://gohugo.io/) へ移行しました。

[Hugo](https://gohugo.io/) いいよね。Markdown で書けるし、早いし。テーマもいっぱいあるし。
