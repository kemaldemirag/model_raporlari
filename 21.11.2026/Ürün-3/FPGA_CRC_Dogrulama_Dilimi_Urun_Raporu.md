**ÜRÜN 3 · 26 KASIM 2026**

# FPGA CRC Doğrulama Dilimi Ürün Raporu

Dar kapsamlı RTL modülleri için yeniden çalıştırılabilir simülasyon ve kanıt paketi

**Ürün kararı**

**Tek bir CRC veya akış tabanlı RTL modülünün arayüz sözleşmesini sabitleyen; bağımsız yazılım referansı, cocotb sürücüleri ve hata enjeksiyonuyla reset, backpressure, veri bütünlüğü ve tekrar edilebilirliği doğrulayan; bulguları ham log, seed, sürüm ve hash ile teslim eden yerel doğrulama hizmeti ve araç dilimi.**

**Birincil kullanıcı:** Küçük FPGA ekipleri, danışmanlar ve RTL teslimi alan donanım girişimleri

**İlk teslim:** Tek modül ve tek arayüz için sabit kapsamlı ücretli doğrulama dilimi

**Kanıt düzeyi:** Ürün hipotezi — Simülasyon başarısı donanım timing closure değildir

**İç kabul kapısı:** Ürün 1 kanıt manifesti ve salt okunur kaynak denetimi

**Drive işlemi:** Yerel çalışma — Drive'a yazılmadı

## 1 Yönetici kararı

Kalan adaylar içinde donanımsız ilk doğrulamaya en hızlı girebilen fırsat FPGA CRC Doğrulama Dilimidir. Referans kayıtları cocotb tabanlı HDL testbench yaklaşımını, CRC vektörlerini, bit değişimi, FIFO, reset ve backpressure senaryolarını doğrudan önerir. İş ilanı kayıtları ise UART alıcısı, FIFO, kontrol durum makinesi, CDC, reset ve XDC incelemesi için ücretli talep bulunduğunu gösterir.

İlk sürüm genel amaçlı FPGA doğrulama platformu değildir. Bir müşteri modülünü tanımlı bir arayüz ve CRC sözleşmesi altında çalıştırır; bağımsız referans modelle karşılaştırır; pozitif, negatif ve hata enjeksiyon testlerini yürütür; aynı girdide aynı kararın yeniden üretilebildiğini kanıtlar. Böylece ürün, geniş bir RTL projesi yerine satın alınabilir ve kabul edilebilir küçük bir doğrulama birimi sunar.

Ürün 3, Ürün 1 veya Ürün 2'nin yeni adı değildir. Ürün 1'deki manifest ve kaynak bütünlüğü yaklaşımını kullanabilir; ancak müşteri problemi RTL davranışının simülasyonda sınanmasıdır. Ürün 2 ise fiziksel titreşim algılama ve saha haberleşmesi ürünüdür.

### Kararı destekleyen kayıtlar

| **Kayıt** | **Doğrudan bulgu** | **Ürün etkisi** |
| --- | --- | --- |
| 24 Kasım Referans Listeler | cocotb; CRC vektörleri; bit değişimi; FIFO; reset; backpressure; Verilator veya benzeri simülasyon raporu | Donanımsız K1 ve araç mimarisi |
| CRC FPGA makalesi | Paralel CRC hesaplama ve Spartan 3 üzerinde kaynak tarafından bildirilen performans | Algoritma ve fixture girdisi; ürün performans kanıtı değil |
| RTL inceleme ilanı | UART receiver, FIFO, kontrol FSM, CDC, reset ve XDC; 20 ile 55 dolar saatlik aralık | Ücretli uzmanlık talebi; tek ilan pazar kanıtı değil |
| ECP5 ilanı | Şema ve PCB denetimi için 250 dolar sabit bütçe | FPGA ekiplerinin dar inceleme satın alabildiğine ek sinyal |
| Taxi ve verilog ethernet | Akış, AXI, Ethernet ve testbench referansları; lisans ve bakım durumu farklı | Fixture kaynağı seçerken commit ve lisans kapısı |

## 2 Müşteri ve kullanım sınırı

### Birincil müşteri

- Tek bir CRC, UART, FIFO veya basit akış modülü teslim almadan önce bağımsız simülasyon isteyen küçük FPGA ekipleri

- Tam zamanlı doğrulama mühendisi olmayan donanım girişimleri ve ürün geliştirme danışmanları

- Müşteriye RTL ile birlikte yeniden çalıştırılabilir test ve kanıt paketi teslim etmek isteyen serbest mühendisler

### Kullanıcının tamamlamak istediği iş

RTL modülünün yalnız birkaç örnekte doğru görünmesini değil; belirtilen CRC parametreleri, paket uzunlukları, reset zamanları ve akış duraklamaları altında referans modelle aynı sonucu verdiğini görmek, başarısız senaryoyu seed ve dalga biçimiyle tekrar çalıştırmak ve teslim kararını izlenebilir kanıta bağlamak.

