**ÜRÜN-4 / ÜRÜN PLANI**

# Robotik Test Kanıt Kapısı

CAN ve ROS 2 kayıtları için kalite, replay ve denetlenebilir kanıt paketi

| **Ürün kararı — koşullu GO**<br>İlk pazara giriş kaması, mevcut CAN telemetri çekirdeğini canlı vcan ve Mosquitto koşularıyla tamamlayan CAN Telemetry Reliability Pack’tir. ROS 2 rosbag2/MCAP adaptörü ikinci dilimdir. Fiziksel ESP32/TWAI doğrulaması tamamlanana kadar fiziksel performans durumu UNVERIFIED kalır. |
| --- |

| **Ürün** | Ürün-4 — Robotik Test Kanıt Kapısı |
| --- | --- |
| **source\_origin** | folder\_extraction |
| **Kesim tarihi** | 21.11.2026 23:59:59 Europe/Istanbul |
| **Ana kullanıcı** | Robotik entegratörleri, gömülü sistem ekipleri, test ve kalite mühendisleri |
| **İlk ürün biçimi** | Yerel çalışan CLI ve sabit kapsamlı doğrulama paketi |
| **Kayıtlı kanıt** | Windows 11 ve Python 3.12.10 üzerinde 42/42 birim testi |
| **Açık doğrulamalar** | Canlı SocketCAN/Mosquitto entegrasyonu, hata koşuları ve tam kanıt paketi |
| **Fiziksel durum** | UNVERIFIED — gerçek CAN ağı ve ESP32/TWAI koşusu yapılmadı |

## Sorun

Robotik ve IoT ekipleri CAN çerçeveleri, MQTT olayları ve ROS 2 kayıtları üretiyor; ancak bu kayıtların eksiksizliği, zaman tutarlılığı, tekrar oynatılabilirliği ve hata koşullarındaki davranışı çoğunlukla farklı komutlar ve el yapımı raporlarla doğrulanıyor. Sonuç, aynı hatanın farklı ortamlarda tekrar üretilememesi ve kararın ham kanıta izlenememesidir.

### Ürün vaadi

Tek bir senaryo manifestinden veri akışını çalıştırır veya mevcut kaydı içe alır; ham ve kanonik olayları saklar; kayıp, tekrar, sıra, zaman ve yeniden bağlanma ölçümlerini hesaplar; replay sonucunu karşılaştırır ve hash’li bir kanıt paketi üretir.

**KAPSAM VE MIMARI**

## Ürün sınırı

| **Ürün** | **Odak** | **Ürün-4 ile sınır** |
| --- | --- | --- |
| Ürün-1 | ECAD, DRC/ERC, Gerber, BOM ve üretim paketi | KiCad/DFM yeni ürün değil; Ürün-1 modülü olarak kalır. |
| Ürün-2 | AnyShake Explorer deposunun yeniden üretimi | Belirli bir sensör ürününü kopyalamaz; veri akışının güvenilirliğini sınar. |
| Ürün-3 | ROBOTIS MuJoCo Menagerie deposunun yeniden üretimi | Model varlığı üretmez; davranış ve kayıt kanıtını doğrular. |
| Ürün-4 | CAN, MQTT ve ROS 2 kayıtları için test, replay ve kanıt kapısı | Ortak test çekirdeği; HIL yalnız sonraki fazdır. |

## MVP kapsamı

- SocketCAN/vcan, MQTT v5 ve SQLite ham/kanonik olay deposu için tekrarlanabilir senaryo koşusu.

- Sıra bütünlüğü, kayıp, tekrar, gecikme, kuyruk taşması, yeniden bağlanma ve ACK-after-commit kontrolleri.

- Salt okunur replay; senaryo ve sonuç hashleri; araç sürümleriyle birlikte kanıt manifesti.

- rosbag2/MCAP veya DB3 kayıtları için konu envanteri, oran, zaman damgası, boşluk ve senkronizasyon denetimi.

- İlk sürümde hesap, bulut ve sürekli internet gerektirmeyen yerel çalışma.

### MVP dışında

- Sertifikasyon, yasal uygunluk veya güvenlik onayı verme.

- Gerçek CAN ağı kurulmadan fiziksel ESP32/TWAI performansı iddia etme.

- DBC çözümleme, genel plant modeli ve sub-millisecond gerçek zaman garantisi.

- İlk sürümde uzaktan laboratuvar yönetimi veya otomatik donanım güç kontrolü.

## Mimari akış

**1. Kaynak:** SocketCAN, MQTT, rosbag2/MCAP veya kontrollü fixture kaydı.

**2. Adaptör:** Kaynağı tek olay sözleşmesine taşır; saat alanını ve kaynak kimliğini korur.

