# Ay 1 – Gün 3

# Kullanıcılar,Gruplar,Root&sudo
## Kullanıcı(user)Nedir?:
- Linux’ta her işlem (process) bir kullanıcı kimliğiyle çalışır
(normal user, system user veya root).; güvenlik için hepsi ayrı ayrı kulanılırlar.
## Kullanıcı Türleri:
- Normal Kullanıcı: Günlük kullanımlar için vardır,kendine ait dosyaları kullanabilir,sistemi bilerek veya yanlişlıkla bozamazlar.
- Sistem Kullanıcıları: Servisler içindir.
- Root Kullanıcı: Herşeye erişebilirler sistemi tamamen değiştirebilirler hata yapmaları durumunda sistem çöker.
## Grup(groups)Nedir?:
- Grup yetkileri toplu haldeyönetmenin kolay yoludur bu amaçlar oluşturulurlar 10 kullanıcı şu dosyaya erişsin demek yerine onları aynı gruba alırsın bu sayede yönetim kolaylaşır,güvenlik rtar,sistem düzenli olur.
## Root Kullanıcı(Kritik!)
- root=Güç modu
- Root kullanıcılar dosya silebilir,kullanıcı ekleyebilir,sistem ayarlarını değiştirilebilirler.Root’un yaptığı işlemler izin kontrolünden geçmez.
## Neden Sürekli root Kullanıcı olunmaz?
-Tek hatada sistem gider,güvenlik açığı oluşur,gerçek sistemlerde root ile sürekli çalışmak yasaktır / kesinlikle önerilmez..
## Sudo Nedir?
-sudo = sudo, tek komutu root yetkisiyle çalıştırır,
kullanıcı root olmaz tek seferlik root yetkisi kullan demektir.Sadece sudo yetkisine sahip grubundak kullanıcılar sudo kullanabilir.
## su ile sudo arasındaki farklar
- su: roota geçer , root şifresi gerekir , tehlikeli , sürekli yetki
- sudo: tek komut için root , kendi şifren gerekir , güvenli , geçici etki
## id Komutundaki Çıktılar
 - id ne yapar ?
 - Kullanıcının UID , GID , Gruplarını tek komutta gösterir.
 - UID(User ID):Kullanıcının sistemdeki sayısal kimliğidir.0 = root , 1-999 = sistem kullanıcıları , 1000+ = normal kullanıcılar
 - GID(Group ID):Kullanıcının birincil grubunun sayısal kimliği. gid=1000(kali) = Bu kullanıcı varsayılan olarak "kali" grubunda.
 
## Komutlar ve anlamları
- whoami: Aktif kullanıcı adını verir.
- id: Kullanıcı kimliği ve grup numarsını verir.
- groups: Üye olduğun grupları gösterir.
- sudo -l: Bu kullanıcı hangi sudo yetkilerine sahip.

## Root ve sudo
- Root neden tehlikeli?
- sudo neden kontrollü?

## Uygulama çıktıları
- whoami çıktısı: kali
- id çıktısı: uid=1000(kali) gid=1000(kali) groups=1000(kali),4(adm),20(dialout),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),100(users),101(netdev),103(scanner),107(bluetooth),125(lpadmin),133(wireshark),135(kaboxer),136(vboxsf)
- sudo -l özeti: (ALL : ALL) ALL Her kullanıcı olarak(ALL) , Her grup olarak(:ALL) , Her komutu(ALL)
                kali kullanıcısı,
                her kullanıcı ve grup adına,
                her komutu,
                sudo ile çalıştırabilir
  
                (ALL : ALL) ALL= Çok güçlü ama riskli bir yapı
                Gerçek hayatta genelde:Sınırlı sudo,belirli komutlar verilir.
## Mini senaryo cevapları
1) Neden Herkes Root olamaz? Cevap =  Root kullanıcı sistemdeki en yetkili kullanıcıdır bu da sistemdeki herşeye erişim ve sınırsız yetki demektir eğer herkese sınırsız yetki verilirse sistemde güvelik açıkları ortaya çıkar ve herhangi bir hata durumunda (yanlış kod ekleme silme gibi) sistem çöker                                                                                                      
2) Neden sudo tek komut için vardır? Cevap = Çünkü sudu tek seferlik root gibi davran demektir daha güvenliklidir kullanıcıya sürekli root yetkisi değil sadece o anlık root yetkisi verir.
3) Bir kullanıcı yanlışlıkla fazla yetki alırsa ne olur? Cevap = Eğer herhangi bir kullanıcı yanlışlıklafazka yetki alırsa bu yetki nedeniyle yanlış işlemler yaparsa sistem güvenlik açığı verebilir veya çökebilir.
