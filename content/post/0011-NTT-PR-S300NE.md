---
title: "続: NTT PR-S300NE ファームウェアバージョン 22.02 の問題"
date: 2018-05-01T18:05:00+09:00
categories: ["NTT", "フレッツ光", "Internet", "ルーター"]
tags: ["NTT", "フレッツ光", "Internet", "インターネット", "ルーター"]
topics: ["NTT PR-S300NE の問題"]
draft: false
---

NTT PR-S300NE ver. 22.02 には、IPv6 を PPPoE する機能があるのだが、ログイン認証情報を IPv4 PPPoE と共有する前提になっているため、[インターリンク](https://www.interlink.or.jp/service/flets/b/option.html)のように、IPv4 と IPv6 で認証情報が異なるプロバイダでは使えない。o(｀ω´*)oﾌﾟﾝｽｶﾌﾟﾝｽｶ!!


ということで、IPv6 の IPoE に対応している [ASAHIネット](https://asahi-net.jp/)の最安プランを再契約。対応ルーターを買えばすむ話だが、新しい Mac を買うので節制しなくてはならんという事情と、[ASAHIネット](https://asahi-net.jp/)なら、IPv6 の設定を変更せずにすむという事情もある。

うちのインターネット環境は、IPv4 = [インターリンク](https://www.interlink.or.jp/service/flets/b/option.html)の固定 IP、IPv6 = [ASAHIネット](https://asahi-net.jp/)と決まった。

