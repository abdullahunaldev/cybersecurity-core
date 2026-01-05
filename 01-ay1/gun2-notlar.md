- Linux Dizin yapısı
-  Dizin: Linuxta dosyaları ve dizinleri düzenlemek için kullanılan klasörlerdir.Linux’ta neredeyse her şey bir dosya gibi temsil edilir
(cihazlar, process bilgileri, socket’ler)..
-   Kök Dizin: Linuxun başlangıç noktasıdır,tüm dizinler buradan dallanır.
-    En Önemli Ana Dizinler:
-     /home:Kullanıcılara ait dizin burasıdır.
-     /root:Admin kullanıcıya ait olan dizindir,normal kullanıcıların varsayılan olarak yetkisi yoktur..
-      Not:home/root diye bir dizin yapısı yoktur
-     /etc:Sistem ayarları bu dizinde yer alır.
-     /var:Değişken(veriable) veriler burada yer alır.Loglar,mailler,web server dosyaları gibi.
-     /tmp:Geçici dosyalar bu dizin de yer alır.Herkes yazabilir sistem kapatılınca yazılanlar silinir.
-     /bin:Sistemin açılmasında mutlaka gereken komutlar burada yer alır.
-     /sbin:Sistem yöneticisine (root) ait komutlar bu diznde yer alır normal kullanıcılar çalıştıramaz,ama bazı sistemlerde dosyaları görebilir..
-     /usr: Kullanıcı programları,ktüphaneler,sonradan kurulan yazılımların bulunduğu dizindir.
-     /dev:Donanımların dosya gibi bulunduğu temsil edildiği dizindir.
-     /proc:Sanal dizindir canlı sistem bilgileri verir.
-     /boot:Sistemin açılış dosyalrı bu dzinde bulunur yanlış bir silme yapılması halinde sistem açılmaz.
- Linux Dosya İzinleri
   -Dosya Türü:
-   "-" normal dosya , "d" dizin, "l" link(shortcut)
   -Yetkiler:
    -Dosya İçin:"r" read(oku) , "w" write(değiştir) , "x" execute(çalıştır)
    -Dizin İçin:"r" read(listele), "w" write(ekle/sil) , "x" execute(içine gir)
-Owner , Group , Others
 -rwxr-xr--
 "-"(dosya) , rwx(owner) , r-x(group) , r--(others) 
     