### Kapsam dışı iddialar

- FPGA üzerinde timing closure, güç, sıcaklık veya hat hızı garantisi

- CDC veya reset güvenliğinin yalnız davranış simülasyonuyla ispatlandığı iddiası

- Formal verification, güvenlik sertifikasyonu veya kapsamlı UVM ortamının yerine geçme

- Belirsiz veya eksik CRC sözleşmesini test yazarının sessizce tahmin etmesi

- Müşteri RTL'sinin izinsiz saklanması, portfolyoya alınması veya üçüncü taraf servise gönderilmesi

## 3 Ürün mimarisi

| **Katman** | **Bileşen ve işlev** | **Çıktı** |
| --- | --- | --- |
| Sözleşme | CRC width, polynomial, init, refin, refout, xorout, veri sırası; clock, reset, handshake ve latency alanları | Sürümlü modül sözleşmesi |
| Referans model | DUT'tan bağımsız yazılım CRC modeli ve ön kayıtlı golden vektörler | Beklenen CRC ve hata sınıfı |
| Sürücü ve izleyici | cocotb ile frame üretimi, valid ready sürüşü, çıktı izleme ve sıra numarası | Zaman damgalı giriş ve çıkış izi |
| Senaryo motoru | Uzunluk sınırları, parçalı trafik, random backpressure, reset enjeksiyonu, bit değişimi ve hatalı çerçeve | Tekrarlanabilir test koşuları |
| Karşılaştırıcı | Golden model, scoreboard, kayıp tekrar sıra bozulması ve latency kontrolleri | Makinece okunur PASS FAIL |
| Statik inceleme izi | Reset, CDC, FIFO, durum makinesi ve XDC kontrol listesi; kullanılan araç ve kanıt sınıfı ayrı | Simülasyon dışı risk bulguları |
| Kanıt paketi | Commit SHA, simulator ve paket sürümü, komut, seed, stdout stderr, JUnit, dalga biçimi, bulgu JSON ve hash | Bağımsız yeniden koşu paketi |

### İşleme akışı

| **Girdi** | **İşlem** | **Karar** | **Teslim** |
| --- | --- | --- | --- |
| RTL ve sözleşme | Arayüz ve CRC parametrelerini doğrula | Eksik alan varsa BLOCK | Sözleşme manifesti |
| Golden ve random frame | DUT ile bağımsız modeli çevrim çevrim karşılaştır | İlk farklılıkta bulgu | Seed, frame ve dalga biçimi |
| Reset ve backpressure | Her işlem evresinde kesme, duraklatma ve yeniden başlatma | Kayıp tekrar sıra veya stale output | Senaryo matrisi ve log |
| İki temiz tekrar | Aynı commit, ortam ve seed ile yeniden koş | Normalize sonuç eşitliği | Hash manifesti ve kabul özeti |

## 4 İlk ürün kapsamı

### MVP teslimleri

- Tek RTL modülü, tek clock domain ve tek streaming arayüz için sabit kapsam

- Bir CRC profili ve en fazla üç seçili alternatif profil için bağımsız referans model

- cocotb sürücü, monitor, scoreboard ve deterministik seed yönetimi

- Golden vektör, sınır uzunlukları, random frame, bit değişimi, reset ve backpressure fixture'ları

- Verilator ana simülatörü; uyumlu tasarımda ikinci simülatörle sonuç eşliği kontrolü

- JUnit veya JSON sonuçları, VCD ya da FST dalga biçimi, ham log, bulgu raporu ve evidence manifest

- Bir düzeltme sonrası yeniden koşu ve müşteri ortamında çalıştırma yönergesi

### MVP dışında

- Çok clock domain içeren geniş SoC veya tam Ethernet yığını

- Formal property yazımı, UVM ortamı veya emülasyon çiftliği

- Place and route, board bring up, SERDES sinyal bütünlüğü veya fiziksel performans

- Kaynağı belirsiz ticari IP'nin test fixture'ına kopyalanması

- Otomatik hata düzeltme veya müşterinin RTL mimarisini baştan tasarlama

## 5 Doğrulama planı

Aşağıdaki sayılar ölçülmüş ürün sonucu değildir. K1 başlamadan önce sabitlenecek önerilen kabul hedefleridir. Her müşteri modülünde CRC sözleşmesi, clock ve reset davranışı ile desteklenen simülatörler ayrıca onaylanır.

