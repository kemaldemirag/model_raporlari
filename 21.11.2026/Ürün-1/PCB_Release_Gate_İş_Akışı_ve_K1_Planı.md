# Ürün 1 PCB Release Gate İş Akışı ve K1 Planı

_21 Kasım 2026 kapsamı. Yerel entegrasyon: 24 Kasım 2026._

Ürün 1, tek bir KiCad kart revizyonunun üretim paketi üzerinde salt okunur ve tekrar çalıştırılabilir bir release denetimi uygular. İlk karar kapısı K1'dir. K1 geçmeden ürünün üretime hazır kart sertifikası, müşteri kabulü veya ticari doğrulama sağladığı ileri sürülmez. FabGate denetim motorunu, TraceWing ise kaynak, ortam, komut, ham çıktı ve hash zincirini bağlayan kanıt katmanını ifade eder.

## Yönetici kararı

| **Karar alanı** | **Uygulama** | **Sınır** |
| --- | --- | --- |
| Aktif çalışma | Önce K1 sentetik fixture paketi | WIP bir olarak korunur |
| Ana araç | Sabit KiCad 10 patch sürümü ve kicad-cli | Araç sürümü manifestte yer alır |
| Opsiyonel adaptör | KiBot yalnız salt okunur ve sürümü sabitlenmiş biçimde | Kaynak değiştiren preflight adımları yasaktır |
| İlk harici holdout | K1 sonrası AnyShake Explorer kartı | Açık kaynak lisansı ve commit sabitlenir |
| İddia | Release paketi tutarlılığı ve yeniden çalıştırılabilir denetim | Elektriksel işlev ve sertifikasyon kapsam dışıdır |

## Ürün tanımı ve hedef kullanıcı

Hedef kullanıcı; üretim siparişi öncesinde KiCad kaynakları, BOM, CPL, Gerber, drill, DNP ve varyant dosyaları arasında gözden kaçan tutarsızlıkları bulmak isteyen donanım girişimi, PCB tasarımcısı, küçük elektronik üreticisi veya bağımsız mühendistir. Ürün, kaynak dosyayı değiştirmeden risk dereceli bulgu, yeniden çalıştırma komutu ve kanıt manifesti verir.

- FabGate, KiCad CLI çıktılarını, üretici profilini ve çapraz dosya kurallarını çalıştıran iç motordur.

- TraceWing, girdi hashleri, araç sürümleri, komutlar, ham loglar, normalize bulgular, çıktılar ve karar sınıfını evidence-manifest.json içinde bağlar.

- Müşteriye sunulan ürün bu iki bileşenin birlikte ürettiği PCB Release Package Review paketidir.

## Kapsam

| **Kapsama dahil** | **Kapsam dışında** |
| --- | --- |
| Tek kart, tek revizyon, tek üretici profili | Çok kartlı sistem entegrasyonu veya seri üretim verimi |
| ERC ve DRC JSON çıktıları ile şema PCB parity | Elektriksel fonksiyon, güç bütünlüğü ve sinyal bütünlüğü |
| Gerber, PTH ve NPTH drill, BOM, CPL, DNP ve varyant kontrolü | RF, EMC, termal, şebeke ve güvenlik sertifikasyonu |
| Reference designator, MPN, taraf, birim, orijin ve rotasyon tutarlılığı | Üretici DFM onayı veya tedarikçi parça uygunluğu garantisi |
| Kaynak mutasyonu, iki temiz tekrar ve normalize sonuç eşitliği | Kartın fiziksel bring-up ve laboratuvar performansı |

## Girdi sözleşmesi

Koşu başlamadan önce kart kimliği, revizyon, hedef üretici, DNP ve varyant politikası, KiCad sürümü, bilinen istisnalar ve kabul eşiği kaydedilir. Kaynak klasörü salt okunur bağlanır veya kopyası hashlenerek çalışma alanına alınır. Eksik girdi, başarılı denetim gibi gösterilmez.

