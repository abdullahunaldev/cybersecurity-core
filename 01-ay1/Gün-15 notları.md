# Network Trafiğini Anlamaya Başlamak 
- Amaç -> Linux'ta akan trafiğin ne olduğunu, neden olduğunu ve hangi process'in ürettiğini anlamaya başlamak.
- Saldırı yok, araç ezberi yok,sadece gözlem ve mantık !!!
## Network Trafiği Ne Demektir?
- En basit haliyle Network trafiği = bir process'in bir yere veri göndermesi veya bir yerden veri alması yani;
- Trafik kendi kendine oluşmaz, mutlaka bir process üretir
  ## Günlük Hayat Benzetmesi
  - Bunu bir apartman olarak düşünelim
  - Apartman -> Linux sistem
  - Daire -> Process
  - Telefon hattı -> Network bağlantısı
  - Kouşan kişi -> Proces içindeki program
  - Eğer bir telefon görüşmesi varsa ; birisi arıyordur aynı şey network içinde geçerli.
  ## İlk Gözlem Sistemde Kim Konuşuyor?
  - Komut: ss -ntp
  - Şu soruyu sorar : Şu anda TCP üzerinden kim, kiminle konuşuyor?
  - Çıktı: ESTAB 0 0 127.0.0.1:4444 127.0.0.1:46842 users:(("nc",pid=2320,fd=4))
- Şimdi yavaş yavaş çözelim:
- ESTAB: Bağlantı kurulmuş, veri gidip geliyor
- Yani bu geçmiş değil, canlı
- 127.0.0.1:4444 Bu makinede, 4444 portunu dinleyen bir şey var
- 127.0.0.1:46842 Bu da yine bu makineden, Rastgele (ephemeral) bir port
- ("nc",pid=2320,fd=4) : Program: nc, Kimlik: PID 2320, Network’e çıkarken kullandığı kapı: FD 4
- Burada beynine şu cümle yerleşsin: “Bir network bağlantısı = bir process + bir FD”
## Şimdi Kendimize Şu Soruyu Soruyoruz
- Bu bağlantıyı kim başlattı?
- artık PID'İ biliyoruz -> 2320
## Process Kimliğini Anlamak 
-Komut: ps -o pid,ppid,user,cmd -p 2320
- Diyelim çıktı şu: PID  PPID USER  CMD
-                   2320 2201 kali  nc -lvnp 4444
-Yorumlayalım:
- nc → elle çalıştırılmış
- kali → kullanıcı adına
- PPID 2201 → terminalden başlatılmış
- Yani: Bu trafik sistem tarafından değil, insan tarafından üretilmiş
## FD'ye Sadece Bakıyoruz (Derine Girmiyoruz)
-Komut: sudo ls -l /proc/2320/fd
- Örnek çıktı:
- 0 -> /dev/pts/0
- 1 -> /dev/pts/0
- 2 -> /dev/pts/0
- 4 -> socket:[9431]
- Bugün şunu anlaman yeterli: Process network için özel bir FD açar. Bu FD → socket → network
## Gün 15 Mantık Özeti
- Bu gün şunları öğrendik:
- Network trafiği kendiliğinden oluşmaz
- Her trafik bir process'in sonucu
- (ss) bize konuşulanları
- (ps) bize konuşanın kimliğini
- /proc/PID/FD bize nasıl konuştuğunu gösterir
## Zihinde Kazanılacak 3 Cümle 
- Network = etki
- Process = neden
- FD = mekanizma
- 
