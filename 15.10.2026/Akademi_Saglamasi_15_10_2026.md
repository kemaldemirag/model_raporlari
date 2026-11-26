|  |
| --- |

# Akademi Sağlaması

Power & Embedded Interface Portfolio<br>15.10.2026 tarih kökü

**Academia.edu kayıtlı içerik bağlamı, ResearchGate yayın kayıtları, kurumsal tez arşivleri ve resmi üretici belgeleriyle çapraz teknik doğrulama**

| **Rapor tarihi** | 18 Ekim 2026 |
| --- | --- |
| **Değerlendirilen durum** | 15 Ekim 2026 anlık görüntüsü / hedef mimari |
| **Ana karar** | Birebir tek yayın yok; güçlü, tamamlayıcı kanıt kümesi var |
| **Kanıt sınırı** | Akademik benzerlik, tamamlanmış donanım veya otomotiv uygunluğu değildir |

*Hazırlanan çıktı: bağımsız, düzenlenebilir akademik-teknik değerlendirme raporu*

## 1. Yönetici özeti

| **GENEL HÜKÜM**<br>**MODÜLER OLARAK GÜÇLÜ** | **BİREBİR AKADEMİK EŞ**<br>**BULUNAMADI** | **ÜRÜN / UYGUNLUK KANITI**<br>**HENÜZ YOK** |
| --- | --- | --- |

| **HÜKÜM** | **Kısa hüküm**<br>İncelenen akademik ve teknik kaynaklarda hedef mimarinin tamamını — ESP32-S3, korumalı 24 V giriş, 24→5→3,3 V raylar, USB-A host/VBUS, çoklu TTL ve RS-232, dört katmanlı PCB, CAN/TWAI, LIN ve GNSS — tek kartta ve deneysel olarak doğrulayan birebir bir çalışma bulunmadı. Buna karşın hedefin ana alt sistemleri güçlü bir tamamlayıcı kaynak kümesiyle destekleniyor. |
| --- | --- |

En yakın akademik üçlü şudur: 24 V endüstriyel ESP32-S3 kart kolu için Kaufman (VUT, 2025) \[A2\]; otomotiv CAN-FD/GNSS ve dört katmanlı küçük kart kolu için Klášterka (CTU, 2026) \[A1\]; ESP32-S3 TWAI ve tekrarlanabilir gerçek-donanım test yöntemi için hakemli CAN-DAQ (HardwareX, 2026) \[A3\]. Bu üç kaynak birbirinin yerine geçmez; birlikte kullanıldığında hedefin güç, otomotiv haberleşme ve deneysel doğrulama boyutlarını tamamlar.

Academia.edu girişli Bookmarks listesi 34 sayfada 840 benzersiz kaydı içeriyordu; anahtar sözcük tabanlı ilk elemede 124 kayıt hedefle potansiyel ilişkiliydi. En değerli kayıt, ResearchGate’te de açık yayın kaydı bulunan “Design of Electrical Test Device for Automotive Industry” oldu \[A6\]. Bu çalışma hedef MCU ve güç mimarisini karşılamasa da otomotiv test cihazında birden fazla RS-232 yolu ile USB Host’un birlikte kullanılmasına somut emsal sağlıyor.

Araştırmanın üç kritik sonucu tasarım kapısı niteliğindedir: (i) ESP32-S3 USB-OTG ve USB Serial/JTAG aynı dahili PHY’yi paylaşır; host ve debug yolu birlikte tasarlanmalıdır \[T1\]; (ii) ESP32-S3 yalnız üç donanımsal UART sunar, dolayısıyla “5 TTL + 5 RS-232” talebi akış anlamı açıklığa kavuşmadan pinlenemez \[T3\]; (iii) 24 V etiketi otomotiv transient uygunluğu anlamına gelmez — araç sınıfı, darbe profili ve fiziksel test planı ayrıca tanımlanmalıdır.

| **SINIR** | **15.10.2026 durumuyla uyum**<br>Akademi sağlaması, bitmiş bir tasarımı onaylamıyor. Yetkili anlık görüntüde G0 PASS, G1 BLOCKED, G2–G6 NOT\_STARTED durumundadır; şematik, PCB, BOM ve üretim paketi henüz doğrulanmış teslim olarak mevcut değildir. Bu nedenle raporun sonucu “tasarım kanıtlandı” değil, “hedef mimari için literatür dayanağı ve karar boşlukları belirlendi” şeklindedir. |
| --- | --- |

## 2. Kapsam, tarih ve kanıt zinciri

| **Alan** | **Değer** |
| --- | --- |
| İncelenen tarih kökü | 15.10.2026 |
| Araştırma / erişim tarihi | 18.10.2026, Europe/İstanbul |
| Yetkili yerel anlık görüntü | 004-v2-pre-flattening-workspace.zip |
| Anlık görüntü SHA-256 | AF60D72B67671DE64FCFA99520F01B01C985C60D05C994E36AFD9226B9311C36 |
| Platform bağlamı | Academia.edu girişli Bookmarks + ResearchGate açık yayın kayıtları |
| Değerlendirme türü | Hedef mimariye teknik benzerlik ve kanıt boşluğu analizi |

### 2.1 Değerlendirilen yerel kapsam

İnceleme, klasör adından bağımsız olarak değişebilen güncel dosyaları değil, 15.10.2026 saat 23:52:20’de oluşturulmuş yetkili ZIP anlık görüntüsünü durum referansı kabul eder \[L4\]. SHA-256 değeri rapor hazırlanırken yeniden hesaplanmış ve yukarıdaki değerle eşleşmiştir. Tarih kökündeki daha sonraki değişiklikler, 15 Ekim durumuna geriye dönük olarak mal edilmemiştir.

