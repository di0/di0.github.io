---
layout: post
title: "NMAP - Searching open ports into specific Network"
date: 2014-07-08 21:12:26 -0200
comments: true
categories: [linux]
---

Exemplo abaixo, busca por todas conexões SSHs em um range de IP da rede local especificada:

```bash
nmap -p22 192.168.0.0/24 -oG - | grep 22/open
```
