# Miceuz/i2c-moisture-sensor: Tüm Issue Analizi

**Repo:** [Miceuz/i2c-moisture-sensor](https://github.com/Miceuz/i2c-moisture-sensor)
**Toplam issue:** 43 (5 açık, 38 kapalı)
**Bakımcı:** [Miceuz](https://github.com/Miceuz) (Albertas Mickėnas, Technarium, Vilnius; şu an [MeshCore](https://github.com/Miceuz/MeshCore) projesine odaklı, bu repo ~2.5 yıldır güncellenmiyor)
**Rapor tarihi:** 1 Kasım 2026

Bu rapor, listenizin üçüncü sırasındaki i2c-moisture-sensor reposundaki **5 açık issue'nun tam analizini** ve katkı fırsatlarını içerir. Kapalı issue'lardan öğrenme değeri yüksek olanlar da özetlenir.

## Açık issue'lar: tam analiz

### 1. [#43 "Multiple sensors measurements"](https://github.com/Miceuz/i2c-moisture-sensor/issues/43) — Eylül 2024

**Talep:** Bir kullanıcı, birden çok sensörü ESP32'ye bağlayıp çok noktadan ölçüm yapmak istiyor. İki sensörlü kod yazmış (`sensor1(0x20)`, `sensor2(0x21)`) ama ikinci sensör sürekli `Soil Moisture Capacitance: 65535, Temperature: -0.10, Light: 65535` okuyor. Çoklu sensör örneği olup olmadığını soruyor.

**Teknik teşhis:** `65535` (0xFFFF), I2C cihazından **yanıt alınamadığını** gösteren klasik bir işarettir. En olası nedenler:
- İkinci sensörün adresi hiç değiştirilmemiş (her ikisi de varsayılan `0x20`'de kalıyor; adres değiştirme komutu yazılmamış ya da yapılmamış).
- Adres değiştirme prosedürü sırasında iki sensörün de aynı anda aktif olması; adres değiştirme tek tek, güç kesilerek yapılmalı.
- Sensör `sleep()` moduna alındıktan sonra yeniden `begin()`/reset yapılmadan okunmaya çalışılması.

**Katılım planı:** Dokümantasyon + örnek kod katkısı. `README`'ye "Birden çok sensör kullanımı" bölümü ekle: adres değiştirme prosedürü (tek tek güç verme), çakışan adres uyarısı ve çoklu okuma örneği. Bu, senin gömülü/I2C bilginle doğrudan uyumlu.

**Zorluk:** Düşük-orta. Çok iyi bir ilk katkı.

### 2. [#37 "ESP32/ MQTT/ LoRa version?"](https://github.com/Miceuz/i2c-moisture-sensor/issues/37) — Kasım 2021

**Talep:** Bir kullanıcı, ESP32 tabanlı bir versiyon öneriyor: [OpenMQTTGateway](https://docs.openmqttgateway.com/) ile çalıştırılıp internete geri bağlanmak için LoRa mesh (LoRaWAN değil) kullanılabilir.

**Bakımcı cevabı (Miceuz, iki yorum):**
1. Kasım 2021: "Öneri için teşekkürler, bakacağım. Not: sensörlerimle çalışan bir LoRaWAN kartı yayınladım: [Low power LoRa I2C RS485 node (Tindie)](https://www.tindie.com/products/miceuz/low-power-lora-i2c-rs485-node/)."
2. Şubat 2023: "Ayrıca sensörü WiFi'ya ve e-ink ekrana bağlayan ESP32 tabanlı bir kart var: [Miceuz/paper-wifi](https://github.com/Miceuz/paper-wifi)."

**Değerlendirme:** Bakımcı **zaten çözümü kendi projeleriyle vermiş** (paper-wifi ve LoRa node). Bu issue aslında kapatılabilir; "yanıt verildi" durumunda. Katkı fırsatı: `README`'ye "Ek donanım / entegrasyonlar" bölümü ekleyip paper-wifi ve LoRa node'a bağlantı vermek. Çok küçük bir dokümantasyon PR'ı.

**Zorluk:** Düşük.

### 3. [#33 "Shorter insert depth"](https://github.com/Miceuz/i2c-moisture-sensor/issues/30) — Kasım 2020

**Talep:** Bir kullanıcı, yalnızca 4-5 cm derinlikteki toprak için nem ölçümü gereken bir sistem geliştiriyor. Mevcut sensör çok uzun olduğu için **şemayı kullanarak daha kısa bir PCB tasarlamış** (görselleri de eklemiş). Ama ATtiny441 programlamada yeni olduğu için, programlayıcı kaynak koduyla çalışmasını sağlayacak **birisiyle işbirliği** arıyor. Başarılı olursa README hazırlamayı ve PCB yerleşimini paylaşmayı teklif ediyor.

**Değerlendirme:** Bu, **açık bir işbirliği daveti**. Teknik iş kısmen yapılmış (kısa PCB tasarımı mevcut); eksik olan ATtiny441 firmware'ini bu yeni donanıma uyarlamak. Ayrıca kapasitif ölçüm, elektrot geometrisi değiştiği için **yeniden kalibrasyon** gerektirir — bu da sensör kalibrasyonu öğrenmek için harika bir fırsat.

**Katılım planı:** Bu issue'nun altına yorum yazıp işbirliği teklif et. Kısa PCB'yi KiCad'de temizle, firmware'i (TWI, termistör, kapasitans ölçümü) uyarla, kalibrasyon prosedürünü uygula. Çıktı: kendi hesabında `chirp-short-stem` tarzı bir repo + detaylı README.

**Zorluk:** Orta. **Öğrenme değeri:** Çok yüksek (KiCad + ATtiny firmware + kalibrasyon).

### 4. [#31 "Add Qwiic connectors"](https://github.com/Miceuz/i2c-moisture-sensor/issues/31) — Aralık 2019

**Talep:** "See issue #30" — #30'un kopyası/duplicate.

### 5. [#30 "Qwiic connectors"](https://github.com/Miceuz/i2c-moisture-sensor/issues/30) — Aralık 2019

**Talep:** [la7dja](https://github.com/la7dja), kapalı mekan kullanımı için [Qwiic konnektörleri](https://www.sparkfun.com/qwiic) seçenek olarak eklemenin harika olacağını düşünüyor. PCB'de yer olduğundan, **fork'unda zaten yapmış**: [la7dja/i2c-moisture-sensor](https://github.com/la7dja/i2c-moisture-sensor) ([görsel](https://github.com/la7dja/i2c-moisture-sensor/blob/master/i2c-moist-sensor-qwiic.png)). İlgilenilirse PR açabileceğini ama KiCad dosyalarıyla ne kadar iyi çalışacağını bilmediğini söylüyor.

**Bakımcı cevabı (Miceuz):** "Merhaba, iyi fikir! Bana pull request gönder, nasıl gittiğine bakacağım."

**Kritik bulgu:** Bakımcı **açıkça PR davet etti** ama la7dja hiçbir zaman PR açmadı. Issue 7 yıldır açık. Bu, listenin en somut katkı fırsatı: **la7dja'nın fork'undaki hazır Qwiic değişikliğini al, KiCad 8'de temizle, PR aç.**

**Ek bağlam:** Issue'nin yorumlarında değerli bir yan ürün daha var — [olNick](https://github.com/) ekibinin (Nikos Boretos, George Kazakis, Dany Ghosn — Mediterranean Agronomic Institute of Chania) [kalibrasyon transfer fonksiyonu dokümanı](https://github.com/Miceuz/i2c-moisture-sensor/files/4857624/M.M_Soil.Moisture.Sensor.Calibration.docx) (02/2020, 3V3 besleme, kil toprak). Örnek: 550 kapasitans okuması → `MCd = (0.000001007*550²)+(0.000024885*550)-0.062508722` → %25.58 kuru baz nem. Bakımcı bunu toplulukla paylaşmaktan memnuniyet duyacağını söylemiş ama repo'ya eklenmemiş görünüyor.

**Katılım planı:**
1. la7dja'nın fork'undaki Qwiic değişikliğini KiCad 8'de yeniden uygula (eski .sch/.pro dosyaları modern .kicad_sch formatına taşınmalı).
2. PR aç — bakımcı zaten davet etmiş.
3. Aynı PR zincirinde veya ayrı bir dokümantasyon PR'ında kalibrasyon dokümanını repo'ya ekle (yazar atıfıyla).

**Zorluk:** Orta. **Kabul ihtimali:** Orta-yüksek (bakımcı davet etmiş; ama şu an MeshCore'a odaklı).

## Kapalı issue'lardan öğrenme değeri yüksek olanlar

| Issue | Konu | Değer |
|---|---|---|
| [#14](https://github.com/Miceuz/i2c-moisture-sensor/issues/14) (9 yorum) | Kapasitans okuması besleme gerilimine bağlı | HW davranışını anlamak için kritik |
| [#8](https://github.com/Miceuz/i2c-moisture-sensor/issues/8) (10 yorum) | Aniden tuhaf okumalar | Sensör arıza modları |
| [#3](https://github.com/Miceuz/i2c-moisture-sensor/issues/3) (9 yorum) | Arduino pull-up dirençleri | I2C donanım temelleri |
| [#2](https://github.com/Miceuz/i2c-moisture-sensor/issues/2) (9 yorum) | Işık sensöründe tuhaf okuma | Firmware hata ayıklama |
| [#42](https://github.com/Miceuz/i2c-moisture-sensor/issues/42) | ESP32 sleep sonrası kapasitans okuma hatası | Güç yönetimi/I2C |
| [#41](https://github.com/Miceuz/i2c-moisture-sensor/issues/41) (7 yorum) | I2C aniden çalışmayı bırakıyor | Güvenilirlik |

Bu issue'lar, sensörün bilinen zayıf noktalarını (besleme bağımlılığı, pull-up gereksinimleri, sleep/wake davranışı) gösterir — çoklu sensör dokümantasyonu (#43) yazarken bunları da kapsamak değerli olur.

## Katkı öncelik sırası

1. **[#30 Qwiic konnektörleri](https://github.com/Miceuz/i2c-moisture-sensor/issues/30)** — Bakımcı açıkça PR davet etmiş; la7dja'nın hazır çalışmasını KiCad 8'e taşıyıp PR aç. En somut iş.
2. **[#43 Çoklu sensör ölçümü](https://github.com/Miceuz/i2c-moisture-sensor/issues/43)** — `65535` sorununun teşhisini ve çoklu sensör kılavuzunu dokümante et. I2C uzmanlığına oturur.
3. **[#33 Kısa sensör varyantı](https://github.com/Miceuz/i2c-moisture-sensor/issues/33)** — İşbirliği daveti; ATtiny441 firmware uyarlaması + kalibrasyon. Öğrenme değeri en yüksek.
4. **[#37 ESP32/MQTT/LoRa](https://github.com/Miceuz/i2c-moisture-sensor/issues/37)** — Bakımcının çözümlerini (paper-wifi, LoRa node) README'ye taşıyan küçük dokümantasyon PR'ı.
5. **Kalibrasyon dokümanı ekleme** — #30 yorumlarındaki MAICh kalibrasyon çalışmasını yazar atıfıyla repo'ya ekle.

## Kaynaklar

- [Miceuz/i2c-moisture-sensor](https://github.com/Miceuz/i2c-moisture-sensor) — repo
- Açık issue'lar: [#43](https://github.com/Miceuz/i2c-moisture-sensor/issues/43), [#37](https://github.com/Miceuz/i2c-moisture-sensor/issues/37), [#33](https://github.com/Miceuz/i2c-moisture-sensor/issues/33), [#31](https://github.com/Miceuz/i2c-moisture-sensor/issues/31), [#30](https://github.com/Miceuz/i2c-moisture-sensor/issues/30)
- [la7dja/i2c-moisture-sensor](https://github.com/la7dja/i2c-moisture-sensor) — hazır Qwiic fork'u
- [Miceuz/paper-wifi](https://github.com/Miceuz/paper-wifi) — bakımcının ESP32 çözümü
- [Low power LoRa I2C RS485 node (Tindie)](https://www.tindie.com/products/miceuz/low-power-lora-i2c-rs485-node/) — bakımcının LoRa çözümü
- [MAICh kalibrasyon dokümanı](https://github.com/Miceuz/i2c-moisture-sensor/files/4857624/M.M_Soil.Moisture.Sensor.Calibration.docx) — #30 yorumlarından
