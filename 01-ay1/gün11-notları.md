# Ay 1 – Gün 11
## Terminale ls Yazdığında Ne Olur?
- Sen şunu yazıyorsun:
  ls
- Bilgisayar şunu düşünmez
  aaa ls komutu var
- Bilgisayar şunu düşünür
  ls diye bir dosya var mı, varsa nerede?
- **Çünkü Linux'ta her komut aslında bir dosyadır.**
## ls Gerçekte Nedir?
- /bin/ls
- Yani
  ls -> kısayol gibi
  /bin/ls -> gerçek dosya
# PATH Nedir?
- PATH, sistemin bir komut yazıldığında onu nereden çalıştıracağını söyleyen listedir."Bir komut yazıldığında şu klasörlere sırayla bak." 
## Ne Anlama Gelir?
- Sen terminale şunu yazdığında
  ls
- Sistem şunu yapar:
  /usr/local/sbin/ls var mı ?
  Yoksa /usr/local/bin/ls
  Yoksa /usr/bin/ls
  Bulunca orada çalıştırır.
  **İlk bulduğunu çalıştırır**
## PATH olmasaydı?
- PATH olmasaydı:
  ls
  Çalışmazdı
  Ama şunu yazarsan:
  /bin/ls
  Çalışır
  Çünkü:
  PATH = arama kolaylığı
  Tam yol = doğrudan adres
## Tehlike Nerede Başlıyor?
- Diyelim ki senin klasöründe şu dosya var
  /home/abdullah/ls
- Bu dosya zararsız görünüyor, ama aslda farklı bir şey yapıyor.
- Normalde
  ls
  /bin/ls çalışır
- Ama eğer PATH şöyleyse
  PATH=/home/abdullah:$PATH
- Bilgisayar artık şunu yapar:
  /home/abdullah/ls var mı? 
  Onu çalıştırır
  /bin/ls’ye bakmaz bile
  İşte olay burada.
## Environment Variable Ne Demek ?
- Environment variable, programlara arka planda verilen bilgidir.
- Mesela:
  HOME → ev dizinin neresi?
  USER → kimsin?
  PATH → nerelere bakayım?
  Bunları programlar sorgulamadan kabul eder.
  Yani:
  “Bana ne verirsen, ben ona göre davranırım.”
## sudo Bu İşin Neresinde ?
- sudo, sana geçici olarak şunu der:
  Tamam, bu komutu root gibi çalıştırabilirsin.
  Ama kritik nokta:
  sudo, komutu çalıştırırkenortamı da kullanır.
  Yani:
  PATH
  Bazı environment değişkenleri
  Eğer bunlar temizlenmezse:
  Root kandırabilir
  Örnek:
  Senin PATH’in bozuk:
  /home/abdullah/bin:/usr/bin:/bin
  Ve /home/abdullah/bin/ls var.
  Sen yazıyorsun:
  sudo ls
  Bilgisayarın iç sesi:
  “Root oluyorum ama PATH’e bakıyorum…
  İlk ls burada… çalıştırayım.”
  Sahte ls, root yetkisiyle çalıştı.
## ## O Yüzden sudo Kullanıcı PATH’ine Güvenmez
- İşte bu yüzden sudo şöyle der:
  secure_path = /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
  Yani:
  “Kendi güvenli PATH’imi kullanırım, seninkini çöpe atarım.”
  Ve:
  env_reset
  “Ortam değişkenlerinin çoğunu sıfırlarım.”
## Güvenli Alışkanlıklar
 Tam yol kullan
 sudo /bin/ls
 sudo /usr/bin/python3
 Ortamı temizle
 sudo -i
 PATH’e . ekleme
 PATH=.:$PATH  
## Gerçek Hayat Saldırı Özeti
1-Kullanıcının PATH’i zayıf
2-Sahte komut yerleştirilir
3-sudo çağrılır
4-Sahte komut root olur
5-Log yok, gürültü yok
İşte buna:
Görünmeyen saldırı yüzeyi denir
## Mini Özet
  ls = dosya
  PATH = “nerelere bakayım?” listesi
  İlk bulunan çalışır
  Environment variable = programın körü körüne güvendiği ayar
  sudo = root ama temkinli
  Kötü PATH + sudo = sessiz felaket
