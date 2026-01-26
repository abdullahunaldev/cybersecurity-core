# GÜN 1 – Bilgisayar Mantığı & Linux’a Giriş
## Ne Öğrendik?
- Bilgisayarlar = CPU + RAM + Disk uyumudur.
- Programlar disk üzerinde pasiftir.
- Çalışınca RAM'e alınır ve **process** olur.
- Not:
  Bilgisayarlar **dosya** değil **process** çalıştırır.
  Güvenlikte Bu Neden Önemli?
  Saldırgan dosyayı değil , çalışan şeyi(process) hedefler.
# Gün 2 - Linux Dizin Yapısı
## Ana Dizinler:
- / -> Kök
- /home -> kullanıcı alanı
- /root -> root'a özel
-  /etc -> sistem ayarları
-  / var -> loglar,değşken veriler
-  /bin , /sbin -> kritik komutlar
-  /proc , /dev -> canlı sistem bilgisi
## Kritik Farkındalık
- Dosyanın nerede olduğu, ne işe yaradığını söyler
# Gün 3 - Kullanıcı, Grup, Root, Sudo
## Temel Kavramlar:
- Her işlem bir kullanıcı adına çalışır.
- Root (UID=0) = Sınırsız yetki
- sudo = **kontrollü root**
- Altın Kural:
  Güvenlikte:
  Root yetkisi = büyük risk
  sudo = profesyonel yaklaşım
# Gün 4 - Program Vs Process
## Ayrım:
- Program -> disk
- Process -> RAm'de çalışan canlı yapı
## Komutlar:
- ps, ps aux ,top
## Kritik Fark:
- root process -> tüm sistemi etkiler
- user process -> sınırlı zarar
- Güvenlikte.
  Gereksiz root process = davetiye
# Gün 5 - Process - Network - Port
## Öğrendik:
- Process networke direk çıkmaz
- Socket açar
- Port üzerinden konuşur
## Komutlar:
- ss -tuln
- ss -tulnp
## Kritik Cümle:
- Hedef port değil, hedef process.
  Güvenlikte:
  Açık port = saldırı yüzeyi
  portun arkasındaki process asıl hedeftir.
# Gün 6 - Log Mantığı
## Log Nedir?
- Sistem hafızası, kim, ne zaman, ne yaptı sorularının kayıtlı hali.
## Önemli Loglar:
- /var/log/auth.log
- /var/log/syslog
- journalctl
## Komutlar:
- tail -f
- grep
- journalctl -u servis
  Güvenlikte:
  Log silinmesi = Alarm
# Gün 7 - Servis & Daemon
## Kavramlar:
- Servis = arka plandasürekli çalışan process
- Daemon = Linux'ta servis adı
- systemd = PID 1
## Açılışta:
- Servisler kullanıcıdan önce başlar.
  Güvwnlikte:
  Servisler genelde root çalışır
  Backdoorlar servis olarak gizlenir
# Gün 8 - Gereksiz Servis & Hardening 
## Gereksiz Servis:
- Sistemin amacına hizmet etmeyen, ama çalışan servis
## Attack Surface:
- Açık portlar, servisler, kullanıcılar, default ayarları.
## Hardering:
- Gerekmiyorsa çalışmasın
  Güvenlikte:
  Az servis = az risk
# Gün 9 - Kullanıcı & Grup Güvenliği
## Last Privilege:
- İhtiyacın kadar yetki, fazlası risk
## Mantık:
- Servisler kendi user'ı ile çalışmalı, root sadece gerektiğinde
  Güvenlikte:
  Ele geçirilirse bile zararı sınırlamak esastır.(damage control)
# Gün 10 - Dosya İzinleri(chmod/chown)
## 3 taraf:
- user
- group
- others
## 3 yetki
- r(read)
- w(write)
- x(execute)
## Komutlar:
- chmod -> Ne yapabilir?
- chown -> Kim yapabilir?
## Kritik Gerçek:
- 777 = alarm
- 750 / 640 = profesyonel
  Güvenlikte:
  Dosya izinleri = son savunma hattı
# 10 Günün Tek Cümlelik Özeti
- Sistem process çalıştırır
- Process bir kullanıcı adına çalışır
- Process port açabilir
- Port bir servise aittir
- Servis yetkiyle çalışır
- Yetki dosya izinleriyle sınırlandırılır
- Loglar her şeyi kaydeder
- Hardening saldırıdan önce yapılır

  