| **Repo / çalışma kolu** | **15.10.2026 amaç** | **Yetkili durum** |
| --- | --- | --- |
| REP-01 — esp32-s3-24v-interface-board | Korumalı 24 V giriş, 5 V/3,3 V güç, ESP32-S3, USB host, seri arayüzler, dört katmanlı KiCad aday tasarımı | SCAFFOLD\_ONLY; G1 kaynak doğrulamasını bekliyor |
| REP-02 — power-rail-validation-and-review | Ray bütçesi, derating, sıralama, reset/PGOOD, hata enjeksiyonu ve kanıt sınıflı inceleme yöntemi | Yöntem / sözleşme kolu; fiziksel donanım doğrulaması değil |
| REP-03 — esp32-s3-automotive-interfaces | CAN/TWAI, LIN, GNSS ve araç giriş koşulları için ayrı varyant | SCAFFOLD\_ONLY; exact-part ve transient profili açık |

### 2.2 Kanıt katmanları

| **1 · Yerel hedef** | **2 · Kayıtlı bağlam** | **3 · Akademik doğrulama** | **4 · Mühendislik kapısı** |
| --- | --- | --- | --- |
| Anlık görüntü, handoff, gereksinim matrisi | Academia Bookmarks; ResearchGate yayın mevcudiyeti | Hakemli makale, kurumsal tez, açık donanım | Resmi üretici kısıtı, test ve kabul ölçütü |

Kanıt önceliği; DOI/yayınevi veya kurumsal arşiv kaydı → tam metin/kurumsal tez → açık veri ve kaynak dosyaları → platform kopyası/saved-item bağlamı sırasındadır. Academia.edu ve ResearchGate, keşif ve çapraz mevcudiyet için değerlidir; teknik iddialar mümkün olduğunca özgün DOI, kurumsal kayıt veya üretici belgesine bağlanmıştır.

## 3. Platform oturumlarıyla çalışma ve arama yöntemi

### 3.1 Academia.edu — girişli kayıtlı içerik

Kullanıcının açık Academia.edu oturumu üzerinden Bookmarks listesi sayfa sayfa tarandı. 34 sayfada 840 benzersiz kayıt bulundu; geniş anahtar sözcük filtresi (ESP32, embedded, automotive, CAN, LIN, USB, UART, RS-232, PCB, power, buck, regulator, validation vb.) 124 aday üretti. Bu sayı kalite puanı değildir; yalnızca ikinci aşama insan-okur elemesi için kapsayıcı bir aday kümesidir.

| **Kayıtlı içerik** | **Bağlam değeri** | **Sınır** |
| --- | --- | --- |
| Design of Electrical Test Device for Automotive Industry | Otomotiv test cihazında çoklu RS-232, RS-232/USB ve USB Host; ResearchGate kaydı ve DOI ile çaprazlandı \[A6\] | STM32F407; ESP32-S3/24 V hedefi değil |
| Simple and Robust Multipoint Data Acquisition Bus Built on Top of the Standard RS232 Interface | RS-232 üzerinde çok noktalı veri toplama ve hata toparlama yaklaşımı \[A14\] | Hedefteki bağımsız kanal bankasını doğrudan çözmez |
| Advanced stackup planning with impedance, delay and loss validation | Dört katmanda stack-up verisini üretici bilgisiyle doğrulama gereğini destekler \[A15\] | Hedef kart için stack-up veya DRC kanıtı değildir |
| AN 702: Interfacing a USB PHY … USB 2.0 OTG Controller | Harici PHY ve host denetleyici ayrımına kavramsal bağlam | ESP32-S3’e özgü değildir; nihai karar Espressif belgesine dayanır |
| Modeling and simulation of automotive interleaved buck converter | Otomotiv DC/DC dinamiği ve modelleme bağlamı \[A16\] | Giriş koruması veya ürün uygunluğu kanıtı değildir |

### 3.2 ResearchGate — yayın kayıtları ve erişim sınırı

Bağlanan Edge oturumundaki özel ResearchGate Saved List bu çalışma ortamında makine-okur otomasyon yüzeyi olarak açılamadı. Bu nedenle rapor, ResearchGate’te “kayıtlıdır” iddiasında bulunmaz. Bunun yerine açık ResearchGate yayın kayıtları DOI, dergi, yazar ve özet doğrulaması için kullanıldı; mümkün olduğunda ana atıf özgün DOI veya açık tam metin kaynağına verildi.

- Vardar vd. (2021) kaydı, Academia Bookmarks’ta bulunan aynı başlık ve DOI ile eşleşti; bu, iki platform arasında güçlü çapraz mevcudiyet sağlar \[A6\].

- Low-Cost Data Acquisition System for Automotive Electronic Control Units (Sensors, 2023) açık DOI ve tam metinle doğrulandı; CAN/CCP/XCP, araç bataryası regülatörleri ve gerçek araç test yöntemi için tamamlayıcıdır \[A7\].

- ESP32-S3 + GNSS kullanan V2I çalışması yalnız yardımcı örnek olarak tutuldu; temel kaynak sıralamasına alınmadı.

- Metadata/iç tutarlılık sorunu bulunan bir TCU kaydı (ResearchGate tarihi ile PDF cilt/tarih/yazar bilgileri çelişkili) ana kanıt kümesinden çıkarıldı.

### 3.3 Arama sınırı ve yorum ilkesi

Tarama; girişli Academia kütüphanesi, erişilebilen ResearchGate kayıtları, kurumsal tez arşivleri, PubMed Central/HardwareX, açık donanım deposu ve resmi Espressif belgelerini kapsar. Sonuç sistematik literatür taraması, patent yenilik araştırması veya evrensel yokluk kanıtı değildir. Doğru ifade “incelenen kaynaklarda birebir eşleşme bulunmadı”dır.

## 4. En yakın akademik çalışmalar ve benzerlik sıralaması

