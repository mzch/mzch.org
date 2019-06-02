---
title: "Growi をインストールしてみた"
date: 2019-01-10T01:40:00+09:00
categories: ["Wiki", "自宅サーバー", "FreeBSD"]
tags: ["Wiki", "Growi", "自宅サーバー", "FreeBSD"]
topics: ["Growi を FreeBSD へインストール"]
draft: false
---

備忘録置き場に Wiki を使いたくてググってみたところ、[Growi](https://growi.org/) というよさげな Wiki エンジンがあった。Linux へのインストールは色々記事が見つかったけど、FreeBSD にインストールしてる人はいなさげなので、作業記録をメモっておく。

[Growi](https://growi.org/) は、全文検索に [ElasticSearch](https://www.elastic.co/products/elasticsearch) を使うので、まずそれをインストール。
```sh
sudo pkg install elasticsearch6-6.4.2_1
```
必要なプラグインもあわせてインストール。
```sh
sudo /usr/local/lib/elasticsearch/bin/elasticsearch-plugin install analysis-kuromoji
sudo /usr/local/lib/elasticsearch/bin/elasticsearch-plugin install analysis-icu
```
次に、[Node.js](https://nodejs.org/ja/) が必要なので、これもパッケージからインストール。ただし、バージョンは [LTS](https://github.com/nodejs/Release#release-schedule) の <span style="color:red">10.x</span> でないといけないので、そこだけ注意。
```sh
sudo pkg install node10-10.15.3
sudo pkg install npm-node10-6.9.0
```
Yarn が必要なので、npm でインストール。
```sh
sudo npm install -g yarn
```
続けて、[MongoDB](https://www.mongodb.com/) をインストール。
```sh
sudo pkg install mongodb36-3.6.10
```
[gcc](https://www.gnu.org/software/gcc/) でビルドが前提のモジュール (DTraceProviderBindings) があるため、[gcc](https://www.gnu.org/software/gcc/) をインストール。12.0-RELEASE は (それ以前もだが)、[llvm](https://llvm.org/) がシステムデフォルトなので、別途導入が必要なのだ。
```sh
sudo pkg install gcc
```
最後に、[Git](https://git-scm.com/) をインストール。
```sh
sudo pkg install git-2.20.1
```
[Growi](https://growi.org/) の[リポジトリ](https://github.com/weseek/growi)をクローンする。
```sh
cd /usr/local
sudo git clone https://github.com/weseek/growi
```
最新リリースを確認して、チェックアウトしておく。
```sh
git checkout -b v3.3.4 refs/tags/v3.3.4
```
Yarn で必要なパッケージをインストール。
```sh
sudo yarn
```
DTraceProviderBindings が、DTrace 機能を必要とするので、カーネルモジュールをロードする。ロードしておかないとエラーになって起動しない。
```sh
kldload dtrace
```
/boot/loader.conf に追記しておくと便利。
```conf
dtrace_load="YES"
```
起動スクリプトを /usr/local/bin/startgrowi.sh として作成。以下のような感じ。
```sh
#! /usr/local/bin/bash

PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin
export PORT=3000
export PASSWORD_SEED=/dev/random
export FILE_UPLOAD=local
export MONGO_URI=mongodb://localhost:27017/growi
export ELASTICSEARCH_URI=http://localhost:9200/growi
cd /usr/local/growi
npm start > /dev/null 2>&1 &
```
ここで、試しに起動してみると、
```log
ld-elf.so.1: /usr/local/growi/node_modules/uws/uws_freebsd_57.node: Undefined symbol "SSL_library_init"
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! growi@3.3.4-RC server:prod: `env-cmd config/env.prod.js node src/server/app.js`
npm ERR! Exit status 1
npm ERR! 
npm ERR! Failed at the growi@3.3.4-RC server:prod script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     /root/.npm/_logs/2019-01-09T14_32_48_170Z-debug.log
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! growi@3.3.4-RC start: `npm run server:prod`
npm ERR! Exit status 1
npm ERR! 
npm ERR! Failed at the growi@3.3.4-RC start script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     /root/.npm/_logs/2019-01-09T14_32_48_222Z-debug.log
```
とかいうようなエラーが出るかも知れない。が、慌てず騒がず、
```sh
sudo yarn upgrade
```
する。すると、依存パッケージが更新されて、uws は消えてなくなり、上記のスクリプトで起動するようになる。
