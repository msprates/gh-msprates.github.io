---
title: GNS3 & Terminator
date: 2021-02-11 10:00:00 +0000
categories: [GNS3]
tags: [tips]
---

Para fazer com que o GNS3 use abas ao invés de abrir um novo terminal de cada vez, pode-se usar o terminator com a seguinte sintaxe.

`terminator -m --new-tab -T "%d" -e "telnet %h %p"`