Aşağıdaki puanlar bilimsel etki, dergi kalitesi veya bibliyometrik değer değildir. Yalnız hedef mimariye teknik benzerliği ve tasarım kararına sağlayacağı kanıt değerini karşılaştıran karar-destek puanıdır. Bilgi yoksa puan verilmemiş; kısmi benzerlik tam eşleşme sayılmamıştır.

| **Boyut** | **Ağırlık** | **Boyut** | **Ağırlık** |
| --- | --- | --- | --- |
| ESP32-S3 | 15 | USB host/VBUS | 10 |
| Özel PCB / 4 katman | 10 | Çoklu TTL/RS-232 | 10 |
| 24 V güç / 5 V / 3,3 V / koruma | 15 | CAN/TWAI | 10 |
| LIN | 7 | GNSS | 7 |
| Deneysel doğrulama | 10 | Açık/yeniden üretilebilir çıktılar | 6 |

| **Çalışma** | **Puan /100** | **En güçlü örtüşme** | **Ana boşluk** |
| --- | --- | --- | --- |
| Klášterka, CTU, 2026 \[A1\] | 66 | ESP32-S3, 4 katman, CAN-FD, GNSS, gerçek araç testi | 12 V; USB host ve seri kanal bankası yok; LIN uygulanmamış |
| Kaufman, VUT, 2025 \[A2\] | 62 | ESP32-S3; 24→5→3,3 V; koruma; CAN; deneysel test | Endüstriyel; USB host, GNSS, LIN ve hedef seri bankası yok |
| Liebler, AALE, 2025 \[A4\] | 54 | ESP32-S3; 12–24 V; özel PCB; CAN/RS-485 | USB host, otomotiv transient ve 4 katman kanıtı sınırlı |
| Kluka, VUT, 2025 \[A5\] | 50 | ESP32-S3-WROOM-1; 4 katman; MAX3232; çoklu UART tüketicileri | 24 V/USB host/CAN/LIN/GNSS yok; yalnız bir fiziksel RS-232 |
| CAN-DAQ, HardwareX, 2026 \[A3\] | 48 | ESP32-S3 TWAI; açık KiCad/firmware; gerçek donanım testleri | 5 V USB besleme; güç, USB host ve diğer arayüzler kapsam dışı |
| Vardar vd., 2021 \[A6\] | 35 | 3×RS-232, RS-232/USB, USB Host ve otomotiv test topolojisi | STM32F407; hedef güç/CAN/GNSS mimarisi değil |

| **OKUMA** | **Sıralamanın yorumu**<br>Tek bir “kazanan” seçilmemelidir. Klášterka + Kaufman + CAN-DAQ çekirdek kanıt üçlüsüdür. Kluka ve Vardar seri haberleşme/USB alt problemlerini; Kupka, Huvar ve Pokorný ise LIN ile otomotiv giriş/test boşluklarını tamamlar. |
| --- | --- |

## 5. Çekirdek kaynakların ayrıntılı değerlendirmesi

### 5.1 Klášterka (CTU, 2026) — otomotiv kolunun en yakın bütünsel benzeri

Kurumsal kayıtta 22.05.2026’da verilmiş ve 11.06.2026’da savunulmuş lisans tezidir \[A1\]. Tasarım; ESP32-S3-MINI-1-N8, MCP2518FD CAN-FD denetleyicisi, ATA6563 fiziksel katmanı, SIMCom A7670E LTE+GNSS, 12,2–14,7 V araç girişi, TPS54331 12→5 V, R1171J 5→3,3 V ve LM66200 güç seçimini 50×45 mm dört katmanlı bir PCB’de birleştirir.

- Güçlü kanıt: gerçek Škoda Kodiaq üzerinde 500 kbit/s CAN testi (13 dakika), 90 saniyelik LTE uçtan uca deneme ve Nordic PPK2 ile 100 kHz güç ölçümü.

- Hedefe katkı: ESP32-S3 + otomotiv CAN-FD + GNSS + kompakt dört katman entegrasyonunun uygulanabilirliğini gösterir.

- Sınır: 24 V değil 12 V sınıfı; USB-C yalnız güç/sink yoludur, USB-A host/VBUS değildir; çoklu TTL/RS-232 yoktur; LIN teorik olarak anılır fakat uygulanmaz.

- Kanıt niteliği: kurumsal tez ve gerçek araç testi; hakemli dergi makalesi değildir.

### 5.2 Kaufman (VUT, 2025) — 24 V ESP32-S3 kart kolunun en yakın akademik eşi

VUT Brno yüksek lisans tezi, 10.06.2025’te savunulmuş ve A notu almıştır \[A2\]. Özel kart; ESP32-S3, LMR33630 ile 24→5 V, SY8089 ile 5→3,3 V, sigorta/P-MOS/zener koruması, TWAI + SIT1050T, RS-485 ve W5500 gibi endüstriyel arayüzleri bir araya getirir.

- Güçlü kanıt: hedefin 24 V → 5 V → 3,3 V zinciri ve ESP32-S3 tabanlı özel kart fikrine en doğrudan akademik karşılıktır.

- Hedefe katkı: besleme topolojisi, koruma katmanı, CAN/TWAI ve deneysel test yaklaşımı.

- Sınır: otomotiv yerine endüstriyel kullanım; RS-485 hedefteki TTL/RS-232 bankası değildir; USB-A host, LIN ve GNSS yoktur. Genel koruma elemanları otomotiv darbe uygunluğu kanıtlamaz.

- Dört katman benzerliği puanı ihtiyatlı tutulmuştur; mevcut kurumsal özetten katman yapısı hedef düzeyinde doğrulanmamıştır.

### 5.3 CAN-DAQ (HardwareX, 2026) — en güçlü hakemli ve yeniden üretilebilir emsal

Verma vd. tarafından HardwareX 25 (2026) içinde yayımlanan, DOI 10.1016/j.ohx.2026.e00743 olan hakemli açık donanım çalışmasıdır \[A3\]. ESP32-S3 DevKitC-1, MCP2561 transceiver, KiCad taşıyıcı PCB, firmware, masaüstü GUI ve üretim çıktıları açıkça paylaşılmıştır.

