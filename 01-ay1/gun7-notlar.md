# Ay 1 – Gün 7

## Servis ve Deamon nedir?
- Service(Servis):Sistemde arka planda,kullanıcıdan bağmızsız çalışan ve belirli bir işi sürekli yapan programlardır.
- Deamon:Linux/Unix dünyasnda servislerin özel adıdır.Her deamon bir servistir ama her servis mutlaka Deamon olmak zorunda değildir.
- Neden Deamon deniyor?:İngilizcede Deamon arka planda çalışan varlık anlamındadır.Deamon Linux'ta sürekli çalışan servislerdir.Genelde isimleri d harfi ile biter
## Program/Servis/Process(Kritik Ayrım)
Kavram:	           Açıklama:
      Program	             Diskte duran dosya (/usr/bin/ssh)
      Process	             Program çalışınca RAM’de oluşan canlı yapı
      Service / Deamon	   Sürekli çalışan, genelde açılışta başlayan process
-Not: Bir Servis her zaman bir process olarak çalışır.
## Sistem Açılırken Neler Olur? Adım Adım:
- Bir Linux sistemi çalışırken arka planda inanılmaz bir trafik vardır.
🧩 1. BIOS / UEFI
Donanım kontrolü
Disk nerede? RAM var mı?
🧩 2. Bootloader (GRUB)
Hangi işletim sistemi açılacak?
Linux kernel’i yükler
🧩 3. Kernel (Çekirdek)
Donanımı tanır
CPU, RAM, disk, ağ kartı
İlk process’i başlatır:
📌 PID 1
🧩 4. init / systemd (EN KRİTİK NOKTA)
Modern Linux’larda:
systemd çalışır (PID 1)
systemd =
“Hangi servis ne zaman, hangi sırayla, hangi yetkiyle çalışacak?” sorusunun cevabı
🧩 5. Servisler Ayağa Kalkar
systemd:
Ağ servisini başlatır
Log servisini başlatır
SSH, cron, bluetooth, printing vb.
⚠️ Hepsi kullanıcı giriş yapmadan önce olur.
## Tipik Açılışta Çalışan Servisler
-Sistem İçin zorunlu olanlar:
- systemd-journald ->Log tutar.
- udevd ->Donanım algılama
- Networkmanager ->ağ bağlantısı
- dbus ->Servisler arası iletişim.
- Ağ & Erişim:
- sshd ->Uzaktan bağlantı
- cups->Yazıcı
- avahi-deamon ->Yerel Ağ Keşfi
- Zamanlanmış İşler:
- cron/crond
## Servisler Neden Güvenlik Açısından Kritik?
- Servisler sürekli çalışır
- Genelde root yetkisi ile başlar
- Ağ dinler(port açar)
- Saldırganın Baktığı Şey
- Hangi Servis Çalışıyor?
- Hangi Port Açık?
- Güncel Mi ?
- Bi port = bir servis = bir saldırı yüzeyi
- Not:Bir servisin çalışıyor olması tek başına zafiyet değildir.
Ancak güncel olmayan, yanlış yapılandırılmış veya gereksiz servisler
saldırı yüzeyini büyütür.
## Zararlı Servis /Backdoor Mantığı
- Gerçek Saldırı Senaryosu:
1)Saldırgan sisteme sızar
2) Kendi yazdığı programı:
  - Servis olarak ekler
  - Açılışta başlatır
3)Sistem her açıldığında :
  - Arka kapı tekrar çalışır. 
- İste bu yüzden sistemde gereksiz servis çalışmaz.
## Servisleri Görmek 
- systemctl list-units --type=service
- Bu Komut: Çalışn servisleri listeler , hangisi aktif görürsün.
