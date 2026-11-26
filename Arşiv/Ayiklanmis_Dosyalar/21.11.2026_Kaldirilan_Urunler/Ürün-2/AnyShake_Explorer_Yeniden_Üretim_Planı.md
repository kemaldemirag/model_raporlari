# AnyShake Explorer Yeniden Üretim Planı

Ürün 2  21 Kasım 2026 kapsamı  Yerel entegrasyon 24 Kasım 2026

Bu plan, AnyShake Explorer'ın sabit bir kaynak revizyonundan yazılım ve protokol doğrulamasıyla başlayıp fiziksel cihaz, metroloji ve Observer uçtan uca kanıtına ilerleyen yeniden üretimini tanımlar. İlk iddia yalnız aynı kaynağın derlenebilmesi ve mevcut DFU testlerinin geçmesidir. Bit eşit binary, fiziksel cihaz parity ve sismik performans parity ayrı kapılardır ve kanıtlanmadan birleştirilmez.

## Yönetici kararı

| **Karar** | **Uygulama** | **Neden** |
| --- | --- | --- |
| Kaynak kilidi | Explorer commit dde90ce53811f090137d9a962bde6d7408477aef | İncelenen kod ve release aynı kimliğe bağlanır |
| Başlangıç | Yazılım ve protokol MVP | Donanım maliyetinden önce derleme veri ve DFU riskini azaltır |
| ECAD | Ürün 1 K1 geçtikten sonra Explorer kartını holdout olarak denetle | PCB üretimi kanıt kapısına bağlanır |
| Fiziksel rota | Tek kart bring up sonra sadık E C111G | Tüm sistemi bir defada hata ayıklama riski azalır |
| Dağıtım | AGPL veya ticari lisans yolu G0'da seçilir | Proprietary ve OEM kullanım için lisans kararı ertelenmez |

## Sabit kaynak ve mimari

Explorer; KiCad donanımı, STM32F103 ve FreeRTOS tabanlı firmware, 4 KiB bootloader ve Go tabanlı DFU aracından oluşur. Cihazın seri verisini alan AnyShake Observer ayrı bir depodur ve veri arşivi, web dalga formu ile MiniSEED gibi dışa aktarımları sağlar. Explorer ve Observer ayrı kaynak kimlikleriyle sabitlenir.

| **Bileşen** | **Teknik rol** | **İlk kanıt** |
| --- | --- | --- |
| Hardware | KiCad şema PCB BOM CPL Gerber ve drill | Ürün 1 ECAD holdout raporu |
| Firmware | STM32F103 FreeRTOS ADS1262 IMU GNSS PPS ve seri paketleme | Sabit matris derlemesi ve yerleşim doğrulaması |
| Bootloader | 0x08000000 başlangıç ve 4 KiB sınır | Boyut vektör ve flash yerleşim kontrolü |
| DFU tool | Go istemcisi autobaud image ve protokol | Test race vet coverage fuzz ve HIL |
| Observer | Seri TCP alımı SQLite web ve veri dışa aktarımı | Sabit commit ile tekrar oynatma ve soak |

## Bağımlılık sırası

WIP bir yaklaşımı korunur. Ürün 1 K1 tamamlanırken Ürün 2'de yalnız kaynak ve ortam manifesti hazırlanır. Fiziksel kart siparişi, Ürün 1 K1 ve Explorer ECAD kapısı geçmeden verilmez.

1. Ürün 1 K1 sentetik fixture kapısını tamamla.

2. Explorer ve Observer kaynak kimliklerini, lisans yolunu ve araç zincirini sabitle.

3. Explorer donanımını Ürün 1 ile ilk açık kaynak holdout olarak denetle.

4. Firmware bootloader ve DFU için dijital parity ve deterministik derleme düzeltmelerini tamamla.

5. Tek kart elektriksel bring up ve DFU HIL çalıştır.

6. Metroloji ve Observer soak kanıtından sonra yeniden üretim iddiasını yayınla.

## Kapı modeli

| **Kapı** | **Amaç** | **Geçmeden yapılmayacak işlem** |
| --- | --- | --- |
| G0 | Kaynak lisans ve ortam kilidi | Proprietary karar veya release iddiası |
| G1 | Yazılım ve CI parity | Firmware davranış parity iddiası |
| G2 | Tekrar üretilebilir derleme | Bit eşit binary iddiası |
| G3 | ECAD release paketi | PCB siparişi |
| G4 | Elektriksel bring up ve DFU HIL | Saha kurulumu |
| G5 | Metroloji ve Observer uçtan uca | Sismik performans parity iddiası |
| G6 | Release ve tedarik zinciri | Kamuya dağıtım veya seri üretim |

## G0 Kaynak lisans ve ortam kilidi