- Güçlü kanıt: PCAN-USB ile standart CAN hızlarında test; 1 kHz / 1000 frame/s düzeyinde raporlanan kayıpsız yakalama; gerçek Orion BMS2 (250 kbit/s) ve motor kartı denemeleri.

- Hedefe katkı: ESP32-S3 TWAI’nin harici fiziksel CAN transceiver ile ayrılması; açık BOM/Gerber/PnP/STEP/firmware ve ölçülebilir test zinciri.

- Sınır: USB’den 5 V ile beslenen dar kapsamlı CAN-DAQ’tır; 24 V güç, USB host/VBUS, TTL/RS-232, LIN, GNSS ve hata enjeksiyonlu güç doğrulaması yoktur.

- Düşük toplam benzerlik puanı bilimsel niteliği değil, hedefin geniş kapsamı dışında kalan modülleri gösterir.

### 5.4 Vardar vd. (2021) — Academia/ResearchGate çapraz sağlaması

International Scientific and Vocational Studies Journal 5(2), 154–163’te yayımlanan çalışma, DOI 10.47897/bilmes.1012005’tir \[A6\]. Academia.edu kayıtlı listesinde bulunmuş; ResearchGate açık yayın kaydıyla başlık, dergi, yıl ve DOI bakımından çaprazlanmıştır.

- Güçlü kanıt: otomotiv kablo ve kablo demeti test cihazında STM32F407 tabanlı ana kart; üç RS-232, bir RS-232/USB ve bir USB Host bağlantısı; tamamlanmış işlevsel testler.

- Hedefe katkı: çoklu seri arayüzler ile USB Host’un aynı otomotiv test mimarisinde birlikte yönetilebileceğini gösteren alt-sistem emsali.

- Sınır: ESP32-S3, 24 V güç mimarisi, CAN/LIN/GNSS veya hedef boyut/katman yapısı için doğrudan kanıt değildir.

## 6. Gereksinim → kanıt → boşluk → sonraki doğrulama

Matristeki “güçlü/kısmi/açık” ifadeleri, kaynakların hedef gereksinimi ne ölçüde desteklediğini anlatır; hedef kartın test sonucu değildir. Kabul kararı ancak proje artefaktı ve ölçüm kanıtıyla verilebilir.

| **Gereksinim / karar** | **Kanıt** | **Boşluk** | **Sonraki doğrulama** |
| --- | --- | --- | --- |
| ESP32-S3 özel PCB | Kaufman, Klášterka, Kluka, CAN-DAQ \[A1–A5\] | Güçlü platform desteği; bütünleşik tasarım yok | Modül varyantı, strapping ve pin bütçesini G1’de dondur |
| 24→5→3,3 V güç | Kaufman 24 V; Liebler 12–24 V; Klášterka 12 V otomotiv \[A1,A2,A4\] | Endüstriyel 24 V güçlü; otomotiv 24 V kısmi | USB yükü dâhil worst-case bütçe; min/nom/max giriş, verim, termal ve load-step testi |
| 24 V otomotiv transient | Huvar otomotiv güç; Pokorný ISO 7637-2 test üretimi \[A9,A10\] | Kritik açık: sigorta/TVS/MOSFET tek başına uygunluk değildir | Araç sınıfı + ISO 7637-2 / ISO 16750-2 zarfı; cold-crank, load-dump, ters akü ve ham dalga kayıtları |
| USB-A host + VBUS | Espressif host ve OTG geliştirme kiti \[T1,T2\] | Uygulanabilir; hedef devre ve test yok | EN/OC özellikli akım sınırlı high-side switch, ESD, kısa devre/inrush/500 mA testleri |
| Host + debug PHY | ESP32-S3 OTG ile USB Serial/JTAG aynı iç PHY’yi paylaşır \[T1\] | Kesin mimari kısıt | Dahili host PHY + ayrı USB-UART/JTAG veya harici host PHY seç; eşzamanlı programlama/log test et |
| 5 TTL TX + 5 RS-232 TX + 1 RX | Kluka MAX3232/çoklu UART; Vardar çoklu RS-232/USB \[A5,A6\]; 3 HW UART \[T3\] | Kritik kapasite ve anlam belirsizliği | MIRRORED\_5 mi INDEPENDENT\_10 mu karar ver; harici çoklu-UART; eşzamanlı trafik/baud/hata testi |
| CAN/TWAI | CAN-DAQ TWAI+MCP2561; Klášterka CAN-FD \[A1,A3\] | Klasik CAN güçlü; CAN-FD ayrımı açık | Klasik CAN ise TWAI+transceiver; FD ise harici denetleyici. Hız, bus-load, bus-off ve hata testleri |
| LIN | Kupka gerçek LIN uygulaması \[A8\] | Kısmi; hedef ESP32-S3 uygulaması yok | LIN transceiver, rol/sürüm; break-sync-checksum, wake/sleep ve hata enjeksiyonu |
| GNSS | Klášterka LTE+GNSS; yardımcı ESP32-S3 V2I kaydı \[A1\] | İşlevsel benzerlik var; RF bütünlüğü açık | Anten türü/besleme/keep-out; TTFF, zayıf sinyal ve güç/USB/CAN trafiği altında EMI testi |
| 4 katman / SI | Klášterka ve Kluka 4 katman; Shlepnev stack-up doğrulaması \[A1,A5,A15\] | Uygulanabilir; hedef stack-up yok | Üretici stack-up; referans düzlemi, USB diferansiyel çift, GNSS keep-out ve güç döngüsü incelemesi |
| Tekrarlanabilir doğrulama | CAN-DAQ açık dosya/test; Klášterka gerçek araç ve güç ölçümü \[A1,A3\] | Yöntem güçlü; hedefte fiziksel kanıt yok | Kart revizyonu, konfigürasyon, ham log/iz, başarısız test ve kabul eşiğini birlikte sürümle |

