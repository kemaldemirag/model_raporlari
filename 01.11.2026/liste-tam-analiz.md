# "Katkı Yaptığım Repolar – current" Listesi: Tam Analiz (Güncellenmiş)

**Kaynak:** https://github.com/stars/kemaldemirag/lists/katk%C4%B1-yapt%C4%B1%C4%9F%C4%B1m-repolar-current/
**Tarih:** 1 Kasım 2026

Liste değişmiş. Önceki analiz 3 repo içeriyordu; şimdi **5 repo** var:

1. [Miceuz/i2c-moisture-sensor](https://github.com/Miceuz/i2c-moisture-sensor) (analiz edildi)
2. [brianbennett/fpga_nes](https://github.com/brianbennett/fpga_nes) (analiz edildi)
3. [jamieiles/80x86](https://github.com/jamieiles/80x86) (analiz edildi)
4. **[dubemezeagwu/ble_gatt_flutter](https://github.com/dubemezeagwu/ble_gatt_flutter) (yeni)**
5. **[jenschr/Teensy-4.1-example](https://github.com/jenschr/Teensy-4.1-example) (yeni)**

Bu raporun önceki bölümünde üç eski repo analiz edildi. Şimdi iki yeni repo aynı derinlikte inceleniyor ve tüm liste değerlendiriliyor.

## Yeni repo: dubemezeagwu/ble_gatt_flutter

### Genel bilgi

| Özellik | Değer |
|---|---|
| Açıklama | Bluetooth Low Energy keşif uygulaması, Flutter ile |
| Dil | Dart |
| Yıldız / Fork | 1 / 0 |
| Son güncelleme | 4 Ağustos 2026 |
| Arşivlenmiş | Hayır |
| Issue'lar | 8, hepsi kapalı |
| Lisans | Yok |

Bakımcı: [Dubem Ezeagwu](https://github.com/dubemezeagwu) (Ontario, Kanada; @decadevs).

### Kullanılabilirlik

Bu, tek kişilik bir uygulama projesi; topluluk katkısı beklemiyor. Issue'ları tamamen kişisel geliştirme adımları: "Dev", "chore: Update project exclusions", "main: fix on android permission for version 12+", "main: final cleanup and validation before submission" vb. Son taahhütler: "Fix project name from 'LE' to 'BLE'", "Rename project in README", "feat: added dynamic timeout to the flushBuffer based on buffer size", "refactor: moved buffer to main thread and renamed files for unambiguity".

**Sonuç:** Bu repo **gerçek bir açık kaynak katkı hedefi değil**. 1 yıldız, 0 fork ve tüm issue'lar kapalı. Bir tek kişinin geliştirme geçmişi olarak işe yarar. Katkı yapmaya çalışmak mantıklı değil; herhangi bir açık issue yok ve bakımcı yeni özellik taleplerine açık değil.

**Kariyer uyumu:** Bu projenin konusu (Flutter, BLE) senin ilgi alanlarına (FPGA, ASIC, gömülü, robotik, PCB) uymuyor. Muhtemelen bu listeye yanlışlıkla eklenmiş ya da yalnızca referans için tutulmuş.

**Öneri:** Listeden çıkar.

## Yeni repo: jenschr/Teensy-4.1-example

### Genel bilgi

| Özellik | Değer |
|---|---|
| Açıklama | KiCad dosyaları ile özel bir Teensy 4.1 yapımı; resmi Teensy şemalarına dayanıyor. Test edildi ve doğrulandı. |
| Ana dosyalar | `Teensy4.1-example.kicad_sch`, `Teensy4.1-example.kicad_pcb`, `MIMXRT1062DVJ6B-pcb-part-libraries`, `MIMXRT1062DVJ6B` |
| Yıldız / Fork | 125 / 32 |
| Son güncelleme | 2 Ocak 2025 |
| Arşivlenmiş | Hayır |
| Lisans | MIT |
| Issue'lar | 7; 6 kapalı, 1 açık |

Bakımcı: [Jens Chr Brynildsen](https://github.com/jenschr) (Oslo, Norveç; Maketronics as). Github etkinliği 31 Ekim 2026'da [SpenceKonde/megaTinyCore](https://github.com/SpenceKonde/megaTinyCore) ve 14 Ekim 2026'da [ThisIsNotRocketScience/GerberTools](https://github.com/ThisIsNotRocketScience/GerberTools) projesinde yorum yazıyor. Yani **bakımcı şu an aktif**.

### Açık issue: [#7 USB-C Tasarım Tartışması](https://github.com/jenschr/Teensy-4.1-example/issues/7)

**Talep sahibi:** İsimsiz bir kullanıcı, 11 Ekim 2026'da açmış. Bu, görseldeki issue listesinde de görünüyordu.

**Talep:** USB-C uygulamasını seviyor. İki USB-C portunun aynı görevi yapmasını, dolayısıyla değiştirilebilir olmasını istiyor. Mevcut tasarım, birinin USB host ve diğerinin güç/programlama olduğunu varsayıyor. Kullanıcılar portları karıştırabilir çünkü her iki USB-C de aynı görünüyor. Host modu USB-A olsaydı karıştırma daha az olurdu ama USB-A'nın döndürme sorunu var. Modern bilgisayarlarda her USB-C portu aynı görevi yapabiliyor (örneğin her port şarj edebiliyor). Bunu çözmek için kullanıcının niyetini belirleyen ve D+/D- ve gücü doğru yönlendiren bir çip olabilir. Ya da yazılım değişiklikleri gerekebilir. Düşüncelerini öğrenmek istiyor.

**Katılım planı:** Bu, **senin ilgi alanların için mükemmel bir iş**. USB-C, güç ve veri yönlendirme, PMIC/Port Controller (TI TUSB, Cypress, Diodes Incorporated gibi) seçimi. Somut adımlar:
1. [USB-C Port Controller (PD) seçenekleri](https://www.ti.com/interface/usb/type-c-and-power-delivery/overview.html) araştır.
2. Mevcut şemada USB-C portlarını ve CC hatlarını incele.
3. Port rolünü otomatik algılayan bir devre tasarla (örneğin TUSB321 veya FUSB302 gibi).
4. Yeni bir KiCad revizyonu olarak `usb-c-dual-role` dalı aç.
5. `README`'de tasarım kararlarını, test sonuçlarını ve kullanılan çipleri ayrıntılı yaz.

**Zorluk:** Orta-yüksek. Ama **kabul edilme ihtimali yüksek** çünkü bakımcı aktif ve bu açık issue tamamen tartışmaya davet ediyor.

### Kapalı issue'lar: katkı imkânları

#### [#6 Bootloader serial debug ile iletişim](https://github.com/jenschr/Teensy-4.1-example/issues/6)

**Talep sahibi:** [Wierdwizard2587](https://github.com/Wierdwizard2587). Bu tasarıma dayanarak kendi özel Teensy 4.1 PCB'sini yapıyor. Windows Device Manager'da hata kodu 43 ve descriptor hatası alıyor. Bootloader için JTAG breakout pinlerinden ne beklemesi gerektiğini soruyor. TDO ve TDI'nin bootloader USB iletişiminden sorumlu olduğunu düşünüyor ama emin değil. PCB yerleşimini henüz dışlayamıyor; kartın seri iletişim için işleyişine dair şüphe uyandıracak bir neden görmüyor. Kendi tasarımında SD kart yuvası, Ethernet ve harici RAM ekledi.

**Çözüm:** Wierdwizard2587, ESD diyotlarının USB 1.1 için derecelendirildiğini, USB 2.0 olmadığını keşfetmiş. Bunları çıkardığında sorunsuz çalışmış. Jenschr, bu tasarımda kullandığı ESD diyotunun Littelfuse **SP3003-02JTG** olduğunu, sorunların daha az olduğunu ve CE onayı kapsamında yıkıcı ESD testi için bu çiplerle kartlar gönderdiğini söylemiş.

**Katılım planı:** Dokümantasyon katkısı. `README`'de "ESD Protection" bölümü ekle ve bu öğrenilen dersi (USB 2.0 için doğru ESD diyotu seçimi) açıkla. Bu hem senin USB/ESD bilgin artırır hem de diğer kullanıcıların bu hataya düşmesini önler.

**Zorluk:** Düşük. Çok iyi bir dokümantasyon PR'ı.

#### [#5 Kişisel iletişim talebi](https://github.com/jenschr/Teensy-4.1-example/issues/5)

**Talep:** Bir kullanıcı, Teensy 4'ü bir PCB'ye entegre etmek ve kodu kopyalanmaya karşı korumak istiyor. USB bağlantısı için özel Teensy'ye ihtiyaç duyuyor. Proje çok gelişmiş bir elektronik davul ve davulun dışına monte edilecek sağlam bir USB bağlantısı gerekiyor.

**Cevap:** Jenschr iletişim formu ve e-posta adresini paylaşmış; bu işi danışman olarak yaptığını söylemiş.

**Sonuç:** Bu bir iş talebi, teknik katkı değil. Dokümantasyon katkısı olarak düşünülmemeli.

#### [#4 Neden SD dahil değil?](https://github.com/jenschr/Teensy-4.1-example/issues/4)

**Talep:** Bir kullanıcı, SD kartın neden dahil olmadığını soruyor.

**Cevap:** Jenschr, bu kartın doğrudan kullanılmak için değil, başlangıç noktası olarak tasarlandığını söylüyor. Ethernet eklemek zor değil; sadece şemayı izleyip PJRC'nin kullandığı benzer bir ETH fişi eklemek yeterli ([PJRC ETH şema](https://www.pjrc.com/store/j1b1211ccd.pdf)).

**Katılım planı:** SD kart ve Ethernet desteği ekleyen bir KiCad revizyonu. Bu, senin PCB yeteneklerinle doğrudan uyumlu. Somut adımlar:
1. [PJRC Teensy 4.1 resmi şema](https://www.pjrc.com/store/teensy41_schematic.pdf)'sini incele.
2. SD kart (microSD) ve Ethernet (PHY) modüllerini mevcut şemaya ekle.
3. Yeni bir KiCad revizyonu olarak `sd-eth` dalı aç.
4. Yerleşim ve test sonuçlarını `README`'de ayrıntılı yaz.

**Zorluk:** Orta. İyi bir PCB katkısı.

#### [#3 D41](https://github.com/jenschr/Teensy-4.1-example/issues/3)

Issue'nun gövdesi yok. Muhtemelen bir isim karışıklığı. Bu issue hakkında yorum yapılmamış. Dikkate alınmamalı.

#### [#2 Ethernet Desteği Ekle](https://github.com/jenschr/Teensy-4.1-example/issues/2)

**Talep:** Bir kullanıcı (sdrabb), çevre birimleri entegre etmeye başlamak için işbirliği yapmak istiyor ve Ethernet'i öneriyor.

**Cevap:** Jenschr, kartın doğrudan kullanılması amaçlanmadığını ve başlangıç noktası olduğunu söylüyor. Ethernet eklemek zor değil; şemayı izleyip benzer bir ETH fişi eklemek yeterli.

**Katılım planı:** [#4](https://github.com/jenschr/Teensy-4.1-example/issues/4) ile aynı. Ethernet desteği ekleyen bir KiCad revizyonu.

**Zorluk:** Orta.

#### [#1 Şema revizyonu](https://github.com/jenschr/Teensy-4.1-example/issues/1)

**Talep:** [fabianmuehlberger](https://github.com/fabianmuehlberger), Jenschr'nin "çok da ideal olmayan şeması ve RT1062 sembolünü" yeniden çalışıyor. Henüz bitmemiş ama [fabianmuehlberger/Teensy-4.1-example](https://github.com/fabianmuehlberger/Teensy-4.1-example) reposunda mevcut. Merge ile ilgileniyorsa haber vermesini istiyor.

**Cevap:** Jenschr, test edildiğinden ve doğrulandığında eklemekten memnuniyet duyacağını söylemiş. Ama daha sonra şemanın hiç bitmediğini, güç pinlerinin hiç bağlı olmadığını söylemiş.

**Sonuç:** Bu issue kapanmış ve katkı tamamlanmamış. Ama önemli bir katkı fırsatı: **şema revizyonu**. Fabian'ın başlattığı işi tamamlayıp PR açmak güzel bir katkı olurdu. Ama bu, Jenschr'nin şemasını baştan yazmak anlamına gelir; büyük bir iş.

## Listede neler değişmeli?

Bu listeyi kariyer hedeflerin (FPGA/ASIC, gömülü, robotik, PCB) açısından yeniden değerlendir:

| Repo | Durum | Öneri |
|---|---|---|
| [Miceuz/i2c-moisture-sensor](https://github.com/Miceuz/i2c-moisture-sensor) | Bakımcı duraklamış, [MeshCore](https://github.com/Miceuz/MeshCore)'e odaklı | Koru; PCB katkısı olarak kullan |
| [brianbennett/fpga_nes](https://github.com/brianbennett/fpga_nes) | Bakımcı büyük ölçüde terk etmiş | Koru; RTL katkısı olarak kullan |
| [jamieiles/80x86](https://github.com/jamieiles/80x86) | Bakımcı duraklamış, [merv](https://github.com/jamieiles/merv)/[rxv](https://github.com/jamieiles/rxv)'e odaklı | Koru; GateMate katkısı olarak kullan |
| [dubemezeagwu/ble_gatt_flutter](https://github.com/dubemezeagwu/ble_gatt_flutter) | Tek kişilik uygulama; topluluk katkısı beklemiyor | **Çıkar**; ilgi alanlarınla uyumsuz |
| [jenschr/Teensy-4.1-example](https://github.com/jenschr/Teensy-4.1-example) | **Bakımcı aktif**, KiCad ve PCB | **Koru**; en iyi katkı hedefi |

## Önerilen öncelik sırası (güncellenmiş)

1. **[jenschr/Teensy-4.1-example #7 USB-C tasarım tartışması](https://github.com/jenschr/Teensy-4.1-example/issues/7)** — Bakımcı aktif ve issue tamamen tartışmaya davet ediyor. USB-C çip seçimi ve KiCad revizyonu; PCB yeteneklerinle doğrudan uyumlu.
2. **[jenschr/Teensy-4.1-example #4 / #2 SD ve Ethernet desteği](https://github.com/jenschr/Teensy-4.1-example/issues/4)** — Somut PCB katkısı; bakımcı açıklamış ve davet etmiş.
3. **[jamieiles/80x86 #13 GateMate FPGA taşıması](https://github.com/jamieiles/80x86/issues/13)** — FPGA kariyer hedefin için en yüksek öğrenme değeri; uzun vadeli, kendi hesabında port olarak.
4. **[Miceuz/i2c-moisture-sensor Qwiic revizyonu](https://github.com/Miceuz/i2c-moisture-sensor/issues/30)** — Hızlı KiCad katkısı; kabul ihtimali düşük ama öğrenme değeri var.
5. **[brianbennett/fpga_nes APU düzeltmeleri](https://github.com/brianbennett/fpga_nes/issues/31)** — RTL doğrulama pratiği; kabul ihtimali düşük.

## Kaynaklar

- [Repo listesi](https://github.com/stars/kemaldemirag/lists/katk%C4%B1-yapt%C4%B1%C4%9F%C4%B1m-repolar-current/)
- [jenschr/Teensy-4.1-example](https://github.com/jenschr/Teensy-4.1-example) — açık issue [#7](https://github.com/jenschr/Teensy-4.1-example/issues/7); kapalı issue'lar [#6](https://github.com/jenschr/Teensy-4.1-example/issues/6), [#5](https://github.com/jenschr/Teensy-4.1-example/issues/5), [#4](https://github.com/jenschr/Teensy-4.1-example/issues/4), [#3](https://github.com/jenschr/Teensy-4.1-example/issues/3), [#2](https://github.com/jenschr/Teensy-4.1-example/issues/2), [#1](https://github.com/jenschr/Teensy-4.1-example/issues/1)
- [dubemezeagwu/ble_gatt_flutter](https://github.com/dubemezeagwu/ble_gatt_flutter)
- Bakımcılar: [jenschr](https://github.com/jenschr) (aktif; [Maketronics](https://maketronics.no)), [dubemezeagwu](https://github.com/dubemezeagwu), [Miceuz](https://github.com/Miceuz), [brianbennett](https://github.com/brianbennett), [jamieiles](https://github.com/jamieiles)
- USB-C kaynakları: [TI USB-C PD](https://www.ti.com/interface/usb/type-c-and-power-delivery/overview.html)
- PJRC kaynakları: [Teensy 4.1 şema](https://www.pjrc.com/store/teensy41_schematic.pdf), [ETH fişi şema](https://www.pjrc.com/store/j1b1211ccd.pdf)
