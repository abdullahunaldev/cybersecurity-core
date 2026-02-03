# Process + Network + FD = Canlı Analiz (Avcılığın Başlangıcı)

-Bu doküman, Linux sistemlerde **canlı analiz (live analysis)** yaparken
 bir process’in **gerçekten ne yaptığını** anlamak için kullanılan
 en temel ve en güçlü yaklaşımı anlatır:

> **Process + Network + File Descriptor (FD)**

-Bu üçlü birlikte okunmadan yapılan analizler eksik kalır.

---

## 1. Temel Fikir 

-Bir saldırı veya şüpheli davranış her zaman şu üç izden en az birini bırakır:

- Çalışan bir **process**
- Bir **network iletişimi**
- Bu iletişimi mümkün kılan **FD’ler**

-Bu yüzden analiz şu sorularla başlar:

-1. Hangi process çalışıyor?
-2. Network ile konuşuyor mu?
-3. Hangi FD’leri kullanıyor?

---

## 2. Process Nedir? (Kim Çalışıyor?)

-Linux’ta çalışan her program bir **process**tir.

-Her process:
- Benzersiz bir **PID** alır
- Bir kullanıcıya aittir
- Bir parent process tarafından başlatılır

### Kimlik Analizi
- ps -o pid,ppid,user,cmd -p PID
### Bu KOmut Ne Yapar?
-| Alan | Anlamı                          |
-| ---- | ------------------------------- |
-| PID  | Process ID (benzersiz kimlik)   |
-| PPID | Parent PID (kim başlatmış)      |
-| USER | Hangi kullanıcı adına çalışıyor |
-| CMD  | Çalışan komut / binary          |

### Neden Bu naliz İlk adımdır?
- Bir processi anlamadan:
- ağ bağlantısını
- açık dosyalarını(FD)
- davranışını, analiz edemezsin.
- **Kural** Prosess kimliği -> herşeyin başlangıcıdır.
### Örnek Üzerinden Okuma 
PID    PPID USER     CMD
1049      1 kali     /usr/bin/ssh-agent -s
### Buradan ne anlıyoruz?
- PID 1049 → incelenen process
- PPID 1 → systemd başlatmış
- USER kali → kullanıcı seviyesinde
- ssh-agent → anahtarlara erişen bir ajan
- 📌 Bu noktada şunu sorarız:
- Ağ bağlantısı var mı?
- Socket açmış mı?
- FD’leri neler?
## Analiz Zinciri
-Process Kimliği
      ↓
-File Descriptors (FD)
      ↓
-Socket / Pipe / File
      ↓
-Network İletişimi
      ↓
-Davranış Analizi
### Mini Hatırlatma 
- Eğer şunu görüyorsan:
- ls: cannot open directory '/proc/1049/fd': Permission denied
- Bu hata değil güvenliktir. Çözümü:
- sudo ls -l /proc/1049/fd

## 3.Network: Process Dış Dünya ile Konuşuyor mu?
- Bir process zararlıysa çoğu zaman:
 - Veri alır
 - Veri gönderir
 - Komut alır
 - Bunun için network kullanır.
 - Aktif TCP bağlantıları
 - sudo ss -ntp
- Örnek:
- ESTAB 127.0.0.1:4444 <-> 127.0.0.1:46842 users:(("nc",pid=2394,fd=3))
 - Bu satır bize şunu söyler:
 - ESTAB → aktif konuşma var
 - PID → hangi process
 - FD → network hangi kapıdan yapılıyor
 - **Network tek başına alarm değildir, ama güçlü bir sinyaldir.**

## 4. File Descriptor (FD) Nedir? (Gerçek Kanıt)
- File Descriptor, bir process’in dünyaya açılan kapılarıdır.
- Her process en az şu FD’lere sahiptir:
-| FD | Anlam          |
-| -- | -------------- |
-| 0  | STDIN (girdi)  |
-| 1  | STDOUT (çıktı) |
-| 2  | STDERR (hata)  |

-FD’ler şunlara işaret edebilir:
 - Dosya
 - Socket
 - Pipe
 - Device
## 5. FD’ler Nereden Görülür?
- Her process’in FD’leri:
 - /proc/PID/fd/
- Komut:
 - sudo ls -l /proc/PID/fd
- Bu dizin canlı analizde en kritik noktadır.
## 6. FD Çıktısı Nasıl Yorumlanır?
- **Normal / Sessiz Process0 -> /dev/null**
- 1 -> /dev/null
- 2 -> /dev/null
- 3 -> socket:[9431]
- Yorum:
- Arka planda çalışıyor
- Terminale bağlı değil
- Yerel bir socket kullanıyor
- Çoğu servis için normal
- **Şüpheli / Alarm Profili**
- 0 -> socket:[1234]
- 1 -> socket:[1234]
- 2 -> socket:[1234]
- Yorum:
- Girdi ağdan geliyor
- Çıktı ağa gidiyor
- Process ağ üzerinden kontrol ediliyor
- Bu yapı gerçek hayatta:
- Reverse shell
- Backdoor
- C2 bağlantısı olabilir.
## 7. Socket Gördüm → Alarm mı?
- Hayır.
- Socket türleri:
- TCP/UDP socket → internet olabilir
- UNIX socket → yerel
- Abstract UNIX socket → disk izi yok
- Bu yüzden:
- Socket + bağlam birlikte değerlendirilmelidir.
## 8. Zincir Mantığı (Asıl Olay)
- Gerçek analiz şu sırayla yapılır:
- Process kim?
-   (ps)
- Network var mı?
    (ss)
- FD’ler nereye bağlı?
   (/proc/PID/fd)
- Bu üçü tek tek değil, birlikte okunur.
## 9. Avcı (SOC / DFIR) Bakış Açısı
- **Dikkat edilmesi gereken durumlar:**
- ESTABLISHED TCP bağlantı
- Dış IP ile iletişim
- FD 0/1/2 doğrudan socket’e bağlı
- Arka planda sessiz çalışan process
- **Normal olabilecek durumlar:**
- UNIX socket
- /dev/null’a yönlendirilmiş FD’ler
- Bilinen servisler (ssh-agent, systemd servisleri)
# 10 Altın Kural 
- Her saldırı bir FD izi bırakır ama her FD izi saldırı değildir.
- Canlı analiz:
- Bağlam
- Davranış
- Teknik İzler , birlikte değerlendirildiğinde anlam kazanır.