- Explorer HEAD değerini dde90ce53811f090137d9a962bde6d7408477aef ve ağaç kimliğini fa852ba3bba512ae445f75aa5b05906563281396 olarak doğrula.

- Observer için seçilen commit veya release'i ayrı manifest satırında sabitle. latest etiketi kullanılmaz.

- AGPL kullanım mı ticari lisans mı seçileceğini ürün dağıtım modeline göre yazılı karara bağla. Bu plan hukuki görüş yerine geçmez.

- KiCad 10 exact patch, PlatformIO, ststm32 platformu, GCC Arm toolchain, Go, Node, Python, işletim sistemi ve container digestlerini kaydet.

- Kaynak, alt modül, araç ve girdi dosyalarının SHA 256 değerlerini manifestte bağla.

G0 kabulü, temiz çalışma ağacının doğrulanması, lisans yolunun onaylanması ve tüm zorunlu sürüm veya digest alanlarının dolu olmasıdır. Açık alanlar varken kapı geçmez.

## G1 Yazılım ve CI parity

Sabitlenmiş upstream CI iki donanım revizyonu, dört GNSS seçeneği ve iki firmware yerleşimiyle 16 firmware varlığı; bir bootloader ve beş DFU aracı olmak üzere toplam 22 release varlığı üretir. İlk parity hedefi bu matrisin eksiksiz derlenmesi ve varlıkların isim, boyut ve yerleşim kurallarının doğrulanmasıdır.

- Go tarafında gofmt farkı, go vet, go test count bir race, coverage ve hedefli fuzz koşuları uygulanır.

- Mevcut protocol image client ve autobaud testleri korunur. serialport main ve UI dışı çalışma yolu için test boşlukları kapatılır.

- Firmware ve bootloader için host native birim testleri ile PlatformIO embedded test iskeleti eklenir. Paketleme GNSS PPS filtre EEPROM watchdog ve kuyruk davranışları önceliklidir.

- 22 varlığın yalnız var olması yetmez. Sıfır bayt kontrolüne ek olarak başlangıç adresi, vektör tablosu, maksimum boyut ve varyant meta verisi denetlenir.

## G2 Tekrar üretilebilir derleme

Mevcut kaynak bit bit yeniden üretilebilir değildir. firmware fw\_build.py UTC derleme zamanını binary içine yazar. firmware fw\_rev.py ve bootloader bl\_rev.py commit bilgisini git logs HEAD dosyasından çıkarır. PlatformIO yapılandırmasındaki ststm32 platformu sürümsüzdür. Bu sorunlar giderilmeden yalnız aynı kaynağın derlendiği söylenebilir.

| **Sorun** | **Düzeltme** | **Kabul** |
| --- | --- | --- |
| Derleme zamanı | SOURCE DATE EPOCH veya commit zamanı | İki temiz derlemede aynı alan |
| Commit kimliği | git rev parse HEAD ve açık fallback | Archive ve clone aynı kimliği üretir |
| Platform | ststm32 ve toolchain exact version veya digest | Çözümleme değişmez |
| Runner ve Actions | Container digest ve action tam commit SHA | Değişmez yürütme zinciri |
| Release overwrite | clobber kaldırılır ve tag değişmez | Yayınlanan varlık sonradan değişmez |

G2 kabulü, iki boş cache ve temiz ortamda üretilen 22 varlığın SHA 256 değerlerinin birebir eşleşmesidir. Upstream release digestleri ayrı bir karşılaştırmadır. Mevcut timestamp davranışı düzeltilmeden upstream binary eşitliği beklenmez.

## G3 ECAD release paketi

Explorer kartı Ürün 1 PCB Release Gate ile denetlenir. PCB ve şema dosyalarının generator kaydı KiCad 10 ile uyumlu ele alınır; exact patch sürümü manifestte sabitlenir. Upstream tarafından commit edilmiş Gerber ve BOM dosyaları, yeniden üretilen çıktılarla karşılaştırılır.

- Sıfır onaysız ERC veya DRC hatası ve gerekçeli warning ile exclusion baseline'ı.

- Şema PCB parity, reference designator setleri, DNP politikası ve üst alt taraf doğrulaması.

- Beklenen bakır mask paste silkscreen Edge Cuts PTH ve NPTH dosyalarının eksiksizliği.

- BOM position ve üretici parça kodlarının izlenebilirliği.

- Koşu öncesi ve sonrası KiCad kaynak hashlerinin aynı olması.

## G4 Elektriksel bring up ve DFU HIL

İlk kart akım sınırlı 9 ile 12 V besleme ve ST Link ile standalone firmware üzerinden açılır. Besleme rayları, sıcaklık, reset, SWD, OLED, EEPROM, IMU, ADS1262, GNSS PPS ve seri arayüzler sırayla doğrulanır. Bu aşamada önceden yazılmış voltaj, akım ve sıcaklık sınırları kullanılır.

