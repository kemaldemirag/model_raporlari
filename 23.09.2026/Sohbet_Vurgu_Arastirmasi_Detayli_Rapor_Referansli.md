# SOHBET VURGU ARAŞTIRMASI

Detaylı Sentez Raporu • 26 Eylül 2026

**Üç sohbet vurgu kaydından türetilen teknik düğümler, ortak mimari, proje/ürün çıkarımı ve araştırma devam hattı**

Ana tez: Görünürde farklı alanlara ait kayıtlar, gerçekte “ölçümün tanımlanması → taşınması → doğrulanması → yorumlanması → işlenmesi → olaya/karara dönüştürülmesi” ortak zincirinde birleşiyor.

## 1. Yönetici Özeti

Bu rapor, paylaşılan üç sohbet vurgusunu yalnızca ayrı bulgular olarak değil, birbirini tamamlayan sistem mühendisliği katmanları olarak ele alır. İlk vurgu ham veriden olay çıkarımını, ikinci vurgu alt sistemler arası arayüz ve veri akışını, üçüncü vurgu ise ölçümün fiziksel ve zamansal anlamını görünür hale getirir.

Bulgular birlikte değerlendirildiğinde en açıklayıcı üst çerçeve “Measurement-to-Decision Pipeline”dır. Bu çerçeve; sensör işleme, ROS 2, CAN/DBC telemetrisi, BMS, MPPT, solar araç elektroniği ve genel olay/anomali algılama çalışmalarını aynı mimaride buluşturabilir.

| **Vurgu** | **Ana teknik düğüm** | **Temel soru** | **Çıktı** |
| --- | --- | --- | --- |
| 1 | Event Abstraction | Ham veriden güvenilir olay nasıl çıkarılır? | Baseline, residual, threshold/peak/change-point, validation, event |
| 2 | Interface Contract & Data Flow | Bir fiziksel büyüklük sistemde hangi sözleşmeyle taşınır? | Firmware variable, CAN/DBC, decoder, telemetry |
| 3 | Measurement Semantics | Bir ölçüm ne zaman gerçekten anlamlı ve geçerlidir? | Değer + geometri + zaman + validity + transport context |

### 1.1 Birleşik sonuç

PHYSICAL WORLD / SYNTHETIC SOURCE

↓

Measurement acquisition

↓

Data / interface contract

↓

Transport

↓

Validity gate

↓

Semantic interpretation

↓

Signal processing

↓

Feature extraction

↓

Event detection

↓

Event abstraction

↓

Decision / alarm / telemetry / control

Bu yapı yalnızca bir kavramsal şema değildir. Modüler bir portföy projesine, test altyapısına veya yeniden konfigüre edilebilir ürün iskeletine dönüştürülebilir.

## 2. Vurgu 1 - Event Abstraction ve Domain-Independent Event Extraction

İlk kayıt, CCD sensör doğrusal olmama/tek elektron çözünürlüğü analizi ile jeopolitik şokların piyasa etkisini inceleyen bir replikasyon paketinin karşılaştırılmasını tartışıyordu. Bu iki alanın aynı algılama yöntemini kullanıp kullanmadığı sorusu yüzeysel kalıyor; asıl ortaklık, ham veriden ayrık ve anlamlı olay üretme işlem hattında.

### 2.1 Ortak işlem primitive'leri

- Baseline/reference estimation: pedestal, moving average, rolling median, beklenen seviye.

- Residual/deviation calculation: gözlem ile beklenen davranış arasındaki fark.

- Candidate detection: threshold crossing, local maximum, prominence, rate-of-change veya jump ölçütü.

- Transition detection: rising/falling edge, hysteresis, debounce.

- Persistence/validation: minimum süre, minimum prominence, valid range, noise floor veya güven ölçütü.

- Event representation: zaman, şiddet, süre, kaynak, kanal/konum ve confidence metadata'sı.

### 2.2 Basit matematiksel soyutlama

