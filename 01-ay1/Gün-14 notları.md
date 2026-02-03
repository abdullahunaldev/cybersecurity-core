## Process + Network = Canlı Trafik Analizi
- (Örneklerle, adım adım)
  ## 0 Hikâyeyi Kuralım (Önce Zihinde Oturtalım)
- Bir Linux sistemdesin.
- Bir gün şunu düşünüyorsun:
- “Bu makine şu anda internette ne yapıyor ve bunu kim yapıyor?”
- İşte Process + Network analizi bu soruya cevap verir.
## 1 Process Nedir? (Kısa ama Net)
- Linux’ta:
- Her çalışan program = process
- Her process’in = PID’i (kimliği) vardır
- Aynı program:
- 1 kez çalışır → 1 PID
- 10 kez çalışır → 10 farklı PID
- **PID = canlı örnek**
## 2 Network Tarafı: Sistemde Şu An Ne Konuşuyor?
- Komut: ss -ntp
- Bu komut şunu sorar: “Şu anda TCP üzerinden kim kiminle konuşuyor ve hangi process konuşuyor?”
- Gerçek örnek: ESTAB 0 0 127.0.0.1:4444   127.0.0.1:46842 users:(("nc",pid=2320,fd=4))
## 3 Bu Satırı İnsan Diline Çeviriyoruz
- **Parça parça:**
- ESTAB
- **Bağlantı aktif, veri gidip geliyor**
- 127.0.0.1:4444
- **Bu taraf dinleyen taraf**
- 127.0.0.1:46842
- **Bu taraf bağlanan taraf**
- ("nc",pid=2320,fd=4)
- **Bu bağlantıyı açan:**
- Program: nc
- Kimlik: PID 2320
- Dosya numarası: FD 4
- **Özet cümle:** PID 2320 numaralı nc process’i, FD 4 üzerinden 4444 portunda canlı bağlantı yapıyor.
## 4 Şimdi Network’ten Process’e Geçiyoruz
- Artık biliyoruz:
- Şüpheli / merak edilen şey: **PID 2320**
- **Kim bu?**
- ps -o pid,ppid,user,cmd -p 2320
- **Diyelim çıktı şu:**
- PID  PPID USER  CMD
- 2320  2201 kali  nc -lvnp 4444
- **Bunu nasıl okuruz?**
-| Alan | Anlamı                  |
-| ---- | ----------------------- |
-| PID  | Kimlik                  |
-| PPID | Bunu kim başlattı       |
-| USER | Kimin adına             |
-| CMD  | Tam olarak ne çalışıyor |
- Yorum:
- Bu process elle başlatılmış
- Sistem servisi değil
- Bilinçli bir komut
## 5 File Descriptor (FD) Nedir? (Kritik Nokta)
- Linux’ta **her şey dosyadır:**
Terminal → dosya
Log → dosya
Network socket → dosya
**Process’in açtığı her şeye bir numara verilir:**
-| FD | Ne olabilir           |
-| -- | --------------------- |
-| 0  | stdin                 |
-| 1  | stdout                |
-| 2  | stderr                |
-| 3+ | dosya / socket / pipe |
## 6 FD'leri Görmek(Somutlaştırma)
- sudo ls -l /proc/2320/fd
- Örnwk Çıktı :
- 0 -> /dev/pts/0
- 1 -> /dev/pts/0
- 2 -> /dev/pts/0
- 4 -> socket:[9431]
- **Ne oldu şimdi?**
- FD 4 → bir socket
- Socket ID → 9431
- Bu socket az önce ss çıktısında gördüğümüz bağlantı
- Yani:
- Process → FD → Socket → Network
## 7 Zinciri Net Şekilde Yazalım 
- nc (PID 2320)
   |
   ├─ FD 0 → terminal
   ├─ FD 1 → terminal
   ├─ FD 2 → terminal
   └─ FD 4 → socket → TCP 4444
- Bu canlı davranış haritasıdır.
## 8 Neden Bazen "ss | grep socketID" Çıkmıyor?
- Çünkü:
- ss port/IP üzerinden çalışır
- /proc ise çekirdek içi ID gösterir
- Bu yüzden doğru yol:
- ss -ntp | grep PID
- socket ID her zaman eşleşmeyebilir → bu normal
## 9 Gerçek Hayat Av Senaryosu
- Diyelim ki:
- ss -ntp
- şunu gösterdi:python,bash,nc
- ve bunlar:dış IP’ye bağlı,alışılmadık portta
- İşte burada ders değil olay müdahalesi başlar.
- Sorular:
- Bu process burada neden var?
- Kim başlattı?
- Kalıcı mı?
- Log bıraktı mı?
### Altın Mantık
- Network “ne oluyor” der
- Process “kim yapıyor” der
- FD “nasıl yapıyor” der
- Üçü birleşince:
- **CANLI ANALİZ**