**3. Ham ve kanonik kayıt:** Ham girdiyi değişmeden, normalize edilmiş olayı ayrı SQLite alanında saklar.

**4. Kalite kuralları:** Sıra, boşluk, tekrar, zaman, reconnect ve taşma ölçümlerini üretir.

**5. Replay:** Kaydı salt okunur yeniden oynatır ve kanonik sonuç eşliğini karşılaştırır.

**6. Kanıt yazıcısı:** Manifest, metrik, zaman çizelgesi, sonuç ve SHA-256 listesini tek pakette toplar.

**KABUL KAPILARI**

## G0–G8 kabul modeli

Kapı kararı yalnız zorunlu kanıtlar tamamlandığında yükseltilir. Birim test kaydı, canlı entegrasyon veya fiziksel doğrulama yerine geçmez. Ayrıntılı yürütme alanları eşlik eden Koşu, Kabul ve Kanıt Matrisi içindedir.

| **Kapı** | **Ad** | **Geçiş ölçütü** |
| --- | --- | --- |
| G0 | Kaynak | Commit, lisans, bağımlılık ve SHA-256 manifesti eksiksiz. |
| G1 | Sözleşme | Olay şeması, MQTT, SQLite ve replay kuralları sürümlü ve test edilebilir. |
| G2 | Ortam | Sabit Linux/WSL, vcan, Mosquitto ve araç sürümleri kayıtlı. |
| G3 | Birim | Mevcut 42 test temiz hedef ortamda yeniden geçer. |
| G4 | Nominal entegrasyon | 1–100 sıra kümesi eksiksiz; beklenmeyen veya yinelenen kanonik olay yok; senaryo ve sonuç hashleri kayıtlı. |
| G5 | Hata koşulları | Broker kesintisi ve kuyruk taşması görünür; reconnect, kayıp ve tekrar sayaçları kanıtta. |
| G6 | Kanıt paketi | Kaynak SHA, araç sürümleri, senaryo, ham/kanonik sonuç, metrik, zaman çizelgesi, replay ve checksum manifesti eksiksiz. |
| G7 | Fiziksel | ESP32/TWAI ve gerçek CAN kurulana kadar UNVERIFIED; güvenli teardown ve elektriksel sınırlar ayrıca onaylanır. |
| G8 | ROS 2 genişlemesi | MCAP/DB3 envanteri, oran, zaman, boşluk, senkronizasyon ve replay eşliği doğrulanır. |

## Başlangıç durumu

| **Alan** | **Durum** | **Yorum** |
| --- | --- | --- |
| Birim testleri | Kayıtlı | 42/42 sonuç var; G3 için hedef ortamda yeniden koşu gerekir. |
| Canlı entegrasyon | Engelli | Linux/WSL, vcan ve Mosquitto ortamı kurulmadan G4 ve G5 yürütülemez. |
| Kanıt paketi | Kısmi | Manifest, metrik, replay ve checksum öğeleri tek paket halinde tamamlanmalı. |
| Fiziksel doğrulama | UNVERIFIED | Gerçek CAN ağı ve ESP32/TWAI kurulumu bulunmuyor. |
| ROS 2 adaptörü | Planlandı | MCAP/DB3 inceleme ve replay eşliği dördüncü hafta dilimidir. |

| **İlk engel**<br>G2 tamamlanmadan canlı güvenilirlik iddiası yapılamaz. İlk teknik iş, sürümlü Linux/WSL ortamında vcan ve Mosquitto ile tek komutluk nominal koşuyu kurmaktır. |
| --- |

**UYGULAMA PLANI**

## Dört haftalık yürütme

| **Hafta** | **Amaç** | **İş paketi** | **Çıkış ölçütü** |
| --- | --- | --- | --- |
| 1 | Temel ortam | Bağımlılıkları sabitle; WSL/Linux, vcan ve Mosquitto kur; G0–G2 manifestini üret; 42 testi yeniden koş. | G0–G3 kanıtları hedef ortamdan alınmış. |
| 2 | Nominal akış | 1–100 sıra fixture’ını canlı çalıştır; raw/canonical SQLite, metrik, sonuç ve hashleri üret. | G4 kabul ölçütü tekrarlı iki koşuda sağlanmış. |
| 3 | Hata ve kanıt | Broker kesintisi, reconnect ve kuyruk taşması uygula; replay eşliğini ve tam kanıt paketini doğrula. | G5 ve G6 bağımsız incelemeye hazır. |
| 4 | ROS 2 dilimi | Bir MCAP/DB3 örneğinde konu, oran, timestamp, gap, sync ve replay raporu üret; pilot demosunu paketle. | G8 minimum raporu ve yeniden üretim komutu mevcut. |

## Teslim paketi