Bir sinyal x\_t ve tahmini baseline b\_t için residual:

r\_t = x\_t - b\_t

Eşik tabanlı aday olay:

e\_t = 1\[r\_t > T\]

Yükselen kenar:

rising\_t = e\_t AND NOT e\_(t-1)

Change-point yaklaşımında amaç sadece büyük genlikli tepeyi değil, dağılım parametrelerinin değiştiği τ anını bulmaktır:

x\_t ~ D(theta\_1),  t < tau

x\_t ~ D(theta\_2),  t >= tau

### 2.3 Araştırma değeri

Bu vurgu, tek bir find\_peaks veya threshold örneğini genel bir olay çıkarım çerçevesine yükseltiyor. Böylece farklı veri kaynakları aynı detector arayüzünü kullanabilir.

| **Katman** | **Örnek yöntemler** | **Çıktı** |
| --- | --- | --- |
| Ön işleme | smoothing, detrending, denoising | daha kararlı sinyal |
| Baseline | moving average, rolling median, model-based | beklenen seviye |
| Sapma | residual, derivative, z-score | aday özellik |
| Detector | threshold, peak, change-point | aday olay |
| Validator | prominence, duration, hysteresis | doğrulanmış olay |
| Event model | timestamp, severity, source, confidence | taşınabilir olay nesnesi |

## 3. Vurgu 2 - Interface Contract, CAN/DBC ve Uçtan Uca Veri Akışı

İkinci kayıt solar yarış aracı depolarındaki MPPT firmware, BMS/CAN, güç dağıtımı ve telemetri katmanlarını başlangıç noktası olarak kullanıyordu. Buradaki güçlü yaklaşım, bileşenleri ayrı ayrı tanımlamaktan çok, fiziksel sinyalin tüm sistem boyunca nasıl taşındığını takip etmektir.

### 3.1 Enerji akışı ve bilgi akışı birbirinden ayrılmalı

ENERGY FLOW

PV → MPPT → DC BUS ↔ BATTERY/BMS → PDU → LOADS

INFORMATION FLOW

MPPT/BMS/MOTOR/PDU

↓

CAN BUS

↓

DBC / schema

↓

decoder / logger

↓

telemetry / alarm / analytics

Aynı sistemde enerji akışı fiziksel güç transferini, bilgi akışı ise ölçüm, durum, hata ve komutların taşınmasını temsil eder. İki akış birbiriyle ilişkili ama aynı şey değildir.

### 3.2 Interface contract neyi içerir?

- Signal name ve semantic meaning

- CAN ID / message ID

- bit position, length, endianness

- scale ve offset

- unit

- valid range

- update rate

- timeout/heartbeat davranışı

- fault encoding

- source subsystem ve consumer subsystem

### 3.3 Veri kökeni (data provenance) zinciri

physical signal → ADC/sensor → firmware variable → encoded CAN signal → frame → decoder → telemetry variable → derived metric/event

Bu zincir, telemetride görülen bir değerin gerçekte hangi fiziksel kaynaktan geldiğini ve dönüşüm sırasında nerelerde hata oluşabileceğini izlenebilir hale getirir.

### 3.4 Donanım olmadan yapılabilecek doğrulama

- Synthetic BMS/MPPT/motor-controller veri üreticileri

- CAN frame generator ve replay

- DBC tabanlı encode/decode testleri

- Fault injection: timeout, out-of-range, stuck-at, corrupted scaling

- Telemetry logger ve dashboard

- Golden trace karşılaştırması

- Software-in-the-loop senaryoları

Bu yaklaşım fiziksel doğrulamanın yerini tuttuğu iddiasında değildir; ancak veri sözleşmesi, decoder, hata tespiti ve olay işleme katmanlarını güçlü biçimde doğrulayabilir.

## 4. Vurgu 3 - Measurement Semantics, LaserScan ve Transport Contract