## 7. Tasarım kararlarına etkisi

### 7.1 Kapanmadan G1 PASS verilmemesi gereken kararlar

1. Araç girişi: 24 V’un hangi araç/sistem sınıfını ifade ettiği; minimum–nominal–maksimum sürekli gerilim, jump-start, ters akü, cold-crank ve darbe profili.

2. USB mimarisi: Dahili PHY host için kullanılacaksa programlama/log/debug yolunun ayrı USB-UART/JTAG ile korunması; harici PHY seçeneğinin maliyet/pin/yerleşim etkisi.

3. Seri akış semantiği: 5 TTL ve 5 RS-232 aynı beş verinin iki elektriksel kopyası mı (MIRRORED\_5), yoksa on bağımsız akış mı (INDEPENDENT\_10)? Her iki durumda alıcı sayısı ve hızlar ayrıca tanımlanmalı.

4. CAN kapsamı: Klasik CAN 2.0 mı, CAN-FD mi? ESP32-S3 TWAI yalnız klasik CAN denetleyicisidir; CAN-FD için harici denetleyici gerekir.

5. HMI ve pin bütçesi: LCD arayüz modu/gerilim/arka ışık; iki buton; gelecek TFT DNP pinleri; USB, UART, CAN/LIN ve GNSS ile çakışmayan donmuş GPIO tahsisi.

### 7.2 Mimari öneriler

| **Konu** | **Öneri** | **Dayanak / neden** |
| --- | --- | --- |
| USB host | VBUS’u akım sınırlamalı EN/OC high-side switch üzerinden kaynakla; ESD ve ters akımı ele al | Espressif Type-A 5 V/500 mA referansı ve kısa-devre güvenliği \[T2\] |
| Debug | UART0 veya ayrı USB-UART/JTAG köprüsünü host işlevinden bağımsız tut | Ortak dahili PHY kısıtı \[T1\] |
| Çoklu UART | Dahili üç UART’ı yeterli varsayma; SPI/I²C çoklu-UART genişletici veya açıkça gerekçeli başka mimari | Hedef akış sayısı ve ESP32-S3 çevrebirim kapasitesi \[T3\] |
| RS-232 / TTL | OE, power-off/high-Z, back-power ve açılış glitch davranışını arayüz sözleşmesine yaz | Gerilim alanı ayrımı ve güvenli başlatma; REQ-004 |
| CAN | TWAI denetleyici ile fiziksel transceiver’ı şemada ve dokümantasyonda ayrı tut | CAN-DAQ deneysel emsali \[A3\] |
| LIN | UART sinyaliyle yetinme; otomotiv LIN transceiver, wake/sleep ve rol sözleşmesi | Kupka uygulaması ve fiziksel katman gereği \[A8\] |
| Güç | 24 V etiketini test zarfı yerine kullanma; sürekli ve transient profilleri ayrı kabul et | Otomotiv standardı/test kaynağı \[A9,A10\] |
| PCB | G3 öncesi stack-up/empedans/return-path planını üretici verisiyle dondur | 4 katman tek başına SI/EMI başarısı değildir \[A15\] |

## 8. Akademik konumlandırma ve özgünlük sınırı

| **KATKI** | **Savunulabilir katkı cümlesi**<br>Bu portföyün olası akademik/mesleki katkısı yeni bir temel dönüştürücü veya haberleşme protokolü icat etmekten çok; 24 V güç alanı, USB host/debug paylaşımı, çoklu seri arayüz kapasitesi, CAN/LIN/GNSS varyantı ve kapılı kanıt yönetimini tek bir izlenebilir ESP32-S3 geliştirme yöntemi içinde bütünleştirmektir. |
| --- | --- |

İncelenen literatür, bu alt sistemleri tek tek veya sınırlı kombinasyonlarla kapsar. Birebir eşin bulunmaması otomatik olarak bilimsel yenilik, patentlenebilirlik veya yayın kabulü anlamına gelmez. Özgünlük iddiası ancak açık araştırma sorusu, karşılaştırmalı tasarım alternatifleri, ölçülebilir performans hedefleri ve tekrar üretilebilir deney sonuçlarıyla kurulabilir.

### 8.1 Uygun araştırma soruları

- Üç donanımsal UART sınırlaması altında MIRRORED\_5 ve INDEPENDENT\_10 mimarilerinin maliyet, gecikme, CPU yükü, hata oranı ve pin bütçesi karşılaştırması nedir?

- ESP32-S3 USB host işlevi aktifken bağımsız programlama/log yolunun iki mimarisi (ayrı USB-UART/JTAG ve harici PHY) güvenilirlik ve BOM açısından nasıl karşılaştırılır?

- 24 V araç girişinde tanımlı transient zarfı altında koruma + 24→5→3,3 V zincirinin verim, sıcaklık, yeniden başlatma ve brownout davranışı nasıldır?

- CAN/TWAI, LIN, GNSS, USB host ve çoklu seri trafik aynı anda çalışırken güç bütünlüğü, EMI etkileşimi ve veri kaybı nasıl değişir?

- Kapılı kanıt modeli, başarısız testleri ve belirsiz girdileri PASS’e dönüştürmeden izlenebilir tasarım kararına nasıl bağlar?

### 8.2 Yayın/portföy iddia dili

| **Kaçınılması gereken ifade** | **Savunulabilir ifade** |
| --- | --- |
| Otomotiv sınıfı / ISO uyumlu | Belirtilen giriş zarfına göre tasarlandı; şu testler planlandı/yapıldı; uygunluk iddiası yok |
| Donanım doğrulandı | Şema incelendi / simüle edildi / bench-test edildi — yalnız gerçekten yapılan aşamayı belirt |
| USB host ve debug sorunsuz | Seçilen PHY/debug mimarisi şu eşzamanlı test koşullarında doğrulandı |
| 10 UART kanalı | Akış modeli, genişletici mimarisi, hızlar ve eşzamanlılık ölçütleri açıkça tanımlandı |
| Literatürde yok | Hedefli taramada incelenen kaynaklar içinde birebir eşleşme bulunmadı |

