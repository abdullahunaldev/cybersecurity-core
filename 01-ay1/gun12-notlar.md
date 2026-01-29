# Ay 1 – Gün 12
# Process = İnsan 
- Bir processi bir insan gibi düşün.Bir insanın dış dünya ile iletişimi nasıl olur?
- Kulak -> Dinler
- Ağız -> Konuşur
- Göz -> Görür
- Linux'ta process'in **kulak-ağız-gözleri = File Decriptor(FD)**
- ## File Descriptor nedir?
- FD = process'in"bağlantı deliği" ; ben burada okurum buradan yazarım.
- ## Her Process Doğarken 3 Kapıyla Doğar
- Bunlar **her zaman var** kaçamazsın!!
  
| Numara | Anlamı | Benzetme           |
| ------ | ------ | ------------------ |
| **0**  | STDIN  | 👂 Kulak (dinleme) |
| **1**  | STDOUT | 🗣️ Ağız (konuşma) |
| **2**  | STDERR | 😡 Bağırma (hata)  |

- **Sen Terminalde Ne Yapıyorsun?**
- ls
- Aslında olan şey bu:
  ls → 1 numaralı kapıdan konuşuyor
  Hata olursa → 2 numaralı kapıdan bağırıyor
  Girdi gerekirse → 0 numaralı kapıdan dinliyor
**Şimdi En Kritik Nokta**
- Linux'ta dosya açmak = bir kapı açmak.
- Process dosya açarsa ne olur ?
- Process derki ;
  "Ben log.txt dosyasını açmak istiyorum"
- Linux derki;
  "Tamam sana yeni bir kapı verdim"
- Bu kapının numarası;
  3
- Çünkü;
  0,1,2 dolu
  ilk boş kapı = 3
- **YANİ ŞU ANDA PROCESS’İN KAPILARI ŞÖYLE**
**Kapı (FD)** 	**Nereye bağlı**
     0 	           Klavye
     1	           Ekran
     2	           Ekran
     3	           log.txt
- Process artık 3 numaralı kapıdan log.txt ile konuşuyor
**Peki Network?**
Aynı şey.
- Process der ki:
“Bir IP’ye bağlanacağım”
- Linux der ki:
“Al sana bir kapı”
- Bu da mesela:
FD 4
**FD**	**Bağlantı**
  3       	Dosya
  4	     Network (socket)
- Linux için dosya ile internet arasında fark yok ikisi de FD
**Şimdi Asıl Mesele(Gizli Bağlantı)**
- Sen ekranda birşey göremezsin ama process şunları yapıyo olabilir:
  Bir dosyaya yazıyor
  Bir IP'ye veri gönderiyor
  Başka bir process ile konuşuyor
- Bunların hepsi arka planda FD ile olur !!!
**Linux Bunu Nerede Saklar?**
- Şurada:
  /proc/PID/fd/
- Bu klasör şunu der:
  "Bu process şu kapılardan dünyaya bağlı."
**Örneği Gözünde Canlandır**
  Bir process = oda
  Kapılar = FD
  
  Oda kapıları:
  Kapı 0 → kulak
  Kapı 1 → ağız
  Kapı 2 → bağırma
  Kapı 3 → dosya
  Kapı 4 → internet
- Sen dıiardan bakınca sessiz ama içeride internetle konuşuyor olabilir.
**Neden Hacker Konusu?**
- Çünkü:
  Reverse shell =
  Ağız + kulak internete bağlanır
  Log silinir ama FD açık kalır
  Malware gizlice FD ile veri sızdırır
  **Process, dünyayla sadece File Descriptor'lar üzerinden konuşur.**
## Açık Dosya Ne Demek?
- Linux'ta bir dosya, sadece açıkken bir process tarafından kullanılabilir.
- Yani:
  Dosya var olabilir ama proces o dosyayı açmadıysa onun için yok gibidir.
**Dosya Açılınca Ne olur?**
- Process şunu der:
  "Bu dosyayla işim var"
- Linux'ta şunu yapar:
  Dosyayı açar
  Process'E bir numara verir **(FD)**
  Process artık dosyanın adını kullanmaz, sadece FD numarasıyla konuşur.
**Çok Kritik Bilgi**
- Process dosyayla değil, FD ile konuşur:
  Yani
  Dosya silinse bile
  FD açıksa
  Process yazmaya devam eder
**Basit Senaryo**
- nano test.txt
  Ne oldu?
  nano dosyayı açtı
  Linux nanoya FD verdi
  nano artık test.txt yerine FD ile çalışıyor
  
**Dosya silinirse?**
  Başka terminalde:
  rm test.txt
  Ama nano hâlâ açık!
  Çünkü:
  FD açık
  Dosya RAM + inode üzerinden duruyor
  Diskten silindi
  Process hâlâ yazabiliyor
  İşte bu:
  Açık dosya = FD açık
## Process'in Gizli Bağlantıları Ne Demek?
- Şimdi burası çok netleşecek.Bir process şunları yapabilir:
Dosyaya yazabilir
Network’e bağlı olabilir
Başka process ile konuşabilir
Ama:
Ekrana bir şey yazmaz
Terminalde görünmez
- Bunlar gizli bağlantılar
**Bu gizli bağlantılar NEREDE?**
Cevap artık net:
File Descriptor’larda