Üçüncü kayıt ROS 2 LaserScan mesajında yalnızca ranges dizisine bakmanın yetersiz olduğunu vurguluyordu. Bu vurgu, ölçümün yalnızca sayısal değer değil, bağlam taşıyan bir veri nesnesi olduğunu gösteriyor.

### 4.1 Ölçümün beş bileşeni

Measurement = value + geometry + time + validity + transport context

| **Bileşen** | **LaserScan karşılığı** | **Neden gerekli?** |
| --- | --- | --- |
| Value | ranges, intensities | ölçülen mesafe/geri dönüş bilgisi |
| Geometry | angle\_min, angle\_max, angle\_increment | ışının uzaydaki yönü |
| Time | header.stamp, time\_increment, scan\_time | ölçümün zamanlaması |
| Validity | range\_min, range\_max, finite check | ölçümün fiziksel olarak kabul edilebilirliği |
| Transport | QoS reliability/history/depth | mesajın tüketiciye ulaşma davranışı |

### 4.2 “Dizinin ortası = robotun önü” varsayımının sınırı

ranges\[ranges.size()/2\] ifadesi ancak tarama geometrisi simetrik ve merkez indeksin 0 rad yönüne karşılık geldiği durumda “ön mesafe” anlamına gelir. Daha genel yaklaşım hedef açıdan indeks hesaplamaktır:

index ≈ (target\_angle - angle\_min) / angle\_increment

### 4.3 Validity gate

finite(range)

AND range >= range\_min

AND range <= range\_max

Bu kontrol yalnızca defensive programming değildir; ölçümün sonraki filtreleme ve event detection katmanına kabul edilip edilmemesini belirleyen bir veri kalitesi kapısıdır.

### 4.4 Intensities alanı

intensities alanı sensörün geri dönüş sinyaline ilişkin cihaz-bağımlı nicelikler sağlayabilir. Ancak evrensel bir fiziksel birim gibi yorumlanmamalı; sensör/sürücü dokümantasyonu ile birlikte ele alınmalıdır. Potansiyel kullanım alanları arasında yansıma karakteri, düşük güvenilirlikli return filtreleme ve range-intensity fusion bulunur.

### 4.5 QoS = transport contract

Publisher veri üretse bile subscriber'ın veri alması garanti değildir. Reliability, history, depth ve durability gibi QoS parametreleri taşıma sözleşmesinin parçasıdır. Bu nedenle “topic var” ile “uygulama doğru veri alıyor” aynı doğrulama değildir.

## 5. Üç Vurgunun Birbirine Bağlandığı Noktalar

| **Ortak soru** | **Vurgu 1** | **Vurgu 2** | **Vurgu 3** |
| --- | --- | --- | --- |
| Veri nereden geliyor? | ham seri/görüntü | fiziksel alt sistem | LiDAR return |
| Nasıl tanımlanıyor? | baseline + feature | DBC/schema | LaserScan message fields |
| Nasıl taşınıyor? | dosya/işlem hattı | CAN bus | DDS/ROS 2 |
| Ne zaman geçerli? | noise/threshold criteria | range/timeout/fault semantics | range\_min/max + finite + QoS |
| Nasıl olaya dönüşüyor? | detector/validator | telemetry/fault event | obstacle/proximity/event |

### 5.1 Üst kavram: Data Contract

İkinci ve üçüncü vurguların ortak paydası Data Contract'tır. CAN/DBC ve ROS message semantics farklı teknolojiler olsa da her ikisi de verinin anlamını, birimini, biçimini ve tüketici beklentisini sözleşmeye bağlar.

### 5.2 Üst kavram: Measurement-to-Decision Pipeline

Birinci vurgu bu veriyi karara dönüştüren analitik katmanı ekler. Böylece veri sözleşmesi ile olay çıkarımı tek uçtan uca zincirde birleşir.

SOURCE

↓

ADAPTER / SCHEMA

↓

VALIDATION

↓

SEMANTIC INTERPRETATION

↓

