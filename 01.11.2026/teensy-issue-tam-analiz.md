# jenschr/Teensy-4.1-example: Tüm Issue Analizi

**Repo:** [jenschr/Teensy-4.1-example](https://github.com/jenschr/Teensy-4.1-example)
**Toplam issue:** 7 (1 açık, 6 kapalı)
**Bakımcı:** [jenschr](https://github.com/jenschr) (Jens Chr Brynildsen, Oslo, Norveç; Maketronics as — **şu an aktif**: Ekim 2026'da [megaTinyCore](https://github.com/SpenceKonde/megaTinyCore) ve [GerberTools](https://github.com/ThisIsNotRocketScience/GerberTools) projelerinde yorum yazıyor)
**Rapor tarihi:** 1 Kasım 2026

Bu rapor, listenin dördüncü sırasındaki Teensy-4.1-example reposundaki **tüm 7 issue'yu** analiz eder. Bu repo, listedeki **tek aktif bakımcılı** repo olduğundan katkı kabul ihtimali en yüksek olanıdır.

## Tüm issue'lar: tam analiz

### 1. [#7 "USB-C Design Discussion"](https://github.com/jenschr/Teensy-4.1-example/issues/7) — AÇIK (Ekim 2026)

**Talep:** Bir kullanıcı, USB-C uygulamasını seviyor ama iki USB-C portunun aynı görevi yapmasını (değiştirilebilir olmasını) istiyor. Mevcut tasarım bir portu USB host, diğerini güç/programlama olarak varsayıyor. İki port da aynı göründüğü için kullanıcılar karıştırabilir. Modern bilgisayarlarda her USB-C portu aynı görevi yapabiliyor. Çözüm olarak, kullanıcının niyetini belirleyip D+/D- ve gücü doğru yönlendiren bir çip olabileceğini düşünüyor; yazılım değişikliği de gerekebilir. Düşüncelerini soruyor.

**Teknik bağlam:** Bu, **USB-C Dual-Role Port (DRP)** tasarımı demek. Çözüm için seçenekler:
- **CC yönlendirme/role-switch çipleri:** TI TUSB321, FUSB302 (onsemi) veya Diodes PI5USB series gibi Type-C port controller'lar; CC pinlerini okuyup host/device rolünü belirler.
- **Analog switch/mux:** D+/D- hattını yönlendirmek için USB2.0 mux (örn. TI TS3USB30).
- **Karttaki MIMXRT1062** zaten USB OTG (ULPI) destekliyor; donanım tarafında doğru mux + CC denetleyicisi eklenirse iki port da otomatik rol seçebilir.

**Katılım planı:**
1. Issue'ya yorum yazıp DRP tasarımını öner (bakımcı tartışmaya açık — issue "Discussion" başlığıyla açılmış).
2. CC denetleyici + USB2 mux seçimini yap, şemada mevcut USB-C bölümünü incele.
3. `usb-c-dual-role` dalında KiCad revizyonu hazırla, PR aç.

**Zorluk:** Orta-yüksek. **Kabul ihtimali:** Yüksek — bakımcı aktif ve issue tam bir tartışma daveti.

### 2. [#6 "Bootloader communicating with serial debug"](https://github.com/jenschr/Teensy-4.1-example/issues/6) — KAPALI (2025)

**Talep:** [Wierdwizard2587](https://github.com/), bu tasarıma dayanarak kendi özel Teensy 4.1 PCB'sini yapmış (SD kart, Ethernet, harici RAM eklemiş). Windows Device Manager'da hata kodu 43 ve descriptor hatası alıyor. JTAG breakout pinlerinden bootloader için ne beklemesi gerektiğini soruyor; TDO/TDI'nin USB iletişiminden sorumlu olduğunu düşünüyor.

**Çözüm (kullanıcı buldu, Mayıs 2025):** **ESD diyotları USB 1.1 için derecelendirilmiş**, USB 2.0 değil. Çıkarınca kart sorunsuz çalışmış. Bakımcı da benzer sorunlar yaşadığını doğrulamış ve şu an kullandığı ESD diyotunun **Littelfuse SP3003-02JTG** olduğunu, sorunların daha az olduğunu söylemiş. CE onayı kapsamında yıkıcı ESD testi için bu çiplerle kartlar gönderdiğini eklemiş.

**Katılım planı:** Somut dokümantasyon fırsatı — `README`'ye "ESD Protection" bölümü ekle:
- USB 2.0 hatlarında ESD diyotu seçiminde USB 1.1/2.0 derecelendirme farkı.
- SP3003-02JTG önerisi ve deneyim.
- Bu, bakımcının CE/ESD test deneyimiyle birleşince çok değerli bir öğrenme notu.

**Zorluk:** Düşük. İyi bir ilk PR.

### 3. [#5 "Not a bug, I want to contact you for a custom PCB with a teensy 4.0."](https://github.com/jenschr/Teensy-4.1-example/issues/5) — KAPALI

**Talep:** Bir kullanıcı, davul projesi için Teensy 4'ü bir PCB'ye entegre etmek ve kodu kopyalamaya karşı korumak istiyor. Sağlam bir dış USB bağlantısı gerekiyor.

**Cevap:** Bakımcı iletişim formunu ([Maketronics](https://maketronics.no/support/)) ve e-postasını paylaşmış; bu işi danışman olarak yaptığını söylemiş.

**Değerlendirme:** İş talebi — teknik katkı fırsatı değil. Ama önemli bir sinyal: **bu repo'nun kullanıcıları gerçek üretim işleri için bakımcıya ulaşıyor**; yani bu tasarımın topluluğu aktif.

### 4. [#4 "Any reason why SD not included?"](https://github.com/jenschr/Teensy-4.1-example/issues/4) — KAPALI

**Talep:** Kullanıcı, SD kartın neden dahil olmadığını soruyor.

**Cevap:** Bakımcı, bu kartın doğrudan kullanılmak için değil **başlangıç noktası** olarak tasarlandığını söylüyor. Eklemek kolay: PJRC'nin şemasını izleyip benzer bir ETH fişi eklemek yeterli ([PJRC J1B1211CCD fişi](https://www.pjrc.com/store/j1b1211ccd.pdf)).

**Katılım planı:** Somut PCB işi — microSD yuvası ekle:
1. [PJRC Teensy 4.1 şemasını](https://www.pjrc.com/store/teensy41_schematic.pdf) incele (SD kart bağlantıları).
2. `sd-support` dalında KiCad revizyonu: microSD yuvası + gerekçeli bölüm.
3. Bakımcı "kolay" dediğine göre kabul ihtimali yüksek.

**Zorluk:** Orta.

### 5. [#3 "D41"](https://github.com/jenschr/Teensy-4.1-example/issues/3) — KAPALI (2023)

**Talep:** Kullanıcı, README'de "D41 was not brought out" (D41 dışarı çıkarılmadı) ifadesini görüyor; D41'in ne olduğunu bulamıyor.

**Cevap:** Bakımcı, D41'in [resmi Teensy pinout](https://www.pjrc.com/teensy/pinout.html)'ta yalnızca "41" olarak işaretlenen pin olduğunu açıklamış. Kullanıcı neden hariç bıraktığını sorunca cevap: **"Yer yok"** — espriyle.

**Değerlendirme:** Küçük bir dokümantasyon açıklaması — çözülmüş. Katkı fırsatı: README'de "Known Bugs / Limitations" bölümünde hangi pinlerin neden hariç tutulduğunu tablo halinde netleştirmek.

### 6. [#2 "Add Ethernet Support"](https://github.com/jenschr/Teensy-4.1-example/issues/2) — KAPALI

**Talep:** Kullanıcı, çevre birimleri entegre etmek için işbirliği istiyor ve Ethernet'i öneriyor.

**Cevap:** Bakımcı, kartın başlangıç noktası olduğunu söylüyor; Ethernet eklemek zor değil — şemayı izleyip benzer bir ETH fişi eklemek yeterli ([PJRC J1B1211CCD](https://www.pjrc.com/store/j1b1211ccd.pdf)).

**Katılım planı:** [#4](https://github.com/jenschr/Teensy-4.1-example/issues/4) ile aynı: **Ethernet desteği ekle**. MIMXRT1062'de Ethernet MAC hazır; PHY (örn. KSZ8081 veya LAN8720) + RJ45 jack (magnetik dahil) eklenecek. Bakımcı iki kez "kolay, davet ediyorum" dedi — **en açık davet**.

**Zorluk:** Orta. PHY seçimi, saat (25 MHz crystal veya REF_CLK), RMII arayüzü ve magnetics dahil RJ45 seçimi var.

### 7. [#1 "Schematic rework"](https://github.com/jenschr/Teensy-4.1-example/issues/1) — KAPALI (2022-2023)

**Talep:** [fabianmuehlberger](https://github.com/fabianmuehlberger), bakımcının "çok da ideal olmayan" şemasını ve RT1062 sembolünü yeniden çalışmış; [kendi fork'unda](https://github.com/fabianmuehlberger/Teensy-4.1-example) paylaşıp merge teklif etmiş.

**Süreç:** Bakımcı olumlu karşıladı ("Test ettin mi?"). Fabian USB ve güç bölümlerünü bitirdiğini, **hiyerarşik sayfalara** böldüğünü ve yeni birkaç ağ çizdiğini söyledi. Ama iş yarım kaldı. Şubat 2023'te bakımcı son bir kez baktı: **çipe giden güç pinleri hiç bağlanmamış**. Issue bu şekilde kapandı.

**Katılım planı:** En büyük ve en değerli iş — **Fabian'ın yarıda kalan hiyerarşik şema revizyonunu tamamla**:
1. Fabian'ın fork'unu klonla, hiyerarşik yapıyı anla.
2. MIMXRT1062'nin güç pinlerini (çoklu gerilim alanları, güç sıralaması) doğru bağla — [NXP i.MX RT1060 HDUG](https://www.nxp.com/docs/en/user-guide/IMXRT1060HDUG.pdf) referansı.
3. Footprint bağlantı sorununu düzelt.
4. ERC/DRC çalıştır; mümkünse gerçek kart üret (bakımcının kapanış gerekçesi buydu).
5. `schematic-rework-v2` dalıyla PR aç.

**Zorluk:** Yüksek. **Öğrenme değeri:** Çok yüksek (NXP i.MX güç tasarımı, USB 2.0 ULPI, KiCad hiyerarşik şema).

## Katkı öncelik sırası

1. **[#7 USB-C DRP tasarımı](https://github.com/jenschr/Teensy-4.1-example/issues/7)** — Açık issue, aktif bakımcı, tam tartışma daveti. Önce yorumla başla, sonra KiCad revizyonu.
2. **[#2 Ethernet desteği](https://github.com/jenschr/Teensy-4.1-example/issues/2)** — Bakımcı iki kez "kolay, davet ediyorum" dedi. PHY + RJ45 ekle.
3. **[#4 SD kart desteği](https://github.com/jenschr/Teensy-4.1-example/issues/4)** — Aynı davet; microSD ekle. [#2] ile birlikte tek PR zincirinde düşünülebilir.
4. **[#6 ESD koruma dokümantasyonu](https://github.com/jenschr/Teensy-4.1-example/issues/6)** — Hızlı, düşük riskli dokümantasyon PR'ı.
5. **[#1 Şema revizyonu](https://github.com/jenschr/Teensy-4.1-example/issues/1)** — En büyük iş; uzun vadeli olarak ele al.

## Kariyer uyumu

Bu repo, senin PCB/KiCad uzmanlık alanınla **birebir örtüşüyor**: NXP i.MX RT1062 (600 MHz Cortex-M7), USB-C/USB 2.0, Ethernet PHY, SD kart, ESD koruma ve hiyerarşik şema tasarımı. Bakımcının aktif ve katkıya açık olması, listenin diğer repolarına göre bu repoyu **PR kabul edilme ihtimali en yüksek** hedef yapıyor.

## Kaynaklar

- [jenschr/Teensy-4.1-example](https://github.com/jenschr/Teensy-4.1-example) — repo
- Issue'lar: [#7](https://github.com/jenschr/Teensy-4.1-example/issues/7), [#6](https://github.com/jenschr/Teensy-4.1-example/issues/6), [#5](https://github.com/jenschr/Teensy-4.1-example/issues/5), [#4](https://github.com/jenschr/Teensy-4.1-example/issues/4), [#3](https://github.com/jenschr/Teensy-4.1-example/issues/3), [#2](https://github.com/jenschr/Teensy-4.1-example/issues/2), [#1](https://github.com/jenschr/Teensy-4.1-example/issues/1)
- [fabianmuehlberger/Teensy-4.1-example](https://github.com/fabianmuehlberger/Teensy-4.1-example) — yarıda kalan şema revizyonu
- [PJRC Teensy pinout](https://www.pjrc.com/teensy/pinout.html), [PJRC Teensy 4.1 şeması](https://www.pjrc.com/store/teensy41_schematic.pdf), [PJRC ETH fişi J1B1211CCD](https://www.pjrc.com/store/j1b1211ccd.pdf)
- [NXP i.MX RT1060 HDUG](https://www.nxp.com/docs/en/user-guide/IMXRT1060HDUG.pdf)
- Bakımcı: [jenschr](https://github.com/jenschr) ([Maketronics](https://maketronics.no))
