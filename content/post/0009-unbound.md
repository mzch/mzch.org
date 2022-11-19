---
title: "Unbound で 簡易DNSサーバー"
date: 2018-03-29T03:06:00+09:00
categories: ["DNS", "Unbound"]
tags: ["DNS", "Unbound"]
topics: ["DNS サーバー"]
draft: false
---

XMPP を諦めきれないので、簡易 DNS サーバーを Unbound で立ててみた。例によって、Packages から導入。

```sh
pkg install unbound
cd /usr/local/etc/unbound
cp unbound.conf.sample unbound.conf
```
とした上で、unbound.conf を修正。修正点は以下の通り。

<!--more-->

```conf
interface: 0.0.0.0
interface: ::
port: 53
access-control: 127.0.0.0/8 allow
access-control: ::1 allow
access-control: 192.168.0.0/16 allow
access-control: 2405:6584:7840:800::/64 allow
root-hints: "/usr/local/etc/unbound/named.cache"
local-zone: "mzch.org." static
local-data: "mzch.org. MX 10 mail.mzch.org."
local-data: "mzch.org. MX 100 mx1.mail-services.net."
local-data: "mzch.org. MX 120 mx2.mail-services.net."
local-data: "mzch.org. A 192.168.139.10"
local-data: "mzch.org. AAAA 2405:6584:7840:800::2"
local-data: "www.mzch.org. A 192.168.139.10"
local-data: "www.mzch.org. AAAA 2405:6584:7840:800::2"
local-data: "mail.mzch.org. A 192.168.139.10"
local-data: "mail.mzch.org. AAAA 2405:6584:7840:800::2"
local-data: "box.mzch.org. A 192.168.139.10"
local-data: "box.mzch.org. AAAA 2405:6584:7840:800::2"
local-data: "conference.mzch.org. A 192.168.139.10"
local-data: "conference.mzch.org. AAAA 2405:6584:7840:800::2"
local-data: "Mac_mini.mzch.org. A 192.168.139.10"
local-data: "Mac_mini.mzch.org. AAAA 2405:6584:7840:800::2"
local-data: "_xmpp-client._tcp.mzch.org. SRV 5 0 5222 mzch.org"
local-data: "_xmpp-server._tcp.mzch.org. SRV 5 0 5269 mzch.org"
local-data-ptr: "192.168.139.10 mzch.org."
forward-zone:
        name: "."
        forward-addr: 192.168.139.1
```
さらに、root ネームサーバーのヒントファイルを上記で指定した位置に配置。

```sh
wget https://www.internic.net/domain/named.cache
mv named.cache /usr/local/etc/unbound
```

SRV レコードの設定が肝。これで、ウチの中からでも外からでも XMPP で接続できるようになった。満足。:D