NORMALIZATION / FILTERING

↓

FEATURE EXTRACTION

↓

DETECTOR

↓

EVENT MODEL

↓

LOGGER / DASHBOARD / ALARM / CONTROL

## 6. Portföy Projesi / Ürün İskeleği

Bulgular, tek bir sensör veya tek bir domain'e kilitlenmeyen yeniden yapılandırılabilir bir proje iskeletine dönüştürülebilir.

### 6.1 Önerilen proje adı

Configurable Measurement-to-Event Pipeline

### 6.2 Modüller

| **Modül** | **Sorumluluk** | **Örnek** |
| --- | --- | --- |
| Source adapters | ham veriyi ortak yapıya almak | CSV, synthetic CAN, ROS2 LaserScan |
| Schema/contract | alan, birim, ölçek, metadata | DBC benzeri config, ROS field mapping |
| Validator | geçerlilik ve kalite kapısı | range, finite, timeout, stale |
| Interpreter | geometri/zaman/ölçek çözümü | angle mapping, CAN scaling |
| Processor | filtre ve baseline | moving average, median, detrend |
| Detectors | olay adayı üretme | threshold, peak, change-point |
| Event validator | yanlış pozitif azaltma | persistence, debounce, prominence |
| Event model | ortak olay nesnesi | timestamp, severity, source, confidence |
| Outputs | kayıt/görsel/uyarı | JSONL, dashboard, alarm |

### 6.3 Örnek event modeli

Event {

timestamp

source

signal

value

baseline

residual

detector

severity

confidence

metadata

}

### 6.4 Konfigürasyonla yeniden kullanım

Aynı çekirdek farklı use-case'lere yalnızca adapter, schema ve detector config'i değiştirilerek uyarlanabilir. Bu özellik freelance/portföy açısından projeyi bir demo olmaktan çıkarıp yeniden kullanılabilir blueprint'e dönüştürür.

## 7. Doğrulama ve Test Stratejisi

Bu mimarinin güvenilir olması için yalnızca happy-path değil, veri sözleşmesi ve hata davranışları da test edilmelidir.

| **Test sınıfı** | **Örnek** | **Beklenen kontrol** |
| --- | --- | --- |
| Unit | CAN scale/offset decode | beklenen mühendislik değeri |
| Boundary | range = range\_min/range\_max | doğru kabul/ret davranışı |
| Invalid input | NaN/Inf/out-of-range | event katmanına geçmemeli |
| Transport | QoS mismatch / CAN timeout | diagnostic event |
| Replay | kayıtlı trace | deterministik sonuç |
| Fault injection | stuck sensor / dropped frame | fault classification |
| Regression | golden dataset | çıktı değişikliklerinin yakalanması |

### 7.1 Donanım olmadan kanıt üretme

- Synthetic dataset + expected-event annotations

- Replayable CAN/ROS message traces

- Unit/integration test raporları

- Plots: raw vs filtered vs baseline vs event markers

- Schema validation çıktıları

- Fault injection sonuç matrisi

- README'de sınırlar: fiziksel donanım doğrulaması yapılmadı

## 8. Araştırma Devam Hatları

Üç vurgu mevcut zinciri güçlü biçimde kuruyor; ancak henüz kapsanmayan doğal dallar aşağıdaki gibi.

| **Yeni eksen** | **Neden önemli?** | **Araştırma sorusu** |
| --- | --- | --- |
| State estimation | tek ölçüm yerine sistem durumu | çoklu/gürültülü veriden latent state nasıl tahmin edilir? |
| Sensor fusion | birden fazla sensör/kanal | hangi fusion seviyesi daha uygun? |
| Diagnostics | algılama ile arıza ayrımı | data fault mı gerçek fiziksel event mi? |
| Confidence scoring | ikili event yerine güven skoru | confidence nasıl kalibre edilir? |
| Time synchronization | çoklu kaynak korelasyonu | timestamp skew nasıl yönetilir? |
| Provenance | değerin kaynağını izleme | bir dashboard metriği hangi ham sinyallerden türedi? |
| Replay verification | donanımsız test | aynı trace aynı çıktıyı veriyor mu? |