/proc/PID/fd — GİZLİ KAPI LİSTESİ
Bir process için:
 ls -l /proc/1234/fd
Şunu görürsün:
 0 -> /dev/pts/0
 1 -> /dev/pts/0
 2 -> /dev/pts/0
 3 -> /var/log/auth.log
 4 -> socket:[123456]
**Bunu Türkçeye çevirelim**
Bu process:
 FD 0 → klavyeyi dinliyor
 FD 1 → ekrana konuşuyor
 FD 2 → hataları ekrana yazıyor
 FD 3 → log dosyasına yazıyor
 FD 4 → internete bağlı 
- Ama sen:
top’ta bakınca, ekranda bir şey görmeyebilirsin
**İşte “gizli” kelimesi buradan geliyor**
Process sessiz ama:
 Veri gönderiyor
 Log siliyor
 Arka planda konuşuyor
**Networkde Bir Dosyadır**
- Bu cümle Linux'un kalbidir;
  Yani:
  TCP bağlantısı
  UDP bağlantısı
  Hepsi:
  FD ile temsil edilir
**Malware Örneği**
 Bir zararlı yazılım:
 Ekrana yazmaz
 Terminal kullanmaz
 Ama FD 4 ile:
 4444 portuna veri gönderir
  
 Sen bakarsın:
 “Bir şey çalışmıyor gibi”
 Ama:
 lsof -i
 her şey ortaya çıkar
## lsof "Açık Dosya" Dedektörü
- lsof -p 1234
 Bu komut şunu söyler:
“Bu process hangi kapıları açık tutuyor?”
Dosya mı?
Network mü?
Log mu?
## /proc/PID/fd NEDİR? (1 cümle)
- Bir process’in dünyaya açılan TÜM kapılarının birebir listesidir.
  Dosya mı?
  Ağ mı?
  Pipe mı?
  Log mu?
  Hepsi burada.
**ANALİZE NASIL BAŞLANIR?**
- Önce PID’yi bulalım:
ps aux | grep ssh
- Diyelim PID = 2345
Şimdi:
ls -l /proc/2345/fd
**ÇIKTI NASIL OKUNUR? (EN ÖNEMLİ KISIM)**
Örnek çıktı:
 0 -> /dev/pts/1
 1 -> /dev/pts/1
 2 -> /dev/pts/1
 3 -> /var/log/auth.log
 4 -> socket:[128734]
 5 -> pipe:[445566]
Şimdi satır satır analiz 
- FD 0
 0 -> /dev/pts/1
 STDIN
 Process nereden dinliyor?
 Terminalden

- FD 1
1 -> /dev/pts/1
 STDOUT
 Process nereye konuşuyor?
 Terminale
- FD 2
2 -> /dev/pts/1
 STDERR
 Hatalar nereye gidiyor?
 Terminale
 İlk 3 normal → şüpheli değil
- FD 3
3 -> /var/log/auth.log
 DİKKAT
 Bu process:
 auth.log okuyor veya yazıyor
 Sorular:
 Bu process neden auth.log’a erişiyor?
 Root mu?
 SSH mi?
 Malware mi?
- FD 4
4 -> socket:[128734]
 KRİTİK NOKTA
 Process internete bağlı
 Ama:
 Hangi IP?
 Hangi port?
 SOCKET ANALİZİ
 ss -p | grep 2345
 veya:
 lsof -i -p 2345
 Örnek çıktı:
 TCP 192.168.1.10:22 -> 185.45.33.21:55888
 Process dış dünyayla konuşuyor
- FD 5
5 -> pipe:[445566]
 Bu process:
 Başka bir process ile sessizce konuşuyor
 Genelde:
 grep | awk
 cron job
 malware zinciri
**ŞÜPHELİ DAVRANIŞ NASIL ANLAŞILIR?**
Şimdi analist kafasıyla bakalım:
Alarm Sebepleri
Durum	Neden Şüpheli
FD → socket ama process sessiz	Gizli network
FD → silinmiş dosya	Log gizleme
STDOUT → socket	Reverse shell
Root process + açık FD	Yetki sızıntısı
FD sayısı çok fazla	Kaynak suistimali
**SİLİNMİŞ AMA AÇIK DOSYA (ÇOK KRİTİK)**
 ls -l /proc/2345/fd | grep deleted
 Çıktı:
 3 -> /var/log/syslog (deleted)
 Ne demek bu?
 Dosya diskten silinmiş
 Ama process hâlâ yazıyor
 Log gizleme tekniği

**GERÇEK HAYAT ANALİZ SENARYOSU**
- Sen görüyorsun:
  CPU düşük
  RAM normal
  Ekranda bir şey yok
  Ama:
  ls -l /proc/2345/fd
- Görüyorsun:
  4 -> socket:[999999]
  ss -p
  Uzak IP
  Bilinmeyen port
  Arka kapı (backdoor)
**HIZLI ANALİZ ŞABLONU (EZBERLE)**
- Bir PID için:
  ls -l /proc/PID/fd
  Sonra sırayla sor:
  STDIN / STDOUT normal mi?
  Dosya erişimleri mantıklı mı?
  Socket var mı?
  Pipe ile başka process var mı?
  Deleted dosya var mı?
**TEK CÜMLELİK ÖZET**
- /proc/PID/fd = process’in gizli hayatı,açık dosya, ağ, pipe, her şey burada görünür