| **HIL alanı** | **Test** | **Kabul kanıtı** |
| --- | --- | --- |
| Bellek yerleşimi | Bootloader 0x08000000 uygulama 0x08001000 | Map file vektör ve flash readback |
| Image güvenliği | Standalone image reddi yanlış SP PC offset ve CRC | Yazma başlamadan red veya güvenli recovery |
| Check modu | dfu tool check | Flash sayfalarında değişiklik yok |
| Chunk sınırı | En fazla 1024 bayt ve her sınırda kesinti | Geçersiz uygulama boot etmez |
| Recovery | Güç ve seri kesintisi ile no enter | Cihaz tekrar DFU'ya alınabilir |
| Fiziksel yol | RS 232 ve RS 485 dört baud | 57600 115200 230400 460800 ham log |
| Protokol hata | Bozuk CRC yarım frame çöp bayt tekrar ACK kaybı | Beklenen retry red ve kayıt |

DFU bütünlüğü CRC 32 MPEG 2 ile doğrulanır. Canlı sensör paketindeki tek bayt XOR kontrolü aynı koruma düzeyi sayılmaz ve G5 hata enjeksiyonunda ayrıca değerlendirilir.

## G5 Metroloji ve Observer

Ölçüm eşikleri sonuç görüldükten sonra belirlenmez. Ürün sahibi ve test sorumlusu kullanım senaryosuna göre kabul aralıklarını deneyden önce matrise yazar. 32 bit ADC ifadesi kullanılabilir çözünürlük kanıtı değildir.

| **Ölçüm** | **Yöntem** | **Zorunlu çıktı** |
| --- | --- | --- |
| Sıfır giriş gürültüsü | Kısa devre veya uygun terminasyon ile PSD | Ham kayıt PSD eğrisi RMS ve bant |
| Offset ve termal drift | Kararlı girişte sıcaklık süpürmesi | Offset sıcaklık ve zaman serisi |
| Kazanç ve faz | Üç kanala izlenebilir sinyal enjeksiyonu | Kanal bazlı genlik ve faz hatası |
| Crosstalk | Tek kanal uyarımı diğer kanalları gözlemleme | Frekans bazlı crosstalk oranı |
| Frekans yanıtı | 0.5 ile 27 Hz sweep | Transfer fonksiyonu ve tolerans karşılaştırması |
| Örnek hızları | 50 100 200 250 SPS | Paket sayısı zaman tabanı ve kayıp oranı |
| GNSS PPS | Referans zaman ölçerle timestamp hatası | Hata dağılımı ve holdover davranışı |
| Güç | 9 10.5 ve 12 V noktaları | Akım güç sıcaklık ve kararlılık |

### Observer uçtan uca

- Observer commit veya image digest G0'da sabitlenir. İlk arşiv SQLite ve Explorer seri protokol v3 ile kurulur.

- Altın seri kayıt 50 100 200 ve 250 SPS hızlarında yeniden oynatılır. Bozuk XOR, kayıp paket, zaman boşluğu, yeniden bağlanma ve bozuk çerçeve senaryoları uygulanır.

- Web dalga formu, arşiv kayıt sayısı ve MiniSEED dışa aktarımı aynı kaynak aralığına bağlanır.

- MiniSEED bağımsız bir okuyucuyla açılır ve örnek sayısı, kanal kimliği, başlangıç zamanı ve süre karşılaştırılır.

### Observer dayanıklılık ve toparlanma

- 24 ile 72 saat soak sırasında açıklanamayan boşluk, geriye giden timestamp veya sessiz veri kaybı kabul edilmez. Hizmet ve seri bağlantı kesintisinden sonra otomatik toparlanma doğrulanır.

## G6 Release ve tedarik zinciri

- Korunan ana dal, zorunlu pull request incelemesi ve tüm required check sonuçları.

- Actions bağımlılıklarının major etiket yerine tam commit SHA ile sabitlenmesi.

- İmzalı tag, SHA256SUMS, SBOM, lisans manifesti ve provenance attestation.

- Release varlıklarının source commit, toolchain, runner veya container digest ve kabul kayıtlarıyla tek manifestte bağlanması.

- Yayın sonrasında clobber veya aynı etiketi sessizce değiştiren iş akışının kapatılması.

## Kanıt ve dosya düzeni

| **Klasör** | **İçerik** |
| --- | --- |
| 00 manifest | Kaynak lisans araç zinciri digest ve onaylar |
| 01 build | CI logları 22 varlık map file boyut ve hashler |
| 02 ecad | Ürün 1 raporu ERC DRC Gerber BOM CPL ve kaynak hashleri |
| 03 hil | Güç rayları arayüz DFU hata enjeksiyonu ve ham loglar |
| 04 metrology | Ham dalga kayıtları PSD sweep drift gain phase crosstalk PPS ve güç |
| 05 observer | Config replay soak veri boşluğu ve MiniSEED doğrulaması |
| 06 release | SBOM lisans checksum imza attestation ve release manifesti |