## 9. Önceliklendirilmiş ilerleme planı

| **Öncelik** | **Kapı / çıktı** | **Tamamlanma ölçütü** |
| --- | --- | --- |
| P0 | G1 — giriş profili ve iddia sınırı | Araç/sistem sınıfı, sürekli/transient gerilim zarfı, ortam, USB yükü ve test standardı sürümlü gereksinimlerde tanımlı |
| P0 | G1 — USB ve debug mimarisi | PHY seçimi, VBUS switch/OC davranışı ve eşzamanlı debug yolu karar kaydında; blok diyagram ve pin bütçesi tutarlı |
| P0 | G1 — seri akış modeli | MIRRORED\_5 / INDEPENDENT\_10 kararı; UART genişletici/baud/alıcı sayısı; power-off davranışı ve kabul ölçütü |
| P0 | G2 — CAN/LIN/GNSS varyant sözleşmesi | Klasik CAN vs FD, transceiver’lar, LIN rolü/sürümü, GNSS anten/besleme ve DNP seçenekleri |
| P1 | G2 — ray ve pin bütçesi | Tolerans/tepe akım/termal marjlı güç tablosu; donmuş GPIO ve çakışma analizi |
| P1 | G3 — şema ve pre-layout doğrulama | ERC, kaynak izli exact-part kararları, model sınırlı simülasyon, transient/USB/seri/CAN/LIN/GNSS bench planı |
| P1 | G4 — dört katman PCB | Üretici stack-up, return-path ve SI/PI incelemesi; DRC; severe bulgu yok veya açık NO-GO |
| P2 | G5/G6 — deney ve yayın paketi | Ham log/izler, test konfigürasyonu, kart revizyonu, başarısız testler, BOM/Gerber/PnP/firmware ve yeniden üretim talimatı |

## 10. Sınırlamalar ve dışlanan adaylar

- ResearchGate özel Saved List’in makine-okur erişimi sağlanamadığı için yalnız açık yayın kayıtları doğrulama amacıyla kullanıldı; rapor bu içerikleri kullanıcının özel kaydı olarak etiketlemez.

- Academia.edu anahtar sözcük elemesi kapsayıcıdır; 124 sayısı nitelik veya uygunluk puanı değildir. Seçili kayıtlar tam hedef eşleştirmesi yerine alt-sistem bağlamı olarak yorumlandı.

- Wicaksono vd. (2024) çalışmasındaki “5–24 V” değeri kart besleme girişi değil dört dijital girişe uygulanan sinyal gerilimidir; ayrıca ESP32-S3 değil ESP32/ESP32U kullanır. Bu nedenle 24 V güç mimarisi desteği olarak kullanılmadı.

- UIUC ECE445 Automatic Water Testing final raporu hakemli yayın/tez değildir ve USB OTG’yi yerel seri konsol bağlamında kullanır; USB-A host/VBUS kanıtı sayılmadı.

- Yalnız Scribd kopyasıyla bulunan Cherti (2025) örneği bibliyografik olarak yeterince doğrulanamadığından resmi kaynak sıralamasına alınmadı.

- ResearchGate’te 2026 tarihiyle görünen TCU kaydının PDF cilt/tarih/yazar bilgileri ve içerik kronolojisi çelişkiliydi; temel kanıt kümesinden çıkarıldı.

- Akademik benzerlik; exact-part uygunluğu, üretilebilirlik, EMI/EMC, AEC-Q, ISO uygunluğu veya saha güvenilirliği yerine geçmez.

## 11. Sonuç

| **KARAR** | **Nihai hüküm**<br>Akademi sağlaması POZİTİF fakat KOŞULLUDUR: hedef mimarinin ana parçaları literatürde ayrı ayrı güçlü biçimde destekleniyor; en yakın kaynaklar Kaufman (24 V ESP32-S3 güç kartı), Klášterka (otomotiv CAN-FD/GNSS ve dört katman) ve CAN-DAQ’dır (hakemli TWAI ve açık test kanıtı). Bununla birlikte USB host/debug paylaşımı, çoklu UART kapasitesi ve 24 V otomotiv transient zarfı literatür benzerliğiyle kapanmaz; bunlar G1/G2’de açık mimari karar ve hedef donanım üzerinde deney gerektirir. |
| --- | --- |

Bu sonuç, 15.10.2026 anlık görüntüsündeki G1 BLOCKED durumunu akademik açıdan doğrular: proje doğru problem alanına ve güçlü emsallere dayanıyor, fakat şematik/PCB’ye geçmeden önce müşteri girdileri, çevrebirim kapasitesi ve test zarfı kesinleştirilmelidir. En savunulabilir portföy yaklaşımı, kaynakların ayrı ayrı kanıtladığı modülleri tek bir entegre başarı iddiasına dönüştürmek yerine, her iddiayı ölçülebilir kapı ve ham kanıtla ilişkilendirmektir.

## 12. Kaynakça ve doğrulama bağlantıları

### 12.1 Akademik ve kurumsal kaynaklar

