---
title: "ejabberd へ移行"
date: 2018-12-29T19:03:00+09:00
categories: ["XMPP", "Jabber", "自宅サーバー"]
tags: ["XMPP", "Jabber", "自宅サーバー", "チャット"]
topics: ["チャットサーバー変更"]
draft: false
---

[FreeBSD 12.0](https://www.freebsd.org/releases/12.0R/announce.html) へアップグレードしたら、[Prosody](https://prosody.im/) がパッケージから消えたｗ

ので、[ejabberd](https://www.ejabberd.im/) へ移行。ググっても古いバージョンの情報しかないので、公式サイトを参照しつつ設定。後々参考にするために、設定変更点を残しておく。

<!--more-->

```diff
--- ejabberd.yml.example	2018-12-10 03:53:39.000000000 +0900
+++ ejabberd.yml	2018-12-12 20:54:36.204244000 +0900
@@ -87,6 +87,7 @@
##
hosts:
- "localhost"
+  - "mzch.org"

##
## route_subdomains: Delegate subdomains to other XMPP servers.
@@ -102,12 +103,11 @@
## chains of certificates or certificate keys. Full chains will be built
## automatically by ejabberd.
##
-## certfiles:
-##   - "/etc/letsencrypt/live/example.org/*.pem"
-##   - "/etc/letsencrypt/live/example.com/*.pem"
+certfiles:
+  - "/usr/local/etc/letsencrypt/live/mzch.org/*.pem"
##
## If your system provides only a single CA file (CentOS/FreeBSD):
-## ca_file: "/etc/ssl/certs/ca-bundle.pem"
+ca_file: "/usr/local/etc/ssl/cert.pem"

###.  =================
###'  TLS configuration
@@ -126,11 +126,20 @@
##
## c2s_dhfile: 'DH_FILE'
## s2s_dhfile: 'DH_FILE'
-## c2s_ciphers: 'TLS_CIPHERS'
-## s2s_ciphers: 'TLS_CIPHERS'
-## c2s_protocol_options: 'TLS_OPTIONS'
-## s2s_protocol_options: 'TLS_OPTIONS'
+c2s_ciphers: "ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256"
+s2s_ciphers: "ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256"
+c2s_protocol_options:
+  - "no_sslv2"
+  - "no_sslv3"
+  - "no_tlsv1"
+  - "no_tlsv1_1"

+s2s_protocol_options:
+  - "no_sslv2"
+  - "no_sslv3"
+  - "no_tlsv1"
+  - "no_tlsv1_1"
+
###.  ===============
###'  LISTENING PORTS

@@ -152,7 +161,7 @@
## To enforce TLS encryption for client connections,
## use this instead of the "starttls" option:
##
-    ## starttls_required: true
+    starttls_required: true
##
## Stream compression
##
@@ -265,7 +274,7 @@
## Allowed values are: false, optional or required
## You must specify 'certfiles' option
##
-## s2s_use_starttls: optional
+s2s_use_starttls: required

##
## S2S whitelist or blacklist
@@ -280,9 +289,9 @@
## Preferred address families (which to try first) and connect timeout
## in seconds.
##
-## outgoing_s2s_families:
-##   - ipv4
-##   - ipv6
+outgoing_s2s_families:
+  - ipv4
+  - ipv6
## outgoing_s2s_timeout: 190

###.  ==============
@@ -477,9 +486,9 @@
## The 'admin' ACL grants administrative privileges to XMPP accounts.
## You can put here as many accounts as you want.
##
-  ## admin:
-  ##   user:
-  ##     - "aleksey@localhost"
+  admin:
+     user:
+       - "mzch@mzch.org"
##     - "ermine@example.org"
##
## Blocked users
@@ -705,7 +714,7 @@
## A contact mail that the ACME Certificate Authority can contact in case of
## an authorization issue, such as a server-initiated certificate revocation.
## It is not mandatory to provide an email address but it is highly suggested.
-   contact: "mailto:example-admin@example.com"
+   contact: "mailto:mzch@mzch.org"

## The ACME Certificate Authority URL.
```