| **Aşama** | **Test** | **Önerilen kabul hedefi** |
| --- | --- | --- |
| K1 sözleşme | CRC ve arayüz alanları | Zorunlu alanların tamamı manifestte; belirsiz alan varsa koşu başlamadan BLOCK |
| K1 golden | Ön kayıtlı vektörler | Her vektörde DUT ve bağımsız model çıktısı aynı; uyuşmazlık 0 |
| K1 sınır | Uzunluk ve veri desenleri | Sıfır uzunluk destekleniyorsa dahil; 1 byte, word sınırları, maksimum seçili uzunluk ve tüm sıfır bir desenleri geçer |
| K1 random | Deterministik çerçeveler | En az 100 ön kayıtlı seed koşusunda scoreboard uyuşmazlığı, kayıp veya tekrar 0 |
| K1 akış | Random backpressure | En az 10000 çevrimde sıra bozulması, veri kaybı ve çift çıktı 0 |
| K1 reset | İşlem öncesi ortası ve sonu reset | Tanımlı reset sözleşmesine aykırı stale output veya yarım frame kabulü 0 |
| K1 hata | Bit değişimi ve bozuk çerçeve | Alıcı davranışı golden oracle ile aynı; her farklılık frame ve seed ile yeniden üretilir |
| K1 mutasyon | Kasıtlı RTL hataları | Önceden seçilmiş her mutant en az bir test tarafından yakalanır |
| K1 tekrar | İki temiz ortam koşusu | Normalize bulgular, karar ve artefakt hash listesi eşdeğer |
| K2 holdout | Geliştirmede kullanılmayan core ve vektörler | Kilitli holdout sette beklenen pozitif ve negatif sonuçların tamamı doğru sınıflanır |
| K2 simülatör | Çapraz simülatör eşliği | Desteklenen iki simülatörde normalize verdict ve scoreboard sonucu aynı |
| K3 pilot | Müşteri modülü | Müşteri temiz ortamında tek komutla koşu ve kabul edilen bulgu paketi |

## 6 Sekiz haftalık geliştirme planı

| **Hafta** | **Çıktı** | **Çıkış kapısı** |
| --- | --- | --- |
| 1 | MVP sözleşmesi, IP ve lisans matrisi, örnek CRC profili | Parametre ve kapsam belirsizliği yok |
| 2 | Bağımsız golden model ve ön kayıtlı vektör deposu | Referans model için kendi birim testleri PASS |
| 3 | cocotb driver, monitor, scoreboard ve Verilator akışı | Temiz sentetik DUT temel testleri PASS |
| 4 | Uzunluk, veri deseni, bit değişimi ve random seed matrisi | Her fixture beklenen karar ve artefaktı üretir |
| 5 | Backpressure, reset ve FIFO wrapper senaryoları | Kasıtlı sıra kaybı ve stale output kusurları yakalanır |
| 6 | Mutasyon seti, ikinci simülatör kontrolü ve kanıt manifesti | İki temiz koşu eşdeğer ve kaynak mutasyonu 0 |
| 7 | Kilitli holdout core ve bağımsız inceleme | Test yazarının görmediği fixture üzerinde K2 kararı |
| 8 | Tek modüllü ücretli pilot SOW ve müşteri devir paketi | NDA IP saklama ve kabul koşulları yazılı |

## 7 Ticari pilot

İlk satış biçimi yazılım lisansı değil, tek modüllü sabit kapsamlı RTL doğrulama dilimidir. Müşteri RTL'i, arayüz ve CRC sözleşmesini, izin verilen simülatörü ve beklenen teslim ortamını sağlar. Teslim; testbench, fixture'lar, tek komutlu çalışma, ham loglar, dalga biçimleri, risk dereceli bulgu raporu ve yeniden koşu manifestidir. Bir düzeltme sonrası yeniden koşu fiyata ayrı satır olarak yazılır.

### Doğrulanacak ticari varsayımlar

- Müşteri geniş danışmanlık yerine tek modüllü ve sonuç ölçütlü inceleme satın alır

- Testbench ve raporun müşteri ortamında yeniden çalışması teslim değerini artırır

- NDA ve IP koşulları yerel veya müşterinin CI ortamında çalışmaya izin verir

- İlk teslimden sonra ikinci modül veya yeni revizyon için tekrar ödeme isteği oluşur

- Simülasyon kapsamı ile timing CDC ve hardware kapsamı müşteri tarafından kabul edilir

Kayıtlardaki 20 ile 55 dolar saatlik ilan aralığı fiyat listesi değildir; bir kez gözlenen talep sinyalidir. İlk teklif, giriş sözleşmesi tamamlandıktan sonra süre ve IP koşullarına göre sabit kapsamlandırılmalıdır. Ücretli keşif, doğrulama dilimi ve kapsam dışı timing veya CDC uzman incelemesi ayrı kalemler olmalıdır.

## 8 Riskler ve durdurma kapıları