| **Teslim** | **Asgari içerik** | **Kabul bağı** |
| --- | --- | --- |
| Çalıştırıcı | Yerel CLI, sürümlü yapılandırma ve tek komut nominal koşu | G2–G4 |
| Senaryo manifesti | Kaynak, fixture, zaman alanı, fault adımları ve beklenen ölçütler | G1, G4, G5 |
| Sonuç deposu | Ham ve kanonik olaylar, ölçümler ve koşu kimliği | G4–G6 |
| Replay raporu | Girdi ve sonuç hashleri, farklar ve karar | G6, G8 |
| Kanıt paketi | Araç sürümleri, stdout/stderr, metrik, zaman çizelgesi, sonuç, checksum listesi | G0–G6 |
| Kabul matrisi | Kapı, koşu, kanıt, sorumlu, inceleyen ve tarih alanları | Tüm kapılar |

## İşletim akışı

**1. Hazırla:** Kaynak SHA, araç sürümü, fixture ve güvenlik sınırlarını sabitle.

**2. Çalıştır:** Nominal veya fault senaryosunu benzersiz koşu kimliğiyle başlat.

**3. Topla:** Ham/kanonik kayıtları, metrikleri, zaman çizelgesini ve stdout/stderr’i paketle.

**4. Replay et:** Aynı girdiyle sonucu yeniden üret ve farkı ölç.

**5. İncele:** Kabul matrisini doldur; zorunlu kanıt eksikse kapıyı Geçti yapma.

**RISK, TICARI DOĞRULAMA VE PROVENANS**

## Başlıca riskler ve kontroller

| **Risk** | **Etkisi** | **Kontrol** |
| --- | --- | --- |
| Birim testini entegrasyon kanıtı saymak | Yanlış güven | G3, G4 ve G5’i ayrı tut; canlı servis ve fault koşusu zorunlu. |
| Saat alanı belirsizliği | Yanlış gecikme veya sıra kararı | Kaynak saati, dönüşüm ve belirsizlik alanlarını kanıtta sakla. |
| Replay’in kaynağı değiştirmesi | Kanıt bütünlüğü kaybı | Replay’i salt okunur yap; girdi ve sonuç hashlerini karşılaştır. |
| Fiziksel güvenlik varsayımı | Donanım veya operatör riski | G7 ayrı; E-stop, limit, sigorta ve güvenli teardown onayı olmadan otomasyon yok. |
| Sertifikasyon izlenimi | Yanlış ürün iddiası | Raporu teknik kanıt olarak konumlandır; uygunluk veya sertifika vaat etme. |
| Büyük kayıt hacmi | Yavaş analiz ve disk baskısı | Akışlı okuma, bounded sorgu, saklama politikası ve koşu başına boyut ölçümü. |

## İlk ticari doğrulama

- Sabit kapsam: bir CAN telemetri hattı, bir nominal senaryo, iki fault senaryosu ve teslim edilen kanıt paketi.

- Satış öncesi ölçüt: bir pilot ekip kendi kaydını aynı komutla yeniden üretip kapı kararını ham kanıta izleyebilmeli.

- Ürünleşme kararı: G2–G6 geçmeden CI lisansı veya fiziksel HIL paketi vaat edilmemeli.

- Sonraki faz: ROS 2 adaptörü talebi doğrulanırsa MCAP/DB3 desteği ortak çekirdeğe eklenir; HIL adaptörleri ayrı ücretli modül olur.

## Kaynak manifesti

Aşağıdaki kaynakların tamamı 21.11.2026 tarih kesimi içindedir. tmp altındaki çalışma kopyaları ve 25.11.2026 tarihli sonraki fırsat raporu bu planın çekirdek kanıt manifestine alınmamıştır.

| **Kaynak** | **Konum** | **Plan içindeki rol** |
| --- | --- | --- |
| README.md | IoT\_can-telemetry-sil | Çekirdek bileşenler, 42 test, canlı ve fiziksel doğrulama sınırları |
| TASKS.md | IoT\_can-telemetry-sil | Kayıtlı test ortamı, açık G4/G5 koşuları ve UNVERIFIED durumu |
| a04\_is\_talebi.md | 21.11.2026 ajan\_notlari | ROS 2, sensör pipeline, HIL/SIL, CI ve kanıt paketi talep sinyalleri |
| a09\_ros2\_sim\_sorun.md | 21.11.2026 ajan\_notlari | Senaryo kapsülü, determinizm, zaman/QoS ve kanıt paketi gereksinimleri |
| a13\_hil\_orchestrator.md | 21.11.2026 ajan\_notlari | Yerel HIL orkestrasyonu, CAN/ROS 2 adaptörleri ve güvenlik sınırları |
| a28\_onceliklendirme.md | 21.11.2026 ajan\_notlari | ScenarioGate ve HIL önceliği, koşullu GO ve yeniden puanlama ölçütleri |