| **Girdi** | **Zorunlu kayıt** | **Eksikse işlem** |
| --- | --- | --- |
| KiCad proje şema ve PCB | Dosya listesi ve SHA 256 | Koşu engellenir |
| Üretici profili | BOM CPL birim rotasyon DNP kuralları | Genel profil seçimi açıkça raporlanır |
| Varyant ve DNP | Aktif varyant ve dışlama kaynağı | Varyant kontrolü çalıştırılmaz ve bulgu açılır |
| Waiver dosyası | Bulgu kimliği gerekçe sahibi son tarih | İstisna uygulanmaz |
| Araç ortamı | KiCad patch OS container digest locale | Yeniden üretilebilirlik kapısı geçmez |

## İş akışı

| **Aşama** | **Ana işlem** | **Üretilen kanıt** | **Çıkış koşulu** |
| --- | --- | --- | --- |
| 0 Kapsam | Kart revizyon üretici profil ve kapsam dışını onayla | scope.json ve politika özeti | Tek yorumlanabilir kapsam |
| 1 Kaynak kilidi | Dosya envanteri ile koşu öncesi SHA 256 al | input-manifest.json ve checksums.before | Kaynak kimliği sabit |
| 2 Statik denetim | ERC DRC parity Edge Cuts reference ve exclusion kontrolleri | erc.json drc.json ve ham loglar | İhlaller sınıflandırılmış |
| 3 Çıktı üretimi | Gerber drill BOM CPL ve gerekirse assembly PDF STEP IPC 2581 üret | out klasörü ve komut günlüğü | Beklenen dosya seti tam |
| 4 Semantik kontrol | BOM CPL DNP varyant MPN taraf birim orijin rotasyon ve katman setini karşılaştır | findings.json ve findings.csv | Her kural sonuç üretmiş |
| 5 Tekrar deneyi | Aynı sabit ortamda iki temiz koşu çalıştır | run A run B ve normalize hashler | Normalize sonuçlar özdeş |
| 6 Mutasyon denetimi | Koşu sonrası kaynak SHA 256 ile öncesini karşılaştır | checksums.after ve diff sonucu | Kaynakta sıfır değişiklik |
| 7 İnceleme | Bulguları önem derecesi waiver ve kapsamla gözden geçir | review.html ve review record | Bağımsız inceleyen kararı |
| 8 Paketleme | Manifest komut ve tüm kanıtı teslim paketine bağla | evidence-manifest.json reproduce.md checksums.sha256 | Tek komutla tekrar mümkün |

## K1 deney tasarımı

K1, sonuç görülmeden tanımlanan bir temiz fixture ve altı bağımsız kusurlu fixture ile yürütülür. Her kusur ayrı çalıştırılır. Kusurlar aynı fixture içinde üst üste bindirilmez; böylece yakalanan bulgu belirli bir kuralla eşleştirilebilir. K1 matrisi, bu raporla aynı klasördeki çalışma kitabında yürütme kaydı olarak tutulur.

| **Kimlik** | **Fixture** | **Beklenen davranış** | **Asgari kanıt** |
| --- | --- | --- | --- |
| K1 00 | Temiz negatif kontrol | Onaysız kritik veya hata olmadan PASS | ERC DRC logu çıktı listesi kaynak hashleri |
| K1 01 | Bakır açıklık ihlali | PCB DRC clearance bulgusu | DRC JSON katman koordinat ve HTML kaydı |
| K1 02 | Zorunlu güç pini veya pin tipi ERC hatası | Şematik ERC bulgusu | ERC JSON sayfa sembol ve HTML kaydı |
| K1 03 | Açık veya belirsiz Edge Cuts çevrimi | Kart sınırı bulgusu | İlgili segment ve geometri sonucu |
| K1 04 | Zorunlu Gerber veya NPTH drill eksik | Çıktı eksikliği bulgusu | Beklenen ve gerçek dosya seti farkı |
| K1 05 | BOM ile CPL reference seti uyuşmuyor | Eksik veya fazla reference bulgusu | BOM CPL küme farkı ve kaynak satırı |
| K1 06 | DNP parça CPL içinde yer alıyor | DNP politika ihlali | Varyant DNP kaydı ve CPL satırı |

