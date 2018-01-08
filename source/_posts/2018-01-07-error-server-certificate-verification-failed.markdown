---
layout: post
title: "ERROR: SERVER CERTIFICATE VERIFICATION FAILED"
date: 2015-09-23 03:05:40 -0200
comments: true
categories: [git]
---

No momento do clone:

``` bash
git clone https://develdio.com/SourceCode.git
```

Foi obtido o seguinte erro:

** Cloning into ‘SourceCode’… **

** fatal: unable to access 'https://develdio.com/SourceCode.git/': server certificate verification failed. **
** CAfile: /etc/ssl/certs/ca-certificates.crt CRLfile: none **

A falha ocorre devido o certificado não ser confiado.

Para solucionar o problema através utilitário do git na linha de comando, basta executar:

``` bash
git config --global http.sslverify false
```