| **Risk** | **Kontrol** | **Durdurma veya yeniden tasarım kapısı** |
| --- | --- | --- |
| Belirsiz CRC sözleşmesi | Tüm parametreleri ve byte bit sırasını yazılı giriş manifestine bağla | Müşteri sözleşmeyi onaylamazsa test yazma |
| Testbenchin DUT hatasını kopyalaması | Bağımsız model, golden vektör ve mutant fixture kullan | Aynı varsayımı paylaşan oracle varsa modeli değiştir |
| Dört durumlu mantık farkı | X ve Z davranışını araç bazında kaydet; ikinci simülatör koşusu | Simülatör farkı karar verdiriyorsa tek PASS iddiası yok |
| Reset ve CDC yanlış güveni | Davranış testi ile statik CDC reset incelemesini ayrı sınıflandır | Yalnız simülasyondan güvenli CDC sonucu çıkarma |
| Timing overclaim | XDC ve rapor dosyalarını statik bulgu olarak etiketle | Place route ve donanım ölçümü yoksa frekans garantisi yok |
| Lisans bulaşması | cocotb, Taxi, örnek core ve yardımcıların commit lisans matrisini tut | Uyumsuz lisanslı kod teslim paketine girerse dağıtımı durdur |
| Müşteri IP ve NDA | Yerel çalışma, erişim sınırı, saklama süresi ve silme kaydı | Yazılı hak yoksa kaynak saklama veya portfolyo kullanımı yok |
| Aşırı özelleştirme | Tek clock ve tek arayüz sınırı; adaptör işini ölç | Her pilot yeni framework gerektiriyorsa ürün değil danışmanlık olarak kal |

## 9 Go no go kararı

Go: bağımsız golden model ve sentetik DUT üzerinde K1 matrisi geçer; kasıtlı CRC, reset ve akış kusurlarının tamamı yakalanır; iki temiz koşu aynı normalize sonucu üretir; lisans ve müşteri IP yolu kapanır; sabit kapsamı kabul eden ücretli tek modüllü pilot bulunursa hizmet başlatılır.

No go veya pivot: müşteri modülleri ortak sözleşmeye indirgenemiyor, her iş yeni doğrulama altyapısı gerektiriyor, simülatör farkları kararlı karar üretmiyor ya da alıcı timing closure ve formal proof'u temel paketin içinde bekliyorsa ürünleştirme durdurulur. Çekirdek test ve manifest bileşenleri uzman RTL doğrulama hizmeti olarak korunur.

## 10 Kaynaklar

**A1:** [24 Kasım Referans Listeler PCB Makine Görüsü ROS 2 ve FPGA](https://docs.google.com/document/d/1oAJTYGt4XvYokdU1XCrVnFKb6zCa2ptOWQjd1sC2n5s/edit) — G16 G18 ve A01 teknik kaynakları

**A2:** [24 Kasım İş İlanları ECAD Robotik Makine Görüsü ve RTL](https://docs.google.com/document/d/1IpqQCuqCB5kc1xIjj2I4EGOealOTamDBHhD3CoeyJu8/edit) — J 2411 06 ve J 2411 08 talep kayıtları

**A3:** [24 Kasım Araştırma Kayıtları Güncel Sentezi](https://docs.google.com/document/d/1dcz3_32Uf0tsr9-vKUdqMf60GC-qXrS3NpROj8Uhu4s/edit) — FPGA CRC ürün eşlemesi ve kanıt sınırları

**A4:** [Detecting Errors in Digital Communications with CRC Codes implemented with FPGA](https://drive.google.com/file/d/1KlWWo7OY52sqEiS1lsH4yuqvWEx72nCq/view) — Paralel CRC araştırma kaydı

**A5:** [cocotb](https://github.com/cocotb/cocotb) — Python tabanlı HDL testbench referansı

**A6:** [Taxi FPGA components](https://github.com/fpganinja/taxi) — Akış ve haberleşme fixture kaynağı; lisans incelemesi gerekir

**A7:** [verilog ethernet](https://github.com/alexforencich/verilog-ethernet) — Bakım durumu nedeniyle tarihsel referans

**A8:** [FPGA RTL Code Review and Timing Constraint Verification](https://www.upwork.com/jobs/~022113447768160994103) — UART FIFO FSM CDC reset ve XDC talebi

**A9:** [Lattice ECP5 FPGA schematic and PCB assistance](https://www.upwork.com/jobs/~022123750563371023458) — Dar FPGA inceleme bütçe sinyali

### Kaynak kullanım sınırı

İş ilanları tarihli ve tekil talep örnekleridir; pazar büyüklüğü, açık iş durumu veya satış başarısı kanıtı değildir. Açık kaynak depolar uygulanabilirlik ve fixture tasarımı için referanstır; kod kullanım ve yeniden dağıtım hakkı her commit için ayrıca incelenir. Makaledeki hız değeri bu ürünün ölçülmüş sonucu değildir. Bu rapordaki kabul değerleri önerilen ön kayıt hedefleridir.
