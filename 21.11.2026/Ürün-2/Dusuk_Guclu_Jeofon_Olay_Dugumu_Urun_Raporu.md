**ÜRÜN 2 · 26 KASIM 2026**

# Düşük Güçlü Jeofon Olay Düğümü Ürün Raporu

Saha titreşim olaylarını yerelde kaydeden ve LoRaWAN ile özetleyen mühendislik düğümü

**Ürün kararı**

**Dikey jeofon ve üç eksenli ivmeölçerden gelen veriyi düşük güçte sürekli örnekleyen; olayları RMS ve STA LTA tabanlı kurallarla yerelde yakalayan; ham kaydı cihazda tutup yalnız olay özetini ve sağlık bilgisini LoRaWAN üzerinden gönderen saha düğümü.**

**Birincil kullanıcı:** Titreşim izleme entegratörleri ve saha mühendisliği ekipleri

**İlk teslim:** Üç mühendislik örneği ve bir alıcı kod çözücü ile ücretli pilot

**Kanıt düzeyi:** Ürün hipotezi — Ölçülmüş saha performansı değildir

**İç kabul kapısı:** Ürün 1 PCB Release Evidence Gate

**Drive işlemi:** Yerel çalışma — Drive'a yazılmadı

## 1 Yönetici kararı

Yeni ürün fırsatı, 21.11.2026 başlıklı klasörlerdeki iki ayrıntılı iş ilanı ekranı, AnyShake ve sensör tasarım araştırmaları, hassas ADC kaynakları ve düşük güçlü sismik algılama patent kaydı birlikte değerlendirilerek çıkarılmıştır. Kayıtlardaki doğrudan ihtiyaç; yaklaşık 500 örnek saniye jeofon alımı, 0,25 saniyelik titreşim özeti, olay algılama, eğim ve salınım bağlamı, GNSS ile kurulum bilgisi, LoRaWAN haberleşmesi ve kapalı saha muhafazasıdır.

Önerilen ürün sertifikalı sismograf veya deprem erken uyarı cihazı değildir. İlk sürüm, olay taraması ve mühendislik değerlendirmesi için düşük maliyetli bir saha düğümüdür. Ham dalga biçimini yerelde korur; radyo üzerinden sürekli ham veri taşımak yerine olay başlangıcı, periyodik olay özeti, olay sonu ve cihaz sağlık bilgisini iletir. Bu sınır, enerji ve haberleşme bütçesini yönetilebilir tutar.

Ürün 1 bu ürüne dönüşmez. PCB Release Evidence Gate, kartın üretim paketini ve revizyon kanıtını doğrulayan iç kabul kapısı olarak kullanılır. Ürün 2'nin değer önerisi ise fiziksel algılama, olay kaydı, düşük güç ve saha haberleşmesidir.

### Kararı destekleyen kayıtlar

| **Kayıt** | **Doğrudan bulgu** | **Ürün etkisi** |
| --- | --- | --- |
| IMG 5058 ve 5059 | Dikey jeofon, hassas ADC, ADXL355 sınıfı ivmeölçer, GNSS, LoRaWAN, LiFePO4, 500 SPS DMA, 0,25 s RMS ve STA LTA gereksinimi | Gerçek müşteri problemine en yakın ürün sözleşmesi |
| AnyShake Explorer | Üç jeofon, ADS1262, IMU, GNSS ve seri veri zinciri; donanım ve firmware kaynakları | Sinyal zinciri ve veri biçimi için açık referans |
| Sensor Design eki | 500 SPS ve 250 ms pencere için sentetik titreşim, clipping, girişim ve yanlış tetikleme fixture'ları | Donanımsız ilk doğrulama yolu |
| Omron sismik sensör patenti | Düşük güç için eşik ve çalışma modu yaklaşımı | Güç mimarisi için referans; FTO incelemesi zorunlu |
| TI hassas ADC ve açık titreşim düğümleri | ADC, analog ön uç ve LoRaWAN tasarım örnekleri | Parça seçimi ve bench karşılaştırması için kaynak |

## 2 Müşteri ve kullanım sınırı

### Birincil müşteri

- Şantiye, kazı, altyapı ve endüstriyel saha titreşimini ön elemeden geçirmek isteyen sistem entegratörleri

- Kablo veya sürekli geniş bant bağlantısı olmayan noktalarda olay kaydı isteyen mühendislik ekipleri

- Kendi analiz platformuna ham olay dosyası ve LoRaWAN özeti bağlamak isteyen cihaz üreticileri

### Kullanıcının tamamlamak istediği iş