## Riskler ve durdurma kuralları

| **Risk** | **Erken kontrol** | **Durdurma kuralı** |
| --- | --- | --- |
| Lisans yolu belirsiz | G0 ticari veya AGPL kararı | Dağıtım ve OEM tasarımı durur |
| ECAD kaynak ve committed fab çıktısı ayrışıyor | Ürün 1 yeniden üretim karşılaştırması | PCB siparişi durur |
| Firmware iki temiz ortamda farklı | G2 normalize ve binary hash | Bit eşitlik iddiası durur |
| DFU kesintide recovery yapmıyor | Her chunk sınırında güç ve seri kesintisi | Saha kurulumu durur |
| Metroloji eşiği sonradan yazılıyor | Ön kayıtlı threshold ve onay | G5 sonucu geçersiz sayılır |
| Observer sessiz veri kaybediyor | Bağımsız örnek sayımı ve timestamp kontrolü | Parity ve saha iddiası durur |
| Tek kart sonucu seri üretim sanılıyor | EVT DVT PVT ve lot izlenebilirliği ayrı kapı | Seri üretim kararı durur |

## Başarı iddiaları

| **Geçilen kapı** | **İzin verilen ifade** | **İzin verilmeyen ifade** |
| --- | --- | --- |
| G1 | Sabit kaynak derleniyor ve DFU yazılım testleri geçiyor | Cihaz yeniden üretildi |
| G2 | 22 varlık iki temiz ortamda aynı hashle üretildi | Fiziksel parity sağlandı |
| G3 | Üretim paketi tanımlı ECAD kapısını geçti | Kart elektriksel olarak çalışır |
| G4 | Tek kart bring up ve DFU recovery senaryoları geçti | Sismik ölçüm performansı eşdeğerdir |
| G5 | Ön kayıtlı metroloji ve Observer eşikleri sağlandı | Sertifikalı ürün veya seri üretim hazır |
| G6 | Release provenance ve tedarik zinciri kanıtı yayımlandı | Her kullanım alanında uygunluk |

## Yürütme planı

7. G0 manifestini doldur ve lisans kararını onayla.

8. G1 matrisini temiz ortamda çalıştır ve 22 varlık envanterini bağla.

9. G2 düzeltmelerini yapmadan bit eşitlik hedefi koyma. İki temiz build ile doğrula.

10. Ürün 1 K1 sonrası G3 holdout ECAD denetimini tamamla.

11. Tek kartı akım sınırlı bring up ile doğrula ve G4 DFU HIL senaryolarını çalıştır.

12. Ön kayıtlı eşiklerle G5 metroloji ve Observer soak çalıştır.

13. G6 release kanıtı tamamlanınca sadık yeniden üretim sonucunu yayımla.

14. Ticari türev ancak yeniden üretim kanıtı ve ayrı müşteri gereksinimiyle yeni kapsam olarak açılır.

## Kaynaklar

Bağlantılar, iş akışını tanımlarken kullanılan birincil belgeleri ve sabitlenmiş kaynak revizyonlarını gösterir. Erişim tarihi 24 Kasım 2026'dır.

**U2 1** [AnyShake Explorer sabit kaynak ağacı](https://github.com/anyshake/explorer/tree/dde90ce53811f090137d9a962bde6d7408477aef)

**U2 2** [Explorer ticari lisans metni](https://github.com/anyshake/explorer/blob/master/COMMERCIAL_LICENSE.txt)

**U2 3** [Explorer sabit CI iş akışı](https://github.com/anyshake/explorer/blob/dde90ce53811f090137d9a962bde6d7408477aef/.github/workflows/build.yml)

**U2 4** [Explorer DFU iç yapısı](https://github.com/anyshake/explorer/blob/dde90ce53811f090137d9a962bde6d7408477aef/docs/dfu-internals.md)

**U2 5** [Explorer ST Link programlama rehberi](https://github.com/anyshake/explorer/blob/dde90ce53811f090137d9a962bde6d7408477aef/docs/flashing-with-st-link.md)

**U2 6** [PlatformIO birim test dokümantasyonu](https://docs.platformio.org/en/stable/advanced/unit-testing/index.html)

**U2 7** [AnyShake Observer kaynak deposu](https://github.com/anyshake/observer)

**U2 8** [GitHub Actions güvenli kullanım](https://docs.github.com/en/actions/reference/security/secure-use)

**U2 9** [GitHub artifact attestations](https://docs.github.com/en/actions/concepts/security/artifact-attestations)

**U2 10** [KiCad 10 komut satırı arayüzü](https://docs.kicad.org/10.0/en/cli/cli.html)
