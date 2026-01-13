# Ay 1 – Gün 5
# En Temelden 
## Bilgisayar Aslında Ne yapar 
- Bilgisayarın yaptığı şey şudur:
- Program çalıştırır
- Bellek ve CPU verir
- Giriş/çıkış yapar(disk ,ağ,ekran)
- Bu yüzden herşeyin başlangıç noktası = **PROCESS**
## Process Nedir
-Process,işletim sisteminin şu kararıdır;bu program artık çalışıyor ve ona kaynak verdim.
-Bir process'in mutlaka şunları vardır
 -PID -> Kimliği
 -Owner(user/root) ->Sahibi
 -Memory -> Ram'de yeri
 -File descriptorlar -> Açık dosyalar/soketler
 **Network bağlantısı = bir file descriptordur.**
## Network Nedir
 -Network işletim sistemi için başka bir makineyle veri alışverişi yapabileceği yerdir kanaldır.
## Data Akışı Nasıl Olur
  -Process socket açar
  -OS TCP/IP stack’e verir
  -Paketlere bölünür
  -Network kartından çıkar
  -Karşı tarafın socket’ine girer
  -Karşı process’e teslim edilir
  **📌 Process ↔ Process konuşur
       Network sadece aracıdır.**
## Yetki(Root/User) Neden Önemli
 -Root process → Sistemi temsil eder.
 -User process → Sınırlıdır.
 -Root + açık port → sistem ele geçirilebilir
 -User + açık port → user seviyesinde risk
## Güvenlikte Bu Neden Kritik
-Çünkü saldırgan şunu yapar
 -Açık portu tarar
 -Portun arkasındaki processi bulur 
 -Versiyon/davranışı analiz eder
 -Exploit dener
 -Yani hedef port değil Process!!
## Kavram Haritası
 Program
   ↓
Process (PID, user)
   ↓
Socket açar
   ↓
Port bağlanır
   ↓
LISTEN / CONNECT
   ↓
Network iletişimi

## Process ↔ Network ilişkisi
- Bir process neden port açar? 
 Aynı Ip üzerinde çalışan processler başka bir kaynaktan veri alış verişi yaparken yanlış verinin yanlış process'e gitmemesi için kendilerine belli portlar açar ve o portlar üzerinden veri alışverişi yaparlar.
## Port kavramı
-Socket Nedir?
 -Process networke direk çıkmaz.Socket açar, o socketi bir porta bağlar ve son olarak dinler veya bağlanır.**Socket** process ile network arasındaki kapıdır. 
- Port nedir?
 -Aynı Ip üzerindeki processleri ayırmak için kullanılan numaralardır.
  IP-> Hangi bilgisayar
  Port-> O bilgisayardaki hangi process.
-Listen Nedir?
 -Processin biri gelirse buradayım konuşurum halidir yani socket açıldı, porta bağlandı ve dinlemeye geçti bu yüzden Lısten = potansiyel saldırı yüzeyi.
## Komutlar
- ss -tuln: Açık portları görmek için kullanılan komuttur.
- ss -tulnp:Hangi process bu portu açmış görmemizi sağlayan komuttur.

## Gözlemler
- Açık portlar:
- Netid  State  Recv-Q  Send-Q  Local Address:Port  Peer Address:Port
**Bu şekilde herhangi bir çıktı olmadığında açık(listen;)olan hiçbir port yoktur.
- İlgili process’ler:
- tcp    LISTEN   0   5   0.0.0.0:8000   0.0.0.0:*   users:(("python3",pid=5365,fd=3))
**Servis tcp üzerinden çalışıyor / Listen bu servis dinlemede bağlantı bekliyor / 0.0.0.0:8000 tüm networ arayüzleri local LAN dış ağ**

## Güvenlik yorumları
- Gereksiz açık port neden tehlikelidir?
 Eğer sistemde gereksiz açık port varsa buraya erişmek isteyen kötü niyetli bir Hackerın isteyeceği şey budur çünkü port açık demek sistem erişime açık demek ve eğer o port farkında olmadan root üzerinden açıldı ise sistem tamamen risk altında demektir.
