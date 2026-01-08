# Ay 1 – Gün 4

## Program vs Process
- Program:Disk üzerinde yüklenen ve pasif halde duran dosya
- Process:Ram üzerinde çalışan programların aktif hali
- Örnek:/usr/bin/firefox = program
        firefox'u açtığında process oluşur
          
## Komutlar
- ps:Sadece bu terminale ait processleri görürsün.
- ps aux:Sistemde çalışan tüm processleri listeler.
  -ps aux komutu ile çıkan en kritik 3 kolon :
| Kolon       | Anlamı                       | Güvenlikte Önemi              |
| ----------- | ---------------------------- | ----------------------------- |
| **USER**    | Hangi kullanıcı çalıştırıyor | Root mu, normal kullanıcı mı? |
| **PID**     | Process ID                   | Takip ve analiz için          |
| **COMMAND** | Çalışan komut                | Gerçek iş burada              |

- top:canlı çalışan processleri izlemeyi sağlayan komut
Bu komut:Canlı CPU / RAM kullanımını gösterir
         Sistem yükünü anlık görmeni sağlar

## PROCESS – KULLANICI – YETKİ İLİŞKİSİ
-Her process bir kullanıcı adına çalışır.Kullanıcı yoksa process'de yoktur.
# Root proces ve User process farkları 
| Tür              | Açıklama                 | Risk          |
| ---------------- | ------------------------ | ------------- |
| **root process** | Sistemin tamamına erişir | 🔴 Yüksek     |
| **user process** | Kendi alanıyla sınırlı   | 🟢 Daha düşük |

-Her root process kötü değildir,ama gereksiz root process = risk

# Rİskli Durumlar
-Gereksiz çalışan servisler
-Root yetkili ama amacı belirsiz process’ler
-Tanımadığın isimler
-Nereden geldiği belli olmayan programlar
 -Gerçek hayatta saldırganlar:
-Process’lerini saklar
-Masum isimler verir
-Sürekli çalıştırmaz (gözden kaçsın diye)
## 🔍 Root Process vs User Process Ayrımı
-🧑 User Process (Kullanıcı Process’i)
Kim başlatır?
Sen (kali, abdullah, user vs.)
Ne zaman çalışır?
Sen bir şey açtığında
Örnekler:
Terminal
Tarayıcı
Dosya yöneticisi
Metin editörü
📌 Mantık:
“Ben açtıysam → büyük ihtimalle user process”

-👑 Root Process (Sistem Process’i)
Kim başlatır?
Sistem (boot sırasında veya servis olarak)
Ne zaman çalışır?
Sistem açılırken
Arka planda sürekli
Örnekler:
Ağ servisleri
Log servisleri
Sistem yöneticileri
📌 Mantık:
“Ben açmadıysam ama çalışıyorsa → sistem/root process”
-Gerçek Hayat Benzetmesi (Çok Netleştirir)
🏢 Bilgisayar = Bina

Bina Örneği	Linux Karşılığı
Elektrik sistemi	Root process
Asansör	Root process
Güvenlik kamerası	Root process
Senin odandaki lamba	User process
Senin bilgisayarın	User process
📌 Elektrik kesilirse → bina çöker
📌 Odandaki lambayı kapatırsan → sadece sen etkilenirsin
## Gözlemler
- Root process örnekleri:
  | Process          | Ne İşe Yarar?          |
| ---------------- | ---------------------- |
| `systemd`        | Tüm sistemin **anası** |
| `init`           | systemd’nin eski hali  |
| `sshd`           | SSH servisi            |
| `NetworkManager` | Ağ bağlantıları        |
| `cron`           | Zamanlanmış görevler   |
| `rsyslogd`       | Log toplar             |
| `dbus-daemon`    | Sistem iletişimi       |
| `udevd`          | Donanım algılama       |
| `cupsd`          | Yazdırma servisi       |
| `avahi-daemon`   | Ağ keşfi               |
| `polkitd`        | Yetki kontrolü         |

- User process örnekleri:
  | Process               | Neden User Process?               |
| --------------------- | --------------------------------- |
| `bash`                | Terminali **sen açtın**           |
| `zsh`                 | Kullanıcı shell’i                 |
| `firefox`             | Tarayıcıyı **sen başlattın**      |
| `chrome`              | Kullanıcı uygulaması              |
| `gedit`               | Metin editörü                     |
| `nano`                | Terminalden açılan editör         |
| `vim`                 | Kullanıcı tarafından çalıştırılır |
| `code`                | VS Code                           |
| `thunar` / `nautilus` | Dosya yöneticisi                  |
| `python script.py`    | Kullanıcı script’i                |
| `java -jar app.jar`   | Kullanıcı uygulaması              |

KARŞILAŞTIRMALI TABLO
| Özellik          | User Process            | Root Process     |
| ---------------- | ----------------------- | ---------------- |
| Kim başlatır     | Kullanıcı               | Sistem           |
| Ne zaman çalışır | Kullanıcı açınca        | Boot sırasında   |
| Yetki            | Sınırlı                 | Tam              |
| Risk             | Düşük                   | Yüksek           |
| Kapatınca        | Sadece sen etkilenirsin | Sistem etkilenir |

## Güvenlik notları
- Gereksiz process neden risklidir?
- Ne kadar çok process çalışırsa o kadar çalışan proceslerin çalıştırdığı servisler olur bu da o kadar çok açık ihtimalini ortaya çıkarır.
- Root process neden dikkat ister?
- Root processler tüm sisteme erişebilen servislerdir eğer başka birisi tarafından ele geçirilirse sistem artık o saldırgana ait demektir.
