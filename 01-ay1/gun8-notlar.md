# Ay 1 – Gün 8
## Gereksiz Servis Nedir?
- Gereksiz servis sistemin amacına hizmet etmeyen ama çalışan port açan , log tutan servistir.
-  Servis/Daemon = Arka planda sürekli çalışan,genelde açılışta başlayan programlardır.
- Ama kritik nokta şu; her çalışan servis = potansiyel risk
- Gereksiz Servis Ne Demek ?
- Sistemin amacına hizmet etmeyen, kullanılmayan ama çalışmaya devam eden servis.
- Basit Örnek;
-  Bir web sunucusu kurdun
Ama:
- Bluetooth servisi açık
- Printer servisi açık
- FTP servisi açık
## Attack Surface nedir?
-Saldırganın sisteme girebileceği tüm giriş noktalarının toplamı.
-Neler Attack Surface oluşturur? ;
-Açık servisler,açık portlar,çalışan daemonlar,zayıf konfigürasyonlar,gereksiz kullanıcılar,eski yazılımlar.
-Kural Çok Net: Ne kadar çok servis -> o kadar büyük attack surface.
## Hardening mantığı
- Hardering = sistemi gereksiz şeylerden arındırıp minumum riskle çalışır hale getirmektir.Başka bir deyişle sistemi zayıf değil sıkı yapma sanatıdır.
- Hardering Mantığı (Altın Kurallar):
🔒 1. “Gerekmiyorsa çalışmasın”
Servis lazım değil mi? → KAPAT
“Belki lazım olur” → ❌ Yanlış düşünce
🔒 2. “Minimum açık, maksimum kontrol”
Az servis
Az port
Az kullanıcı
📌 Security = sadelik
🔒 3. “Varsayılan ayar düşmandır”
Default password ❌
Default config ❌
Default user ❌
🔒 4. “Root her şeyi yapar → Risklidir”
Root ile çalışan servis = yüksek risk
User-level servis = daha kontrollü
Not:
Root ile çalışan her servis tehlikeli değildir,
ama ele geçirilirse etkisi çok daha yıkıcı olur.
6️⃣ Hardening = Saldırı Öncesi Savunma
Çok önemli bir farkı netleştireyim:
Saldırı	Hardening
Aktif	Pasif
Exploit odaklı	Önlem odaklı
Saldırgan düşüncesi	Savunmacı düşünce
“Nasıl girerim?”	“Nasıl kapatırım?”
💡 İyi hacker önce hardening bilir
## Kendi sistemim için gözlemler
- Açık ama gereksiz olabilecek servisler
- Neden riskli olabilirler?

## Güvenlik yorumu
- “Az çalışan sistem = daha güvenli sistem”
