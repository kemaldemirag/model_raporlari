# PF 01 PCB kontrol ve üretim teslimi

Codex görev planlama raporu

Kemal Demirağ için hazırlanmıştır. Muhatap: çalışma alanını kuracak ve seçili örneği geliştirecek Codex.

Bu plan yalnız gösterilebilir ilk örneği üretir. Görevler T00 ile T06 sırasında yürütülür; karşılanmayan bir önkoşul bağımlı görevi durdurur. Bağımsız işler devam edebilir. Bu belgedeki komutlar hedef arayüzdür; mevcut bir uygulamanın çalıştığı iddia edilmez.

**Repo** Portfolyo/29.10.2026/002-Proje Çıktıları/pcb-release-review-workbench/

**Ortam** Kartın dosya sürümüyle uyumlu KiCad ortamı. Windows kurulumu kullanılabilir; Linux veya WSL2 seçimi mevcut ortamın envanterine göre yapılır. İkinci bir EDA ortamı gereksiz yere kurulmaz.

**Başlama koşulu** Native şema ve PCB aynı revizyona ait olmalı; proje gerekli kütüphanelerle açılabilmeli. Bu doğrulanmadan üretim çıktısı görevine geçilmez.

## Az sayıda dosyayla kurulacak yapı

| **Konum** | **Görevi** |
| --- | --- |
| config/ | release.json ve üretim aracı ayarları |
| scripts/ | run\_demo.py ve paket doğrulayıcısı |
| tests/fixtures/ | Orijinali değiştirmeyen hatalı çıktı örnekleri |
| demo/output/ | Gerber, drill, BOM, konum dosyaları ve assembly.html |
| demo/ | review.html ve teslim ZIP dosyası |

Ortak dosyalar: README.md, sources.lock.json, docs/case-study.md ve küçük bir tests alanı. Büyük test videoları ve ayrıntılı ham kayıtlar repo dışında 003-Proje Kayıtları içinde tutulur. Release manifesti bu kayıtların konumunu ve hashlerini gösterir.

### PF 01 T00 Kaynak ve başlangıç durumu

**Bağımlılık** Yok. Girdi: Kaynak URL ve release\_contract.json.

**Yapılacak değişiklik** sources.lock.json ile docs/baseline.txt oluştur; kart yolları, commit, lisans ve araç sürümlerini kaydet.

**Kabul** Native dosya açılır, iki kaynak dosyanın aynı revizyona ait olduğu gösterilir.

**Sorun halinde** Erişim veya dosya biçimi belirsizse BLOCKED; başka kartı sessizce seçme.

### PF 01 T01 Teslim sözleşmesini bağla

**Bağımlılık** T00. Girdi: Ek sözleşme ve gerçek kart.

**Yapılacak değişiklik** config/release.json içine doğrulanmış yolları ve üretilecek dosya rollerini yaz; DNP ve THT politikasını belirt.

**Kabul** Zorunlu roller ve hariç parçalar makinece okunabilir; null kaynak alanları çözülmüş.

**Sorun halinde** Eksik varyant bilgisi yalnız ilgili teslim rolünü bloke eder.

## Çalışan örneğin üretilmesi

### PF 01 T02 Üretim işlemini kur

**Bağımlılık** T01. Girdi: Sabit kart ve araç ayarları.

**Yapılacak değişiklik** scripts/run\_demo.py ile KiBot veya doğrulanmış kicad-cli çağrılarını sırayla çalıştır; HTML BOM üret.

**Kabul** Temiz demo/output dizininde gereken çıktılar oluşur; her araç çıkış kodu kaydedilir.

**Sorun halinde** Araç başarısızlığına rağmen eski çıktıyı yeni teslim diye paketleme.

### PF 01 T03 Tutarlılık kontrollerini ekle

**Bağımlılık** T02. Girdi: Üretilen dosyalar ve sözleşme.

**Yapılacak değişiklik** Dosya varlığı, BOM referansları, montaj konumları ve kaynak revizyonunu doğrula; findings.json üret.

**Kabul** A02, A03 ve A04 için beklenen bulgular kaynak dosyayla eşleşir.