Sahadaki sürekli titreşimi düşük güçle izlemek, anlamlı bir olay başladığında zamanını ve şiddet özetini görmek, olayın ham verisini kaybetmemek ve cihazın eğim, bağlantı ve enerji durumunu uzaktan takip etmek.

### Kapsam dışı iddialar

- Deprem erken uyarı, can güvenliği veya mevzuata bağlı alarm işlevi

- Kalibre edilmiş profesyonel sismografla eşdeğer metrologi

- Sertifikalı patlatma veya yapı hasarı limit değerlendirmesi

- Saha karşılaştırması yapılmadan menzil, batarya ömrü, gürültü tabanı veya algılama mesafesi garantisi

- AnyShake veya patentli bir tasarımın lisans ve FTO kararı olmadan kopyalanması

## 3 Ürün mimarisi

| **Katman** | **Bileşen ve işlev** | **Çıktı** |
| --- | --- | --- |
| Algılama | Dikey jeofon; düşük gürültülü analog ön uç; ADS1288 veya ADS1262 sınıfı yüksek çözünürlüklü ADC; üç eksenli ivmeölçer | Ham jeofon örneği, eğim ve salınım |
| Toplama | 500 SPS hedefli DMA blokları, monoton örnek sayacı, clipping ve eksik blok denetimi | Zaman sıralı örnek akışı |
| Olay motoru | 125 örneklik 0,25 s band sınırlı RMS; STA LTA, mutlak eşik ve histerezis; kurulum eğimi kuralı | Olay başlangıcı, sürmesi ve sonu |
| Zaman ve konum | Kurulumda çoklu GNSS; gerekirse 1PPS; ölçüm sonrası alıcının kapatılması | Konum, kurulum zamanı ve saat kalite durumu |
| Yerel kanıt | Kesintisiz halka tamponu, olay öncesi ve sonrası ham kayıt, cihaz ve firmware manifesti | Yeniden incelenebilir olay dosyası |
| Haberleşme | LoRaWAN Class A, OTAA ve bölgesel profil; olay özeti, sağlık ve yapılandırılabilir heartbeat | Düşük bant genişlikli telemetri |
| Güç ve muhafaza | LiFePO4, ölçümlü güç durumları, manyetik anahtar, kapalı servis teması ve saha tüpü | Düşük güç ve servis edilebilir kurulum |

### Temel veri akışı

| **Girdi** | **İşlem** | **Yerel kayıt** | **Radyo çıktısı** |
| --- | --- | --- | --- |
| Jeofon | AFE ve ADC ardından 500 SPS DMA | Olay çevresindeki ham dalga biçimi | RMS, eşik ve olay durumu |
| İvmeölçer | Eğim ve salınım özeti | Kurulum ve olay bağlamı | Eğim bayrağı ve salınım seviyesi |
| GNSS | Kurulum konumu ve zaman kalitesi | Konum ve saat metadata'sı | Kurulum ve sağlık özeti |

## 4 İlk ürün kapsamı

### MVP teslimleri

- Üç mühendislik örneği, bir LoRaWAN alıcı veya test ağı ve olay mesajı kod çözücüsü

- Tek dikey jeofon kanalı ve bir üç eksenli ivmeölçer

- Sürekli örnekleme, RMS ve STA LTA tabanlı olay durum makinesi

- Olay öncesi ve sonrası ham kayıt ile güç kesilmesinde veri bütünlüğü

- Kurulum GNSS kaydı, cihaz kimliği, firmware sürümü ve kalibrasyon dosyası

- Yerel yapılandırma ve saha sökmeden temel teşhis akışı

- Test matrisi, ham loglar, ölçüm özeti ve yeniden üretim manifesti

### MVP dışında

- Çok eksenli profesyonel sismoloji istasyonu

- Bulut analitik veya harita platformu

- Güneş paneli, hücresel modem veya mesh ağ

- Seri üretim kalıbı ve resmî çevresel sertifikasyon

- Otomatik deprem sınıflandırması veya makine öğrenmesi

## 5 Doğrulama planı

Aşağıdaki sayılar ölçülmüş sonuç değildir. K1 başlamadan önce ön kayıt altına alınacak prototip kabul hedefleridir; sensör, analog ön uç, anten ve enerji bütçesi ölçümleriyle değiştirilebilir.