### K1 yürütme protokolü

1. Sabit commit veya fixture sürümünü ve KiCad 10 patch sürümünü kaydet.

1. Her fixture için kaynak SHA 256 listesini koşudan önce üret ve salt okunur sınırı doğrula.

1. Temiz fixture ile iki bağımsız koşu yap. Çalışma ve cache klasörlerini her koşuda sıfırla.

1. Altı kusuru ayrı ayrı çalıştır. Her kusur için beklenen tekil bulgunun üretildiğini ve temiz kontrolün kirlenmediğini doğrula.

1. Run A ve Run B bulgularını değişken zaman damgası ve geçici yol gibi alanlar çıkarıldıktan sonra normalize et.

1. Normalize bulguların ve deterministik çıktıların SHA 256 değerlerini karşılaştır.

1. Koşu sonrası kaynak SHA 256 listesini koşu öncesi listeyle karşılaştır. Tek bir kaynak değişikliği bile kapıyı düşürür.

1. Yeni bir temiz çalışma alanında reproduce.md içindeki tek komutu çalıştır ve aynı kararı yeniden üret.

1. Bağımsız inceleyen, kanıt paketini matrise bağlar ve K1 kararını kaydeder.

## K1 kabul kararı

| **Ölçüt** | **Geçiş şartı** | **Kaldırma koşulu** |
| --- | --- | --- |
| Temiz fixture | Bir temiz PASS | Onaysız kritik veya hata |
| Kusur yakalama | Altı ayrı kusurda altı doğru yakalama | Kaçan kusur veya yanlış kurala bağlama |
| Yanlış pozitif | Temiz kontrol beklenmeyen bulgu üretmez | Temiz fixture gereksiz bloklanır |
| Kaynak bütünlüğü | Koşu öncesi ve sonrası kaynak hashleri aynı | Herhangi bir kaynak dosya değişir |
| Tekrar | İki koşunun normalize sonuçları özdeş | Karar bulgu kimliği veya deterministik çıktı farkı |
| Yeniden çalıştırma | Temiz ortamda tek komut aynı kararı üretir | Gizli manuel adım veya sabitlenmemiş bağımlılık |
| Kanıt paketi | Zorunlu dosyalar eksiksiz ve manifestte bağlı | Ham log komut sürüm veya hash eksik |

## Kanıt paketi

Teslim paketi hem insan incelemesine hem otomatik yeniden çalıştırmaya hizmet eder. Manifestte bulunmayan dosya karar kanıtı sayılmaz. Ham ve normalize çıktılar ayrı tutulur.

| **Dosya veya klasör** | **İçerik** | **Zorunlu** |
| --- | --- | --- |
| review.html | İnsan tarafından okunabilir sonuç ve kapsam | Evet |
| findings.json ve findings.csv | Kararlı bulgu kimliği önem kural kaynak konumu | Evet |
| evidence-manifest.json | Kaynak ortam komut senaryo artefakt hash ve karar bağları | Evet |
| checksums.sha256 | Teslim paketinin dosya özetleri | Evet |
| raw logs | stdout stderr exit code ERC DRC ve araç günlükleri | Evet |
| outputs | Gerber drill BOM CPL ve seçilen ek çıktılar | Evet |
| reproduce.md | Tek komut ön koşullar ve beklenen sonuç | Evet |
| waivers.yml | Bulgu kimliği gerekçe sahibi onay ve son tarih | Varsa |

## Araç ve bağımlılık politikası

- KiCad CLI tek doğruluk kaynağıdır. Exact patch sürümü ve container digest kaydedilir.

- KiBot, yalnız gerekli bir üretim adaptörü olarak ve sürümü sabitlenmiş biçimde kullanılabilir. Kaynak dosyayı değiştiren adımlar kapalı tutulur.

- CopperForge ve benzeri hizmetler çapraz kontrol veya rakip karşılaştırmasıdır. Ürün kararı bunların çıktısına bağımlı olmaz.

