# Ay 1 – Gün 6

## Log nedir?
- Log = sistem hafızası/günlüğü. Bir sistemin benim başıma şu zamanda , şu şekilde , şu oldu bilgilerinin saklandığı yerdir.
- Yani ; Kim,Ne yaptı,Ne zaman yaptı,Nerede yaptı,Sonuç ne oldu.
## Sistem Nereye İz Bırakır
- Bir kullanıcı,program veya saldırgan sistemde yaptığı her harakette mutlaka bir iz bırakır sıfır izle sistemden ayrılamaz.Bu izler; dosyaya yazılır , bellekte tutulur , ağ cihazlarında kayıt edilir. 
## Log Türleri
İŞLETİM SİSTEMİ LOG’LARI (Linux / Windows)
 Linux’ta:
             /var/log/
             Burada şunlar olur:
             Log	                Ne Anlatır
             auth.log           	Giriş denemeleri
             syslog	              Sistem olayları
             kern.log	            Kernel olayları
             boot.log	            Açılış süreci
 Windows’ta:
               Event Viewer (Olay Görüntüleyici)
               Security
               System
               Application
APPLICATION (UYGULAMA) LOG’LARI
  Her program kendi günlüğünü tutar.
                           Örnekler:
                                    Apache
                                    Nginx
                                    MySQL
                                    SSH
                                    Python uygulamaları
AUTHENTICATION (KİMLİK DOĞRULAMA) LOG’LARI
**Bu log’lar altın değerindedir.**
Örnek-> Failed password for root from 192.168.1.50
Bu bize şunu söyler:
                    Root hedeflenmiş
                    Yanlış şifre
                    IP adresi belli
                    Zaman belli
NETWORK LOG’LARI 
          Ağ cihazları da iz bırakır:
                            Firewall
                            Router
                            Switch
                            IDS / IPS
                            Kayıt eder:
                            Kaynak IP
                            Hedef IP
                            Port
                            Protokol
                            İzin verildi mi / engellendi mi
PROCESS & COMMAND LOG’LARI (ÇOK KRİTİK)
Sistem şunları da loglar:
                         Hangi komut çalıştırıldı
                         Kim çalıştırdı
                         Root mu user mı
## LOG SİLİNİRSE NE OLUR?
Log silinmesi başlı başına bir alarmdır
 Çünkü;
       Normal kullanıcılog silmez
       Malware silmeye çalışır
       Saldırgan iz kapatır
Not:
Log silinmesi bazen otomatik log rotation ile de olabilir.
Bu yüzden her log kaybı saldırı değildir,
ama mutlaka açıklanması gereken bir durumdur.

## GERÇEK HAYAT BENZETMESİ
| Gerçek Hayat      | Sistem      |
| ----------------- | ----------- |
| Kamera            | Log         |
| Parmak izi        | Log         |
| Telefon kayıtları | Network log |
| Kapı kartı        | Auth log    |

## Zihin Haritası
Kullanıcı / Program / Saldırgan
          ↓
       İşlem yapar
          ↓
     Sistem kaydeder
          ↓
         LOG
          ↓
   Analiz / Adli İnceleme
## Loglar Nerede Tutulur?
### Linux'ta ana lokasyonlar
Klasik dosya logları: /var/log/

En çok göreceğin yer burasıdır.
Sistem logları (genel)
Servis logları (ssh, web server, vs.)
Paket yöneticisi logları (apt/dpkg)
Boot (açılış) logları
Mantık: “Uygulamalar ve sistem, olayları dosyaya yazar.”

systemd “journal” logları (modern Linux): journald

Birçok sistemde loglar sadece dosyada değil, journal içinde de tutulur.
Komut: journalctl
Journal dosyaları genelde: /var/log/journal/ (kalıcıysa) veya /run/log/journal/ (geçiciyse)
Mantık: “Sistem, olayları merkezi bir veritabanı gibi tutar.”

Uygulamaya özel dizinler

Bazı servisler kendi klasöründe tutar:
Nginx: /var/log/nginx/
Apache: /var/log/apache2/
MySQL: /var/log/mysql/ (dağıtıma göre değişebilir)

Audit logları (denetim): auditd

