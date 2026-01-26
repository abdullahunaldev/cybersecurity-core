# Ay 1 – Gün 9

## Kullanıcı & Grup (güvenlik)
## Kullanıcı nedir?
- Bir Linux/Unix sisteminde her işlem (process) bir kullanıcı adına çalışır.Yani sistem şunu sorar;
  Bu işi kim yapıyor ve yetkisi ne?
## Temel Kullanıcı Türleri
### Root
  UID = 0
  Root (Sistemin En Yetkili Abisi)
  Herşeye erişir herşeyi bozar da düzeltir de.
### Normal Kullanıcılar
  Sınırlı yetki
  Kendi dosyalarına erişir
### Sistem Kullanıcıları
  www-data , ssh , mysql gibi servisleri çalıştırır.
- Altın Kural
  Bir işlemin root olarak yapılması gerekmiyorsa root olmamalı.
## Grup nedir?
- Gruplar, yetkileri tek tek kullanıcıya vermek yerine topluca yönetme yönetimidir.
- Mantık çok basit
  Kullanıcı -> gruba girer
  Grup -> belirli yetkilere sahiptir
  Kullanıcı -> o yetkileri dolaylı alır
### Gruplar Neden Kritik
  Yetki kontrollü dağılır
  Kimin ne yaptığı izlenebilir
  Yetki geri almak tek satır komut
  Güvenlikte bu çok önemlidir yetki vermek kolaydır ama almak zordur bu yüzden gruplar önemli.
## Least Privilege(Yetki kısıtlama sanatı)
Bir kullanıcıya/servise sadece ihtyacı olan kadar yetki ver,fazlası risk demektir.
### Neden Bu kadar Önemli
-Şu senaryoyu düşün:
Bir web servisi (apache, nginx)
root yetkisiyle çalışıyor
İçinde bir zafiyet var
-Saldırgan ne kazanır?
Direkt root erişimi
Tüm sistem gitti 
-Aynı servis:
www-data kullanıcısıyla çalışıyor
Sadece web klasörüne erişimi var
-Saldırgan:
Sadece o alanla sınırlı kalır
Sistem hayatta kalır
İşte buna damage control denir.
## Least Privilege Nerelerde Uygulanır?
- Kullanıcılar
Günlük iş → normal user
Yönetim işi → sudo (geçici)
Sürekli root login
Normal user + sudo

- Servisler
Her servis:
Kendi kullanıcısıyla
Kendi klasörüne erişerek
Gerekli portlarla sınırlı çalışır

- Dosya & Dizinler
Her dosya için sistem şunu sorar:
Kim okuyabilir?
Kim yazabilir?
Kim çalıştırabilir?
Bu yüzden:
chmod
chown
komutları güvenlik araçlarıdır, sadece Linux komutu değil.

- Fazla Yetkinin Tehlikeleri
Yanlışlıkla sistem dosyası silme
Zararlı yazılımın yayılması
Log’ların silinmesi (iz kaybı)
Tüm sistemin ele geçirilmesi
Siber güvenlikte en çok sömürülen şey:
“Bu neden root yetkisiyle çalışıyor?”

- Gerçek Hayat Benzetmesi
root → Hastanenin başhekimi + güvenlik + IT + anahtarlar
normal user → Doktor
grup → Kardiyoloji servisi
least privilege → Doktor sadece kendi servisine girer
Kimseye:
“Belki lazım olur” diye
tüm kapıların anahtarı verilmez.            

- Özet (Bunu kafana kazı)
Kullanıcı = Kim?
Grup = Hangi yetkiler topluluğu?
Least Privilege = En az yetki, en az zarar
Sürekli root = Davetiye
Normal user + sudo = Profesyonel yaklaşım
- Not:
Least privilege sadece kullanıcılar için değil,
uygulamalar, container’lar ve servis hesapları için de geçerlidir.

## Komutlar
- whoami
- id
- groups
- sudo -l

## Güvenlik yorumu
- Yanlış yetkilendirme neden tehlikelidir?
