---
layout: post
title: "Tr0ll Vulnhub Writeup"
date: 2017-04-11
share: true
description: Tr0ll Vulnhub Writeup
tags:
 - vm
comments: false
---

# Tr0ll Vulnhub

Herkese merhabalar. Üniversite 1. sınıftan beri hep ertelediğim işi artık yapacağım! Yani Blog tutucam :) Elimden geldiğince faydalı olacağını düşündüğüm yazılar yazmaya çalışacağım. Umarım faydalı olur. O zaman başlayalım artık.

Bu yazı da sizlere Tr0ll isimli zaafiyetli makinenin çözümünü anlatacağım.Öncelikle makineyi indirmek için;

>[https://www.vulnhub.com/entry/tr0ll-1,100/](https://www.vulnhub.com/entry/tr0ll-1,100/)

Makineyi sanal ortamda kurulumunu tamamlandıktan sonra zaafiyetli makinenin ıp adresini tespit edip işe koyuluyoruz.
Benim kali makinemin ıp adresi 192.168.100.129..

	$ netdiscover -r 192.168.100.0/24 

![](/images/tr0ll/netd.png)
	
diyerekten makinenin ıp adresini buluyoruz.Bende 192.168.100.140 olarak gözüküyor.Ip adresini de bulduğumuza göre bir nmap taraması atıp yolumuzu buluyoruz.

![nmap](/images/nmap.png)

nmap parametrelerini kısaca açıklayayım;
>-sS - - - > SYN Scan. SYN bayraklı paketler yollar. Varsayılan olarak kullanılır ve oldukça hızlıdır.

>-sV - - - > Hangi servisin çalışdığını bilmek yetmez versiyon bilgileride lazımdır.Çalışan servislerin versiyon bilgisini verir.

>-A - - - > Agresif tarama yapar. İşletim sistemi bilgisini , servis ve versiyon bilgilerini verir.

>-Pn - - -> Ping atmadan taramayı gerçekleştirir.

>-p- - - - > Tüm portarı tarar.

Çıktılara bakarsak;

>1-) ftp servisi açık ve anonymous kullanıcılarına açık.
>2-) ssh servisi açık.
>3-) http servisi açık ve çoğu zaman karşılaştığımız robots.txt dosyası bulunuyor.(robots.txt dosyasına arama motorları tarafından indexlenmesini istemediğimiz url leri yazarız.)Ve secret url bulunuyor.

Öncelikle http servisinde neler bekliyor bizi bakıyoruz.

![](/images/tr0ll/1.png)

Kaynak koduna falan bakıyoruz. Cıks ekmek yok buradan. O zaman robots dosyasına bi göz gezdirelim.

![](/images/tr0ll/2.png)

![](/images/tr0ll/3.png)

Buradan da ekmek yok bize. O zaman ftp ye anonymous olarak giriş yapalım.

![](/images/tr0ll/4.png)

Kullanıcı adı anonymous password anonymous olarak giriş yapabiliyoruz. ls komutu ile burada neler tutuluyor diye baktığımızda lol.pcap adında dosyayı görüyoruz. Dosyayı isterseniz get dosyaAdi şeklindede kendi makinenize çekebilirsiniz isterseniz browser üzerinden ftp://ıp-adresi diyerekten görüntüleyip indirirebilirsiniz.

![](/images/tr0ll/5.png)

Bildiğiniz gibi pcap dosyaları ağ trafiğini gösteren dosyalarıdır ve wireshark programı ile incelenebiliyor. 










