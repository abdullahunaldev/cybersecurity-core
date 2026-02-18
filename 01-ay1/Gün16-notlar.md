# Bir Sevis Sistem İçinde Nasıl Yaşar ?
## Servis Nedir?
- Servis; arka planda çalışan ve genellikle bir port dinleyen processtir.
- Örnek Servisler:
- OpenSSH -> SSH bağlantıları için
- Apache HTTP Server -> Web sunucusu
- MySQL -> Veritabanı
-Ama unutma:Servis = özel bir program değil,servis = özel şekilde çalışan bir process.
## Bir Servisin Doumu (Boot anı)
- Linux açıldığında çalışan ilk şey:
- PID1 -> systemd
- systemd şunları yapar:
- Servis dosyalarını okur, hangileri otomatik başlatılacak karar verir, process'leri başlatır.
- Yani Zincir:
- Boot -> systemd (PID 1) -> servis process’i
## Servi Nasıl Başlar?
- Bir servis geelde şu şekilde başlar:
- sudo systemctl start ssh
- Bu komut: systemd’ye “ssh servisini başlat” der, systemd ilgili binary’yi çalıştırır,yeni bir process oluşur.
- Kontrol : ps aux | grep ssh
## Servis Nasıl Yaşar (Çalışma Mantığı)
- Bir servis yaşamak için 4 şeye ihtiyaç duyar:
- 1. Process Kimliği (PID)
- Her servis bir PID alır.
- 2. Açık File Descriptor’lar (FD)
- Log dosyası,Socket,Pipe
- 3. Socket (Network Kapısı)
Örneğin: ss -lntp
- Çıktı: LISTEN 0 128 0.0.0.0:22 users:(("sshd",pid=1024,fd=3))
- Bu ne demek?
- Port 22 dinleniyor
- Dinleyen process: sshd
- PID 1024
- FD 3 üzerinden
- Yani:sshd -> FD 3 ->socket ->port 22
- 4. Bellek ve CPU
- Servis:
- RAM kullanır
- CPU kullanır
- Scheduler tarafından çalıştırılır
## Servis sürekli Nasıl Ayakata Kalır?
- Burada kritik şey:Parent-Child ilişkisi
- Birçok servis: Dinler,bağlantı gelince fork eder,yeni child process oluşturur
- Örneğin ssh:
- ssh(parent) -> ssh(child)
- Parent dinlemeye devam eder child kullanıcıyı yönetir.
## Servis Ölürse Ne Olur ?
- Eğer servis çökerse; systemd bunu fark edebilir.
- Bazı servis dosyalarında : " Restart=always " yazar bu şu demek :
- Process ölürse tekrar doğur. bu yüzden bazı malwarelarda systemd dosyası bırakılır çünkü gerçek servis gibi yaşamak isterler.
- ## Servis ile Normal Process Arasındaki Fark
- | Normal Process     | Servis                  |
- | ------------------ | ----------------------- |
- | Elle başlatılır    | Genelde otomatik başlar |
- | Terminale bağlıdır | Arka planda çalışır     |
- | Genelde geçicidir  | Sürekli yaşar           |
- | Port açmayabilir   | Genelde port açar       |
## Bir Servisin Yaşam Döngüsü
- Boot ->systemd ->Service başlatılır ->PID alır ->Socket açar -> Port dinler ->Bağlantı kabul eder ->Child process üretir ->Log yazar -> Çalışmaya devam eder
## Avcı Gözüyle Bakarsak 
- Bir servis sistemde yaşarken şunları bırakır :
- PID, parentçoğu zaman systemd), açık port, FD listesi, log dosyası, systemctl çıktısı
- Eğer bunlardan biri anormal ise şüphe başlar !!!