| **Aşama** | **Test** | **Önerilen kabul hedefi** |
| --- | --- | --- |
| K1 bench | Örnekleme ve blok bütünlüğü | 500 SPS hedefinde 8 saat koşuda eksik veya yinelenen DMA bloğu 0 |
| K1 bench | RMS referans eşliği | 125 örneklik pencerede cihaz ve çevrimdışı referans RMS farkı yüzde 5 veya daha az |
| K1 bench | Olay fixture'ı | Önceden kaydedilmiş pozitif olayların tamamı bulunur; belirlenen negatif fixture'larda yanlış tetik 0 |
| K1 bench | Reset ve kayıt | Olay sırasında reset sonrası dosya ve manifest okunabilir; bozuk kayıt açıkça işaretlenir |
| K1 bench | Eğim kuralı | Kalibre jig ile 10 derece sınırının iki tarafı doğru sınıflanır; tolerans testten önce sabitlenir |
| K2 holdout | Bağımsız dalga biçimleri | Eğitimde kullanılmayan olay ve gürültü setinde duyarlılık ve yanlış alarm raporlanır |
| K2 holdout | Güç profili | Uyku, örnekleme, olay, GNSS ve LoRa akımları ölçülür; otonomi hesabı gerçek profile dayanır |
| K2 holdout | Radyo güvenilirliği | Tanımlı mesafe ve anten düzeninde mesaj teslimi, tekrar ve kayıp oranı raporlanır |
| K3 saha | Referans karşılaştırması | Aynı zemindeki kalibre referansla olay zamanı, band sınırlı RMS ve ham dalga biçimi farkı yayımlanır |
| K3 saha | Kurulum dayanımı | En az üç düğümlü pilotta su, sıcaklık, eğim, bağlantı ve enerji olayları kaydedilir |

## 6 On iki haftalık geliştirme planı

| **Hafta** | **Çıktı** | **Çıkış kapısı** |
| --- | --- | --- |
| 1 | Kullanım sınırı, frekans bandı, saha profili, lisans ve FTO kaydı | Sertifikalı sismograf iddiası yok; açık karar listesi |
| 2 | Jeofon AFE, ADC, dinamik aralık, gürültü ve enerji bütçesi | Sayısal model ve fixture sinyalleri |
| 3 | Geliştirme kartlarıyla analog ön uç ve örnekleme prototipi | 500 SPS blok akışı ve clipping denetimi |
| 4 | Rev A şema ve PCB; Ürün 1 release denetimi | ERC DRC üretim paketi ve kanıt manifesti |
| 5 | DMA, halka tamponu, ham olay dosyası ve reset dayanımı | Kayıp blok ve bozuk kayıt testleri |
| 6 | RMS, STA LTA, histerezis ve eğim durum makinesi | Sentetik fixture regresyonu |
| 7 | GNSS kurulum akışı, cihaz saati ve zaman kalite alanı | Tekrarlanabilir zaman kaydı |
| 8 | LoRaWAN OTAA, olay mesajları ve sağlık özeti | Kayıp ve tekrar telemetrisi |
| 9 | LiFePO4 güç profili, manyetik anahtar ve servis arabirimi | Ölçülmüş akım durumu tablosu |
| 10 | Kapalı muhafaza prototipi ve kurulum fikstürü | Sızdırmazlık hedefi ve mekanik risk raporu |
| 11 | K1 bench matrisi ve bağımsız K2 dalga biçimi koşusu | Ön kayıtlı test sonuçları ve ham kanıt |
| 12 | Üç düğümlü ücretli saha pilotu SOW'u ve devir paketi | Müşteri sahası ve kalibre referans erişimi |

## 7 Ticari pilot

İlk satış biçimi cihaz kataloğu veya SaaS aboneliği değil, üç düğümlü ücretli mühendislik pilotudur. Müşteri saha türünü, beklenen olay bandını, LoRaWAN erişimini ve referans ölçüm düzenini sağlar. Teslim; düğümler, kurulum prosedürü, kod çözücü, ham olay dosyaları, enerji ve link bütçesi, kabul raporu ve açık sınırlamalar listesidir.

### Doğrulanacak ticari varsayımlar

- Müşteri sürekli ham veri yerine olay özeti ve yerel ham kaydı yeterli bulur

- Üç düğümlü pilotun kurulumu ve bakımı mevcut ekiple yönetilebilir

- Yerel LoRaWAN kapsaması veya müşterinin ağ erişimi kabul edilebilir

- Referans cihazla karşılaştırma için saha ve veri erişimi sağlanır

- Pilot sonrasında ikinci saha veya ek düğüm için ödeme isteği oluşur

Fiyat, tek bir ilan bütçesinden çıkarılmamalıdır. Rev A BOM'u, montaj, muhafaza, kalibrasyon, ağ ve saha süresi ölçülmeden satış fiyatı verilmemelidir. İlk görüşmede ücretli keşif ve bench prototipi, ikinci aşamada saha pilotu ayrı kapsamlandırılmalıdır.

## 8 Riskler ve durdurma kapıları