## 9. Anahtar Kelime ve Arama Haritası

### 9.1 Event extraction

event detection, anomaly detection, threshold crossing, peak detection, prominence, rising edge, hysteresis, debouncing, persistence filtering, change-point detection, residual analysis, event segmentation

### 9.2 Interface / transport

interface contract, data contract, CAN signal definition, DBC, message schema, scaling, offset, endianness, heartbeat, timeout, telemetry pipeline, message replay, QoS compatibility

### 9.3 Measurement semantics

measurement validity, sensor metadata, timestamp semantics, scan geometry, intensity semantics, range validation, stale data, confidence, uncertainty

### 9.4 Verification

software-in-the-loop, replay-based testing, fault injection, golden trace, synthetic sensor data, schema validation, contract testing, deterministic replay

## 10. Sınırlar ve Dikkat Edilmesi Gerekenler

- Farklı domain'lerde aynı kavramsal pipeline'ın bulunması, aynı algoritmanın kullanıldığı anlamına gelmez.

- Event abstraction, domain bilgisinin yerine geçmez; yalnızca ortak yazılım mimarisi sağlar.

- CAN/DBC veya ROS message semantiğinin doğru yorumlanması her zaman ilgili resmi dokümantasyonla doğrulanmalıdır.

- QoS veya timeout sorunları veri yokluğu yaratabilir; bu durum sensörün fiziksel olarak arızalı olduğunu tek başına kanıtlamaz.

- intensities gibi alanlar sensör bağımlıdır; doğrudan fiziksel yorum yapılmamalıdır.

- Donanım olmadan yapılan SIL/replay testleri yazılım ve veri sözleşmesini doğrular, fiziksel performansı kanıtlamaz.

## 11. Kaynak Notları

Bu rapor, paylaşılan üç sohbet vurgu kaydının sentezi ve önceki araştırma turunda yapılan kısa dış kaynak kontrolleri üzerine kuruludur. Kavramsal doğrulama için aşağıdaki kaynak aileleri kullanılmıştır:

- ROS 2 documentation - LaserScan message definition ve QoS concepts (docs.ros.org)

- Illini Solar Car - public GitHub/CAN documentation (wiki.illinisolarcar.com)

- UVA Solar Car - embedded projects, CAN logger/generator ve test örnekleri

- Sunseeker Solar Car Project - Python telemetry örnekleri

- Prohelion ArrowPoint Telemetry - CAN capture/storage/reporting yaklaşımı

Not: İlk ekran görüntüsünde görünen bazı belirli GitHub depo yolları güncel web erişiminde doğrulanamadığı için, depoya özgü ayrıntılar kesin gerçeklik iddiası olarak kullanılmamıştır.

## 12. Sonuç

Bu üç vurgu, tek tek incelendiğinde CCD/olay analizi, solar araç elektroniği ve ROS 2 LiDAR gibi ayrı başlıklar görünür. Birlikte incelendiğinde ise çok daha değerli bir desen ortaya çıkar: ölçümün anlamlı bir karar nesnesine dönüşmesi için yalnızca detector değil; veri sözleşmesi, metadata, transport, validity, interpretation ve doğrulama katmanları da gerekir.

Bu nedenle araştırmanın en güçlü üst çerçevesi “Measurement-to-Decision Pipeline”; portföy/ürün açısından en güçlü çıktı ise adapter + contract + validator + processor + detector + event model + replay/test bileşenlerinden oluşan yeniden yapılandırılabilir bir altyapıdır.

## 13. Çoklu Platform Referans Listesi