Daha “forensic” / denetim amaçlı:
/var/log/audit/audit.log
Bu, “kim hangi dosyaya erişti / hangi syscall oldu” gibi daha derin izler için kullanılır.
## Log dizini
- /var/log
## En Önemli Log Dosyaları(linux)
Dağıtıma göre isimler değişebilir ama Kali/Debian’da genelde şu “çekirdek” set var:

Kimlik doğrulama / giriş çıkış (en kritik)
/var/log/auth.log
SSH girişleri, sudo kullanımı, başarısız şifreler, user değişimleri.
“Kim sisteme girmeye çalışmış?” sorusunun ana kaynağı.

Sistem olayları (genel)
/var/log/syslog
Servislerin genel mesajları, bazı daemon çıktıları.
/var/log/kern.log
Kernel olayları, driver, donanım, düşük seviye hatalar.

Boot ve servis hataları
/var/log/boot.log (her sistemde olmayabilir)
systemd için: journalctl -b

Paket kurulumu/değişiklik izi
/var/log/apt/history.log
/var/log/dpkg.log
“Sisteme sonradan neler yüklendi/değişti?” için çok önemli.

Web sunucu logları (varsa)
Apache:
/var/log/apache2/access.log
/var/log/apache2/error.log
Nginx:
/var/log/nginx/access.log
/var/log/nginx/error.log
“Hangi IP hangi endpoint’e istek attı?” sorusu buradan çıkar.

Zaman senkronu (olay zamanını doğrulama)
timedatectl (log değil ama olay zamanını anlamada kritik)
NTP/chrony logları (varsa)
## Log Okumayı Kolaylaştırma (pratik teknikler)
Log okumayı gözle tarama ile yaparsan çok yorucu olur.En verimli yöntemler:
A) “Son satırlar” ile başla
-sudo tail -n 50 /var/log/auth.log

B) Arama: grep (en çok kullanacağın)
-Örnek: başarısız giriş denemeleri
-sudo grep -i "failed" /var/log/auth.log

-SSH geçen satırlar:
-sudo grep -i "sshd" /var/log/auth.log

C) Akıllı filtre: awk / kolonlar
-Örn. web access log’da IP’leri çekmek (Apache/Nginx formatına göre değişir):
-awk '{print $1}' /var/log/apache2/access.log | sort | uniq -c | sort -nr | head
Bu şunu verir:
En çok istek atan IP’ler (olası bot/bruteforce/scan)

D) Okunabilirlik: less ile gezinme
-sudo less /var/log/syslog
-less içinde:
 /kelime ile ara
 n sonraki eşleşme
 Shift+G sona git
 
E) Zaman aralığıyla bakmak (journalctl büyük kolaylık)
“Bugünkü boot”:
 -sudo journalctl -b
 “Son 1 saat”:
 -sudo journalctl --since "1 hour ago"
 “Bir servisin logları” (örn: ssh):
 -sudo journalctl -u ssh
 ## Canlı Log Takibi
 A) Klasik: tail -f
Auth log’u canlı takip:
-sudo tail -f /var/log/auth.log

Web erişim log’u canlı:
-sudo tail -f /var/log/apache2/access.log

B) Daha güçlü: journalctl -f
Sistemde anlık olan biten:
-sudo journalctl -f

Sadece SSH servisi:
-sudo journalctl -u ssh -f

C) “Canlı takip + filtre”
Canlı akışı sadece “failed” içerenlere indir:
-sudo tail -f /var/log/auth.log | grep -i "failed"
## Güvenlik Açısından Loglar Nasıl Yorumlanır?
A) Log’ların güvenlikte 5 ana kullanımı
-Tespit (Detection): Şüpheli olayları yakalama
-Olay inceleme (Incident Response): Ne oldu, nasıl oldu?
-Adli analiz (Forensics): Zaman çizelgesi ve delil
-Uyumluluk (Compliance): Kim ne zaman erişti?
-Hardening: Gereksiz servisleri, hatalı konfigleri bulma
B) En kritik “şüpheli” işaretler (red flags)
-Tekrarlayan login başarısızlığı (bruteforce denemesi)
-Hiç beklemediğin saatlerde başarılı login
-Kısa sürede çok sayıda farklı kullanıcı denemesi
-Sudo kullanımı artışı / beklenmeyen komutlar
-Yeni servis açılması (port dinleme → log + process kontrolü)
-Log silme / loglarda boşluk (gap)
-Yeni paket kurulumu (apt/dpkg loglarında)
