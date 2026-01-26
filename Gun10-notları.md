# Ay 1 – Gün 10

## Dosya izinleri nedir?
- Linux'ta her dosya ve dizin şu 3 soruya cevap verir;
  Kim?(user/group/other)
  Ne Yapabilir?(read/write/execute)
  Ne Kadar?(yetki seviyesi)
  Yani sistem şunu sorar,bu dosyaya kim ne yapabilir?
## 3 Yetki Sahibi(Kural-1)
- Her dosya aynı anda 3 taraf için izin tutar:
  | Kısaltma | Anlamı | Kim             |
  | -------- | ------ | --------------- |
  | **u**    | user   | Dosyanın sahibi |
  | **g**    | group  | Dosyanın grubu  |
  | **o**    | others | Herkes          |
- Öncelik sırası; **user** -> **group** -> **others**
## 3 Temel Yetki(Kural-2)
| Harf  | Anlam   | Dosyada          | Dizinde               |
| ----- | ------- | ---------------- | --------------------- |
| **r** | read    | Dosyayı oku      | Listele (`ls`)        |
| **w** | write   | Dosyayı değiştir | Dosya ekle/sil        |
| **x** | execute | Çalıştır         | Dizine girebil (`cd`) |
- Dizinlerde **x** çok kritiktir **x** yoksa içeri giremezsin !!!
### ls -l Okumayı Öğren 
- Komut:
  ls -l
- Örnek Çıktı:
  -rwxr-x--- 1 abdullah admin 4096 test.sh
- Bunu Parçalayalım:
  -rwxr-x---
→ dosya (d olsaydı dizin)
rwx → user
r-x → group
--- → others
- Anlamı:
Dosya sahibi → her şeyi yapar
Grup → okur & çalıştırır
Diğerleri → hiçbir şey
### chmod Yetki Değiştirme 
- Sembolik Yöntem (okunabilir)
chmod u+x dosya.sh
→ kullanıcıya çalıştırma yetkisi ver
chmod g-w dosya.sh
→ gruptan yazma yetkisini al
chmod o-r dosya.sh
→ herkesten okuma yetkisini kaldır
- Format:
chmod [u/g/o][+/-][r/w/x] dosya
### Sayısal(octal) Yöntem(siberci yöntemi)
| Yetki | Değer |
| ----- | ----- |
| r     | 4     |
| w     | 2     |
| x     | 1     |
- Topla → yaz 👇
| Değer | Anlam |
| ----- | ----- |
| 7     | rwx   |
| 6     | rw-   |
| 5     | r-x   |
| 0     | ---   |
- Örnek
  chmod 750 dosya.sh
  | User    | Group   | Others  |
  | ------- | ------- | ------- |
  | 7 (rwx) | 5 (r-x) | 0 (---) |

**750 = profesyonel ve kontrollü güvenlik ayarıdır**
### chown Sahiplik Değiştirme 
- Yetki yetmez sahip de önemlidir.
  chown abdullah dosya.txt
  → sahibi değiştirir
  chown abdullah:admin dosya.txt
  → user + group değiştirir
  chown -R www-data:www-data /var/www/
  → dizin ve içindekiler (recursive)
  Yanlış chown = servis çöker
- Not:
  Dosya izinleri yanlışsa, servis doğru yapılandırılmış olsa bile güvenlik zayıflar.
### Güvenlik Mantığı (En Önemli Kısım)
- Kötü Örnek:
  chmod 777 dosya
  Bu ne demek?
  Herkes
  Her şeyi
  Yapabilir
- Hacker cümlesi:
  “777 mi? Güzel…”
- Doğru Yaklaşım:
  Dosya çalışacak mı? → x
  Okunacak mı? → r
  Değiştirilecek mi? → w
  Gerekmeyen yetki VERİLMEZ
### Gerçek Senaryo (Siber Güvenlik)
- Web dizini:
  /var/www/html
- Doğru ayar:
  chown -R www-data:www-data /var/www/html
  chmod -R 750 /var/www/html
- Ne olur?
  Web servisi çalışır
  Dışarıdan yazılamaz
  Saldırı etkisi sınırlı kalır
  Least Privilege hayatta kalır
### Özet (Ezber Değil, Mantık)
Dosya izinleri = son savunma hattı
chmod → ne yapabilir?
chown → kim yapabilir?
777 → alarm
750 / 640 → profesyonel
## r w x anlamları
- Dosya için
  r → Dosyayı oku
  w → Dosyayı değiştir
  x → Dosyayı çalıştır
- Dizin için
  r → Listeleyebilirsin (ls)
  w → İçine dosya ekle/sil
  x → Dizinin içine girebilirsin (cd)
## Güvenlik yorumları
- 777 neden tehlikeli?
  chmod 777
  Herkes yazabilir
  Herkes çalıştırabilir
- Write yetkisi neden kritik?
  Bir saldırgan:
  senin grubuna dahil olursa
  group write varsa
  → dosyayı sessizce değiştirebilir
  Bu yüzden profesyonel yaklaşım:
  Yazma yetkisi = sadece gerçekten yazması gereken kişide olmalı