Bu bölüm, rapordaki bulguların devam araştırması için GitHub, YouTube, Pinterest ve Scribd kaynaklarını aynı referans şemasında toplar. Liste; Measurement-to-Decision Pipeline, event abstraction, CAN/DBC ve telemetri, ROS 2 LaserScan, QoS/transport contract, measurement validity ve replay/simulation eksenleri temel alınarak düzenlenmiştir.

### 13.1 GitHub Referans Listesi

Kod, gerçek repo mimarisi, veri sözleşmesi, telemetri, replay/test ve event-detection uygulamaları için birincil teknik referans kümesi.

- **deepcharles/ruptures —** Change-point detection; event abstraction ve regime-shift tespiti. [Bağlantı](https://github.com/deepcharles/ruptures)

- **Lucew/changepoynt —** Online/offline change-point detection; değiştirilebilir detector mimarisi. [Bağlantı](https://github.com/Lucew/changepoynt)

- **mireo/can-utils —** DBC parsing, CAN sinyal çözümleme ve telemetri işleme. [Bağlantı](https://github.com/mireo/can-utils)

- **commaai/opendbc —** CAN/DBC veri sözleşmesi ve üst seviye araç arayüzü. [Bağlantı](https://github.com/commaai/opendbc)

- **richhaar/can-bus-visualizer —** CAN log + DBC decode + zaman serisi görselleştirme. [Bağlantı](https://github.com/richhaar/can-bus-visualizer)

- **AmirrezaRoodsaz/ev-can-bms-telemetry —** BMS → CAN → DBC → telemetry hattına yakın portföy referansı. [Bağlantı](https://github.com/AmirrezaRoodsaz/ev-can-bms-telemetry)

- **ciandonovan/ros2\_qos\_reliability —** ROS 2 reliable/best-effort QoS davranışlarını deneysel inceleme. [Bağlantı](https://github.com/ciandonovan/ros2_qos_reliability)

- **mich1342/ros2\_laser\_scan\_merger —** ROS 2 LaserScan işleme ve çoklu tarama verisi birleştirme. [Bağlantı](https://github.com/mich1342/ros2_laser_scan_merger)

### 13.2 YouTube Referans Listesi

Uygulamalı öğrenme, iş akışı gözlemi ve kavramların gerçek araçlar üzerindeki karşılığını görmek için.

- **DBC File Explained - A Simple Intro —** DBC signal, start bit, length, scale ve offset kavramlarını görsel olarak anlamak için. [Bağlantı](https://www.youtube.com/watch?v=jvYkFr96Nss)

- **Change Point Detection with ruptures —** Regime shift ve change-point detection uygulaması. [Bağlantı](https://www.youtube.com/watch?v=ZHfIG2JZQhs)

- **ROS 2 LaserScan araması —** ranges, angle\_increment, timestamps ve scan geometry örnekleri. [Bağlantı](https://www.youtube.com/results?search_query=ROS2+LaserScan+sensor_msgs+LaserScan)

- **ROS 2 QoS araması —** best\_effort, reliable ve DDS/QoS uyumluluğu. [Bağlantı](https://www.youtube.com/results?search_query=ROS2+QoS+best+effort+reliable+DDS)

- **CAN + BMS + DBC telemetry araması —** Solar/EV veri zincirini uygulamalı takip etmek için. [Bağlantı](https://www.youtube.com/results?search_query=CAN+DBC+BMS+telemetry+EV)

- **SocketCAN / vcan / replay araması —** Fiziksel donanım olmadan CAN simülasyonu ve replay için. [Bağlantı](https://www.youtube.com/results?search_query=SocketCAN+vcan+candump+CAN+replay+tutorial)

### 13.3 Pinterest Referans Listesi

Teknik doğrulama kaynağı olarak değil; blok diyagram, dashboard, sistem mimarisi ve sunum görselleştirmesi için kullanılmalıdır.

- **CAN Bus Architecture / DBC —** ECU → CAN → logger → decoder → dashboard görsel dili. [Bağlantı](https://www.pinterest.com/search/pins/?q=CAN+bus+architecture+DBC)

- **ROS 2 LiDAR Architecture —** LiDAR → driver → /scan → processing → navigation diyagramları. [Bağlantı](https://www.pinterest.com/search/pins/?q=ROS2+LiDAR+architecture+LaserScan)

- **Embedded Telemetry Dashboard —** BMS/MPPT/CAN telemetry dashboard yerleşimleri. [Bağlantı](https://www.pinterest.com/search/pins/?q=embedded+telemetry+dashboard+CAN)

- **Signal Processing Pipeline —** filter → feature extraction → event detection blok diyagramları. [Bağlantı](https://www.pinterest.com/search/pins/?q=signal+processing+event+detection+pipeline)

- **Robotics Sensor Fusion —** İleride eklenecek sensor-fusion katmanı için görsel referanslar. [Bağlantı](https://www.pinterest.com/search/pins/?q=robotics+sensor+fusion+architecture)

### 13.4 Scribd Referans Listesi

Ders notu, proje dokümanı, geniş bağlam ve sistem mühendisliği okuması için ikincil kaynak havuzu.

- **Model-Based Design of Distributed ROS2 Systems Using IEC 61499 —** Distributed ROS 2, pub/sub ve event-based kontrol mimarisi. [Bağlantı](https://www.scribd.com/document/806781360/Model-Based-Design-of-Distributed-ROS2-Systems-Using-IEC-61499)

- **Basic ROS2 - LaserScan Subscriber —** LaserScan, /scan ve QoS profile kullanımına giriş. [Bağlantı](https://www.scribd.com/document/994415835/p1-Basic-Ros2)

- **Obstacle Avoidance with DWA —** LiDAR ölçümünün navigation/obstacle avoidance kararına dönüşümü. [Bağlantı](https://www.scribd.com/document/965353269/07-08-09-Obstacle-Avoidance-With-DWA)

- **ROS 2 Mobile Robotics Algorithms Survey —** Laser scanner, localization ve navigation için geniş mimari bağlam. [Bağlantı](https://www.scribd.com/document/923846857/2307-15236v2)

- **Automotive SW Engineering Smart Handbook 2026 —** CAN arbitration ve araç içi yazılım/iletişim mimarisi. [Bağlantı](https://www.scribd.com/document/1029069544/Automotive-SW-Engineering-Smart-Handbook-2026-by-Ashish)

- **Embedded Software Interview Cheat Sheet —** Sensör → filtre/safety logic → motor controller → CAN/DBC uygulama akışı. [Bağlantı](https://www.scribd.com/document/901090639/Embedded-Software-Interview-Cheat-Sheet)

### 13.5 Platformların Araştırmadaki Rolü

| **Platform** | **Birincil rol** | **Bu raporda kullanım şekli** |
| --- | --- | --- |
| GitHub | Kod/repo/test mimarisi | Gerçek uygulama ve yeniden kullanılabilir modül çıkarımı |
| YouTube | Uygulamalı öğrenme | Araçların ve veri akışlarının adım adım gözlemi |
| Pinterest | Görsel referans | README/blok diyagram/dashboard sunum dili |
| Scribd | Doküman/rapor bağlamı | Ders notu, proje raporu ve geniş literatür taraması |

### 13.6 Önerilen Referans Kayıt Şeması

Referans Listesi klasöründe her kaydın aşağıdaki alanlarla tutulması önerilir:

- Başlık

- Platform

- Bağlantı

- Anahtar Kavram

- İstifade Noktası

- İlgili Vurgu

- Projeye Aktarılabilir Çıktı

- Doğrulama Notu

### 13.7 Önerilen İnceleme Sırası

GitHub tarafında ilk inceleme sırası: opendbc → can-utils → ev-can-bms-telemetry → can-bus-visualizer → ros2\_laser\_scan\_merger → ros2\_qos\_reliability → ruptures/changepoynt. Bu sıra, data contract → decode → telemetry → measurement → transport → event detection akışını takip eder.