- İlk sürüm yerel veya izole ortamda çalışır. CI ikinci uygulama yüzeyidir. En az üç ücretli ve kabul edilmiş pilot görülmeden barındırılan SaaS kapsamı açılmaz.

## Waiver ve bulgu yönetimi

Waiver, bulguyu silmez. Bulguyla aynı kararlı kimliği taşır ve gerekçe, kapsam, sahibi, onaylayan, son tarih ile etkilenen revizyonu kaydeder. Süresi geçmiş, farklı revizyona ait veya sahibi olmayan waiver uygulanmaz. Kritik bulgular için otomatik waiver yasaktır.

## Riskler ve kontroller

| **Risk** | **Kontrol** | **Kalan sınır** |
| --- | --- | --- |
| DRC geçişinin üretim onayı gibi sunulması | Rapor başında iddia sınırı ve kapsam dışı listesi | Fiziksel üretici ve laboratuvar doğrulaması gerekir |
| Araç kaynak dosyayı düzeltir veya yeniden yazar | Salt okunur mount ve koşu öncesi sonrası hash | İşletim sistemi düzeyinde izin ayrıca doğrulanır |
| İki koşu farklı ama karar aynı görünür | Normalize bulgu ve deterministik çıktı hash karşılaştırması | Tarih damgalı ham loglar doğal olarak farklı olabilir |
| Üretici rotasyon kuralları yanlış modellenir | Profil sürümü ve gerçek örnekle profil kabul testi | Üretici belgesi değiştiğinde profil yeniden onaylanır |
| Waiver kalıcı borca dönüşür | Sahip son tarih ve revizyon bağı | Yenileme bağımsız onay ister |

## Ürün 2 ile entegrasyon

K1 geçtikten sonra AnyShake Explorer kartı ilk açık kaynak holdout vakası olarak Ürün 1 kapısından geçirilir. Bu çalışma, Ürün 2'nin ECAD kapısıdır. Ürün 1 başarısızsa Ürün 2 için fiziksel kart üretimine geçilmez. Explorer üzerinde bulunan bulgular yeni fixture adayına dönüşebilir; ancak holdout sonucu sonradan K1 fixture performansını yükseltmek için kullanılmaz.

## Sıralı uygulama planı

1. K1 fixture ve kabul matrisini dondur.

1. KiCad 10 patch sürümü ile izole koşu ortamını oluştur.

1. Temiz kontrolü ve altı ayrı kusuru iki tekrar halinde yürüt.

1. Kanıt paketini bağımsız incelemeye ver ve K1 kararını kaydet.

1. K1 geçerse AnyShake Explorer kartını holdout olarak denetle.

1. Holdout sonrası üç ücretli pilot için dar kapsamlı hizmet paketi ve fiyat hipotezini test et.

## Kaynaklar

Bağlantılar, iş akışını tanımlarken kullanılan birincil belgeleri ve sabitlenmiş kaynak revizyonlarını gösterir. Erişim tarihi 24 Kasım 2026'dır.

- **U1-1:** [KiCad 10 komut satırı arayüzü](https://docs.kicad.org/10.0/en/cli/cli.html)

- **U1-2:** [KiBot kaynak deposu](https://github.com/INTI-CMNB/KiBot)

- **U1-3:** [CopperForge dokümantasyonu](https://docs.copperforge.dev/)

- **U1-4:** [Araştırma Kayıtları güncel sentez](https://docs.google.com/document/d/1dcz3_32Uf0tsr9-vKUdqMf60GC-qXrS3NpROj8Uhu4s/edit)

- **U1-5:** [Referans Listeler PCB ve doğrulama kaynakları](https://docs.google.com/document/d/1oAJTYGt4XvYokdU1XCrVnFKb6zCa2ptOWQjd1sC2n5s)

- **U1-6:** [AnyShake Explorer sabit kaynak ağacı](https://github.com/anyshake/explorer/tree/dde90ce53811f090137d9a962bde6d7408477aef)