| **Risk** | **Kontrol** | **Durdurma veya yeniden tasarım kapısı** |
| --- | --- | --- |
| Analog gürültü ve dinamik aralık | Jeofon, AFE ve ADC'yi sentetik giriş ve referans cihazla ölç | Hedef olay bandında ayırt edilebilir sinyal yoksa sensör zincirini değiştir |
| Enerji bütçesi | Her güç durumunu akım ve süreyle ölç; GNSS ve radyo görev çevrimini kaydet | Hedef otonomi seçilen batarya hacmiyle sağlanmıyorsa kapsamı daralt |
| LoRaWAN kapsaması | Gerçek anten, muhafaza ve saha yerleşiminde link bütçesi | Sürekli gateway erişimi yoksa yerel logger veya başka uplink seç |
| Kurulum değişkenliği | Jeofon temas, yön, toprak ve muhafaza fixture'ını standartlaştır | Aynı olay farklı kurulumlarda karşılaştırılamıyorsa eşik iddiasını geri çek |
| Lisans | AnyShake AGPL ve ticari lisans yolu; tüm türev bileşenler için lisans matrisi | Ticari dağıtım hakkı kapanmadan kod kopyalama veya satış yok |
| Patent ve FTO | Omron ve ilişkili olay algılama patentleri için profesyonel FTO incelemesi | Engelleyici hak veya lisans gereği varsa mimariyi değiştir |
| Yanlış güven | Raporlarda tarama düğümü, kanıt düzeyi ve kalibrasyon durumu | Sertifikalı alarm veya hasar kararı isteyen müşteri kapsam dışı |
| Muhafaza ve saha güvenliği | Sızdırmazlık, yoğuşma, sıcaklık, batarya ve servis kontağı testleri | Çevresel test yoksa dış saha ürünü iddiası yok |

## 9 Go no go kararı

Go: lisans ve FTO yolu yazılı kapanır; seçilen jeofon ve analog ön uç K1 fixture'larında yeterli sinyal ayrımı gösterir; gerçek güç ölçümü hedef otonomiye izin verir; üç düğümlü saha ve kalibre referans erişimi olan ücretli pilot bulunursa Rev A ilerler.

No go veya pivot: sinyal zinciri hedef bandı ayıramaz, enerji bütçesi muhafaza ve batarya sınırını aşar, LoRaWAN kapsaması doğrulanamaz, kurulum değişkenliği eşik sonuçlarını baskılar ya da ticari lisans ve FTO kapanmazsa saha donanımı ürünü durdurulur. Çıktılar Sensor Validation Microbench ve PCB fixture'ı olarak korunur.

## 10 Kaynaklar

**A1:** [IMG 5058 düşük güçlü yer titreşim sensörü ilanı](https://drive.google.com/file/d/1qtbc7hIHwtG-OWbLg7WmWx5rNBNFBC3B/view) — Cihaz, sensör ve prototip kapsamı

**A2:** [IMG 5059 firmware ve LoRaWAN gereksinimleri](https://drive.google.com/file/d/1He5IOXeSJFJrVwTefchEUZ1qREg8_vcj/view) — 500 SPS, RMS, STA LTA, eğim ve olay akışı

**A3:** [25.11 Sensor Design AnyShake IBIS AMI ve ECAD](https://docs.google.com/document/d/1HAvsXbhbCUlsLZklsxx71rHHo443WDlwZxCgDMVbd0s/edit) — AnyShake, açık titreşim düğümü, AFE ve fixture kaynakları

**A4:** [25.11 Sensor Design Araştırma Eki](https://drive.google.com/file/d/16kosQanS7Xt_pAy8OR8hDEkiiesNizZI/view) — Sentetik titreşim ve veri kalite fixture'ları

**A5:** [AnyShake Explorer](https://github.com/anyshake/explorer) — Jeofon ADC IMU GNSS ve seri veri referansı

**A6:** [Omron sismik sensör patenti](https://drive.google.com/file/d/1CkzGYQe2wn_6rW65uVV2qaXWngCxEvFc/view) — Düşük güç çalışma modu ve FTO riski

**A7:** [26.11 KiCad Temelleri PCB Kontrolü ve Motor Sistemleri](https://docs.google.com/document/d/1yS-EWmp0yQTV8ED8w1vbIbd81HcfkD6y3yeH6N4zxpA/edit) — Ürün 1 kabul kapısı ve kaynak sınıflandırması

### Kaynak kullanım sınırı

İş ilanı tek müşterinin tarihli talebidir; pazar büyüklüğünü veya satış başarısını kanıtlamaz. Açık kaynak depolar ve patentler uygulanabilirlik, risk ve fixture tasarımı için referanstır; çalışan saha ürünü ya da kullanım hakkı sağlamaz. Bu rapordaki kabul değerleri önerilen ön kayıt hedefleridir.
