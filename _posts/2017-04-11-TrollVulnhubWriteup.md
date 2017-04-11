---
layout: post
title: "Troll Vulnhub Writeup"
date: 2017-04-11
share: true
description: Troll Vulnhub Writeup
tags:
 - vm
comments: false
---

# Troll Vulnhub

Bu yazı da sizlere Troll isimli zaafiyetli makinenin çözümünü anlatacağım.Öncelikle makineyi indirmek için;

>[https://www.vulnhub.com/entry/tr0ll-1,100/](https://www.vulnhub.com/entry/tr0ll-1,100/)

Makineyi sanal ortamda kurulumunu tamamlandıktan sonra 

	$ netdiscover ya da netdiscover -r 192.168.1.0/24
	
diyerekten makinenin ıp adresini buluyoruz.Bende 192.168.1.41 olarak gözüküyor.Ip adresini de bulduğumuza göre bir nmap taraması atıp yolumuzu buluyoruz.

![nmap](/images/nmap.png)