**Sorun halinde** Ham raporu kaybetme; muafiyetleri gerekçesiz PASS sayma.

### PF 01 T04 Gösterim raporunu oluştur

**Bağımlılık** T03. Girdi: Bulgu listesi ve teslim dosyaları.

**Yapılacak değişiklik** demo/review.html içinde kaynak, sürümler, çıktı listesi, açık bulgular ve iddia sınırını göster.

**Kabul** HTML yerelde açılır; bağlantılar geçerlidir ve kırmızı bulgular saklanmaz.

**Sorun halinde** Eksik test kaydına sonuç uydurma; NOT\_RUN göster.

### Uygulama kararları

Başlangıç DRC/ERC bulgularını otomatik temizlemek ilk örneğin kapsamını büyütür. Mevcut bulguları raporla; yalnız bu çalışmanın ürettiği tutarsızlıkları gider. Şema, PCB, BOM ve üretim dosyaları aynı kaynak revizyonuna bağlanmalıdır.

## Kabul ve portföy teslimi

### PF 01 T05 Kabul denemelerini çalıştır

**Bağımlılık** T04. Girdi: A01 ile A06 senaryoları.

**Yapılacak değişiklik** Temiz ve bozuk teslim kopyalarını ayrı çalıştır; komut, çıkış kodu, beklenen ve gözlenen sonucu kaydet.

**Kabul** Olumsuz örnekler reddedilir; tekrarda anlamlı içerik aynı kalır.

**Sorun halinde** Yalnız dosya sayısına veya test sayısına bakarak kabul etme.

### PF 01 T06 Portföy paketini kapat

**Bağımlılık** T05. Girdi: Kabul kayıtları ve HTML raporu.

**Yapılacak değişiklik** README.md, docs/case-study.md ve demo ZIP dosyasını hazırla; kaynak lisanslarını koru.

**Kabul** Müşteri üç dakika içinde katkıyı ve açık sınırları anlayabilir; temiz kurulum talimatı denenmiş.

**Sorun halinde** Açık teknik bulgu varsa portföyde görünür tut; üretim onayı yazma.

### Çalıştırma sözleşmesi

python scripts/run\_demo.py --out demo<br>python -m pytest -q

Komutlar T00 ortam kararına göre tamamlanır. run\_demo geliştirme sırasında oluşturulur; bugün hazır değildir. --out gösterim kökünü belirtir; PF-01 üretim dosyaları bunun output alt dizinine gider. Çıkış kodları: 0 başarılı işlem, 2 beklenen veri/kabul sorunu, 1 araç hatası. Genel demo, bozuk örnekleri beklendiği gibi reddettiğinde başarılı olabilir; her örneğin sonucu ayrı kalır.

### Her görev sonunda bırakılacak kayıt

task\_id, status, input\_hashes, changed\_files, command, exit\_code, expected, observed ve next\_task alanlarını içeren kısa bir görev kaydı bırak. Komut çalıştırılmadıysa NOT\_RUN; gerekli girdi yoksa BLOCKED yaz. Kayıt tamamlandığında kapsam içindeki sıradaki göreve geç; her adımda yeniden izin isteme.

### Codex için ilk görev metni

PF-01 için bu Görev Planı ile eşleşen İş Akışı raporunu oku. Portfolyo/29.10.2026 içindeki manifestten repo yolunu bul. Kaynak kartın erişimi, lisansları ve dosya biçimini doğrula; bir baseline kaydı çıkar. Kaynak uyumsuzsa otomatik dönüştürme yapma; dosya yolu ve gerekçeyle BLOCKED kaydı üret. İlk çıktı T00 kaydı ve kaynak kilidi olsun. Bu turda T00 sonucuna göre T01'e geç; engel varsa dosya ve gerekçeyi açıkça yaz. Önceden var olan kullanıcı dosyalarını koru.

**Bitiş koşulu** İş Akışı raporundaki A01 ile A06 kontrolleri sonuçlanmış; demo açılmış; kısa vaka çalışması kaynak ve sınırları belirtmiş; yeniden çalıştırma kaydı var. Yalnız test sayısı veya klasörlerin varlığı tamamlanma ölçütü değildir.