**\[A1\]** Klášterka, J. (2026). Systém analýzy dat ze sběrnice CAN pro monitorování automobilů / CAN Bus Data Analysis System for Automotive Monitoring \[Lisans tezi, Czech Technical University in Prague\]. Kurumsal kayıt ve PDF bütünlüğü doğrulandı; hakemli dergi makalesi değildir.  [Kurumsal kayıt](https://dspace.cvut.cz/entities/publication/f54136e3-1fe0-4bc6-9ef2-69d3f5b5db89)  [Handle](https://hdl.handle.net/10467/180588)

**\[A2\]** Kaufman, O. (2025). Návrh demonstračního řídicího systému / Design of Control System for Product Demonstration \[Yüksek lisans tezi, Brno University of Technology, FEEC\]. Akademik yıl 2024/25; savunma 10.06.2025, derece A.  [VUT kaydı](https://www.vut.cz/studenti/zav-prace/detail/168020)  [Tam metin](https://dspace.vut.cz/server/api/core/bitstreams/545f087b-33d7-4182-bec5-c29a431ae6c3/content)

**\[A3\]** Verma, A.; Dutta, C. M.; Ghosh, A.; Kanchibail, S. M.; Harish, S.; Rishvanth, S. K.; Chandra, S.; Das, S.; Selvakumar, K. (2026). CAN-DAQ: An open-source, cost-effective data capture device and software for automotive research. HardwareX, 25, e00743. DOI: 10.1016/j.ohx.2026.e00743. Hakemli, CC BY 4.0; çevrimiçi 02.02.2026.  [DOI](https://doi.org/10.1016/j.ohx.2026.e00743)  [PMC tam metin](https://pmc.ncbi.nlm.nih.gov/articles/PMC12906070/)  [Açık veri DOI](https://doi.org/10.17632/x6yf8d3tp5.1)  [Kaynak kodu](https://github.com/eccentricOrange/can-daq)

**\[A4\]** Liebler, K. (2025). Lab@Home automatisiert eine Modellfabrik und erhält einen KI-Assistenten. İçinde Tagungsband AALE 2025: Menschzentrierte Automation im digitalen Zeitalter, ss. 232–240. DOI: 10.33968/2025.25. Konferans bildirisi, CC BY 4.0; 2023 öncülü DOI 10.33968/2023.19.  [DOI](https://doi.org/10.33968/2025.25)  [Kurumsal kayıt](https://htwk-leipzig.qucosa.de/en/recherche/landing-page/https%3A%2F%2Fhtwk-leipzig.qucosa.de%2Fen%2Fapi%2Fqucosa%253A95816%2Fmets%2F)  [2023 öncülü](https://doi.org/10.33968/2023.19)

**\[A5\]** Kluka, D. (2025). Design of a Secure Data Transmission System in NB-IoT Environment \[Yüksek lisans tezi, Brno University of Technology, FEEC\]. Savunma 09.06.2025, derece A. ESP32-S3-WROOM-1, dört katmanlı KiCad 9 kartı ve MAX3232 içerir.  [VUT kaydı](https://www.vut.cz/en/students/final-thesis/detail/167301?zp_id=167301)  [Tam metin](https://dspace.vut.cz/server/api/core/bitstreams/93b226b4-f5c1-4eb6-bcaa-5fb46cece4c7/content)

**\[A6\]** Vardar, K.; Aydemir, F.; Özdemir, D.; Topuz, M.; Kolay, B.; Yıldırım, Y.; Bilici, Z.; Baydoğrul, M. U.; Yağan, Y. E.; Kaya, B.; Tandoğan, A.; Gürer, E. Y. (2021). Design of Electrical Test Device for Automotive Industry. International Scientific and Vocational Studies Journal, 5(2), 154–163. DOI: 10.47897/bilmes.1012005.  [DOI](https://doi.org/10.47897/bilmes.1012005)  [Birincil DergiPark kaydı](https://dergipark.org.tr/en/pub/bilmes/article/1012005)  [ResearchGate](https://www.researchgate.net/publication/357013240_Design_of_Electrical_Test_Device_for_Automotive_Industry)  [Academia](https://www.academia.edu/122150782/Design_of_Electrical_Test_Device_for_Automotive_Industry)

**\[A7\]** Bedretchuk, J. P.; Arribas García, S.; Igarashi, T. N.; Canal, R.; Wedderhoff Spengler, A.; Gracioli, G. (2023). Low-Cost Data Acquisition System for Automotive Electronic Control Units. Sensors, 23(4), 2319. DOI: 10.3390/s23042319. Hakemli açık erişim makalesi; ESP32-WROOM-32D yalnız BLE alt modülüdür.  [DOI](https://doi.org/10.3390/s23042319)  [Yayın sayfası](https://www.mdpi.com/1424-8220/23/4/2319)  [ResearchGate](https://www.researchgate.net/publication/368681102_Low-Cost_Data_Acquisition_System_for_Automotive_Electronic_Control_Units)

**\[A8\]** Kupka, J. (2024). ECU pro monitorování energetických toků experimentálního vozidla s elektrickým pohonem \[Lisans tezi, VŠB–Technical University of Ostrava\]. Teensy 4.1 üzerinde CAN/LIN birlikte kullanımına tamamlayıcı kanıt; ESP32-S3 değildir.  [Kurumsal kayıt](https://dspace.vsb.cz/items/e64a4124-5ce2-4764-94fc-377b707c3bb5)  [Handle](http://hdl.handle.net/10084/155083)

**\[A9\]** Huvar, V. (2023). Demonstrační kit s napájecími obvody automobilových elektronických systémů / Demonstration Kit with Power Supply Circuits for Automotive Electronic Systems \[Yüksek lisans tezi, VŠB–Technical University of Ostrava\].  [Tez kaydı](https://theses.cz/id/ydd86l/)  [Handle](http://hdl.handle.net/10084/150207)

**\[A10\]** Pokorný, R. (2017). Absolvování individuální odborné praxe / Individual Professional Practice in the Company \[Lisans tezi, VŠB–Technical University of Ostrava\]. ISO 7637-2 darbelerini simüle eden sistem, yazar tarafından sertifikalı validasyon cihazı değil dahili mühendislik testi olarak sınırlandırılır.  [Tez kaydı](https://theses.cz/id/c5wtdd/)

**\[A14\]** Pavluchenko, A.; Kukla, A.; Lozovoy, S. (2016). Simple and Robust Multipoint Data Acquisition Bus Built on Top of the Standard RS-232 Interface (extended version). Çalışma metni/kitap bölümü bağlamı; hakemli dergi makalesi olarak sınıflandırılmadı.  [Academia kayıtlı kopya](https://www.academia.edu/107479835/Simple_and_Robust_Multipoint_Data_Acquisition_Bus_Built_on_Top_of_the_Standard_RS232_Interface)  [ResearchGate çalışma metni](https://www.researchgate.net/publication/303689925_Simple_and_Robust_Multipoint_Data_Acquisition_Bus_Built_on_Top_of_the_Standard_RS-232_Interface_extended_version)

**\[A15\]** Shlepnev, Y. (2018). Advanced Stackup Planning with Impedance, Delay and Loss Validation. Simberian application note / teknik makale; akademik veya hakemli yayın değildir.  [Üretici PDF](https://www.simberian.com/AppNotes/AdvancedStackUpPlanning.pdf)  [Academia kayıtlı kopya](https://www.academia.edu/64528324/Advanced_stackup_planning_with_impedance_delay_and_loss_validation)

**\[A16\]** Shrud, M. A.; Bousbaine, A.; Ashur, A. S.; Thorn, R.; Benmusa, T. (2009). Modeling and Simulation of Automotive Interleaved Buck Converter. 44th International Universities Power Engineering Conference (UPEC 2009). 42/14 V, 1 kW benzetim çalışması; düşük güçlü hedef kartın doğrudan emsali değildir.  [Proceedings TOC](https://www.proceedings.com/content/007/007426webtoc.pdf)  [Academia kayıtlı kopya](https://www.academia.edu/55339379/Modeling_and_simulation_of_automotive_interleaved_buck_converter)

### 12.2 Resmi teknik kaynaklar

**\[T1\]** Espressif Systems. ESP-USB Programming Guide — USB Host (ESP32-S3). USB-OTG ve USB-Serial-JTAG’ın aynı dahili PHY’yi paylaşması; eşzamanlı kullanım için harici PHY ve VBUS güç anahtarı/akım sınırlaması gereği.  [Resmi belge](https://docs.espressif.com/projects/esp-usb/en/latest/esp32s3/usb_host.html)

**\[T2\]** Espressif Systems. ESP32-S3-USB-OTG Development Board User Guide. Type-A VBUS 5 V, 500 mA ve MIC2005A akım sınırlı switch referansı.  [Resmi belge](https://docs.espressif.com/projects/esp-dev-kits/en/latest/esp32s3/esp32-s3-usb-otg/user_guide.html)

**\[T3\]** Espressif Systems. ESP-IDF Programming Guide — UART (ESP32-S3). Üç donanım UART denetleyicisi.  [Resmi belge](https://docs.espressif.com/projects/esp-idf/en/release-v5.4/esp32s3/api-reference/peripherals/uart.html)

### 12.3 Yerel proje kanıtları

**\[L1\]** Codex\_Handoff\_15\_10\_2026\_v2\_Proje\_Ciktilari.md — beş iş, üç repo, G0–G6 ve iddia sınırları. Yerel yol: 15.10.2026/001-Kayıt Notları ve Dokümantasyon/001-Kayıt Notları/Codex Handoff/004-…

**\[L2\]** gereksinim-matrisi.csv ve requirements-traceability.csv — REQ-001…REQ-015, müşteri/türetilmiş kurallar, açık kararlar ve sonraki güvenli eylemler.

**\[L3\]** 002-Phase0-Phase1-Kapi-Raporu.md — G0 PASS; G1 BLOCKED; G2–G6 NOT\_STARTED; kaynak paketleri ve araç durumu.

**\[L4\]** 004-v2-pre-flattening-workspace.zip — oluşturma/değiştirme: 15.10.2026 23:52:20 +03:00; 76.890 bayt; SHA-256 AF60D72B67671DE64FCFA99520F01B01C985C60D05C994E36AFD9226B9311C36.

## Ek A — Puanlama katsayıları ve güven dili

| **Katsayı** | **Anlam** | **Rapor dili** |
| --- | --- | --- |
| 1,00 | Doğrudan uygulanmış ve doğrulanmış | Güçlü doğrudan emsal |
| 0,67 | Yakın fakat eksik veya farklı bağlam | Güçlü/kısmi destek |
| 0,33 | Çevresel, teorik veya dolaylı | Tamamlayıcı bağlam |
| 0,00 | Kapsam dışı ya da kanıtlanmamış | Kanıt yok / açık boşluk |

Güven dili: “doğrulandı” yalnız kaynak metadata’sı, açıkça raporlanan deney veya yerel dosya hash’i için kullanılır. “Destekler” benzerlik kanıtıdır; “kanıtlar” hedef kart üzerinde sonuç anlamına gelmez. “Uygunluk” terimi yalnız normatif test ve belgelenmiş kapsamla kullanılmalıdır.

## Ek B — Rapor bütünlük kontrolü

| **Kontrol** | **Sonuç** |
| --- | --- |
| 15.10.2026 yerel durum referansı | Yetkili ZIP anlık görüntüsü ve SHA-256 ile sabitlendi |
| Academia.edu oturumu | 34 sayfa / 840 benzersiz bookmark tarandı; 124 geniş aday filtresi |
| ResearchGate | Özel Saved List erişimi yok; açık yayın kayıtları DOI/metadata çapraz kontrolünde kullanıldı |
| Birincil akademik kaynaklar | Kurumsal tez kayıtları, DOI ve açık tam metinlere öncelik verildi |
| Şüpheli/yanıltıcı adaylar | 24 V sinyal/besleme karışıklığı, USB-host olmayan ders projesi ve metadata çelişkili kayıt dışlandı |
| İddia sınırı | Akademik sağlamanın donanım/otomotiv uygunluğu olmadığı rapor boyunca korundu |

*— Rapor sonu —*
