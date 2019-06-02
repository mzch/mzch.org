---
title: "Pydio - ファイル共有"
date: 2018-03-09T23:56:00+09:00
categories: ["File Sharing", "Online Storage", "Pydio", "Self-Hosted"]
tags: ["ファイル共有", "オンラインストレージ", "Pydio", "Self-Hosted", "自宅サーバー"]
topics: ["自宅サーバー向けオンラインストレージ"]
draft: false
---

ちょっとでも自宅サーバーの価値を高めなくてはならん、ということで、ファイル共有アプリなどを入れてみる。そしたら、PHP 7.2 に未対応で、PHP をバージョンダウンせざるを得なかった。おまけに、runTest.php では検出されない必須モジュールがあったりして、ちょっと手こずった。最終的に、以下のスクリプトでインストールしてけりをつけた。

```sh
#! /bin/sh

PKG='pkg install'

PHP='php71'

$PKG $PHP
$PKG $PHP-bz2
$PKG $PHP-curl
$PKG $PHP-dom
$PKG $PHP-exif
$PKG $PHP-gd
$PKG $PHP-gettext
$PKG $PHP-hash
$PKG $PHP-iconv
$PKG $PHP-imap
$PKG $PHP-intl
$PKG $PHP-json
$PKG $PHP-mbstring
$PKG $PHP-mcrypt
$PKG $PHP-mysqli
$PKG $PHP-opcache
$PKG $PHP-openssl
$PKG $PHP-pdo
$PKG $PHP-pdo_mysql
$PKG $PHP-phar
$PKG $PHP-readline
$PKG $PHP-session
$PKG $PHP-xml
$PKG $PHP-xmlrpc
$PKG $PHP-zip
$PKG $PHP-zlib
```
