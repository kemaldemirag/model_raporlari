# REP 02 Codex Görev Planlaması

Power Rail Validation and Review için pozisyon bazlı görev ve prompt raporu

Kaynak: REP-02 İş Akışı Raporu v1.0  |  JOB-02 / JOB-03  |  Tarih: 17.10.2026

## Amaç ve uygulama sınırı

Bu rapor, kaynak iş akışındaki WF-02-01 ile WF-02-07 arasındaki yedi pozisyonu Codex tarafından yürütülebilecek görevlere ve doğrudan kullanılabilir promptlara dönüştürür. Plan yalnız görev tanımıdır; repo üzerinde işlem yapmaz. G1 BLOCKED iken veri sözleşmesi, sentetik model, fixture ve otomatik testler hazırlanabilir. Gerçek şematik veya PCB çalışması bu ürünün kapsamına alınmaz ve donanım kapılarını bekler.

Her çıktı SOURCE\_EXPLICIT, DERIVED, ENGINEERING\_RULE, ASSUMPTION, UNKNOWN veya CONFLICT sınıfını korumalıdır. Her kural sonucu PASS, FAIL, UNKNOWN, BLOCKED veya NOT\_RUN durumlarından biriyle; rule\_id, reason\_code, kaynak, sürüm ve input hash bilgileriyle raporlanmalıdır.

## Pozisyon haritası

| **Pozisyon** | **Sorumluluk** | **Ana teslim** |
| --- | --- | --- |
| WF-02-01 | Veri sözleşmesi | JSON/CSV şemaları, fixture ve sözleşme testleri |
| WF-02-02 | Rail budget | Hesap motoru ve sonuç JSON |
| WF-02-03 | Sequencing | FSM ve deterministik olay izi |
| WF-02-04 | Fault injection | Fault runner ve negatif testler |
| WF-02-05 | Regulator review | Review rules ve model provenance |
| WF-02-06 | Evidence and integration | Markdown/JSON rapor ve sürümlü dosya API |
| WF-02-07 | CLI and release | CLI, CI, test logu, README ve MVP paketi |

## Ortak Codex çalışma sözleşmesi

- Kaynak iş akışındaki yedi pozisyonu koru; yeni rol veya donanım tasarım faaliyeti ekleme.

- Mevcut dosyaları silme veya üzerine yazma, Git push yapma ve yeni paket kurma işlemlerini kullanıcı onayı olmadan gerçekleştirme.

- Eksik müşteri veya kaynak parametresini tahmin ederek PASS üretme; UNKNOWN veya BLOCKED durumunu açık tut.

- Aynı kanonik fixture aynı teknik sonucu ve mümkünse aynı içerik hashini üretmelidir.

- Sentetik test sonuçlarını fiziksel ölçüm, EMC, ISO, AEC veya saha başarısı olarak sunma.

- Her pozisyon sonunda gerçek test durumunu PASS, FAIL, UNKNOWN, BLOCKED veya NOT\_RUN olarak bildir; çalıştırılmayan testi geçmiş gösterme.

- Çıktılarda rule\_id, reason\_code, source reference, source version, input hash ve tool/package version alanlarını koru.

## WF 02 01 Veri Sözleşmesi

Amaç: Rail, load, regulator, source, event ve finding varlıkları için makinece doğrulanabilir JSON ve CSV sözleşmelerini kurmak.

Girdiler ve bağımlılıklar: REP-02 kapsamı; kaynak sınıfları; sonuç durumları; REP-01 ve REP-03 ile sürümlü dosya entegrasyonu beklentisi.

Beklenen teslimatlar: contracts klasörü; JSON Schema dosyaları; CSV alan sözlüğü; geçerli, eksik ve bozuk fixture seti; sözleşme testleri; sürüm notu.

### Adım adım Codex görevleri

1. Rail, load, regulator, source, event ve finding nesnelerinin zorunlu ve isteğe bağlı alanlarını çıkar.

2. Kaynak sınıfı, sonuç durumu, rule\_id, reason\_code, source/version ve input\_hash alanlarını ortak sözleşmeye yerleştir.

3. JSON ve CSV temsillerinde birim, tip, null/unknown ve kimlik kurallarını açıkla.

4. Geçerli, eksik alanlı, yanlış tipli, bilinmeyen limitli ve çelişkili örnek fixture üret.

5. Şema doğrulama ve geriye dönük uyumluluk testlerini yaz; yanlış tipin FAIL, eksik karar girdisinin UNKNOWN/BLOCKED olduğunu doğrula.

6. D1 sözleşme onayına sunulacak değişiklik, test ve açık karar özetini üret.

### Sınırlar ve blokajlar

UNKNOWN değeri saklanmalı, sıfır veya boş metinle karıştırılmamalıdır. Birim belirtilmeyen sayısal değer sessizce kabul edilmemelidir. Sözleşme sürümü değişmeden kırıcı alan değişikliği yapılmamalıdır.

### Pozisyon kabul ölçütü

Geçerli fixture kabul edilir; yanlış tip ve geçersiz birim FAIL üretir; eksik müşteri limiti PASS olmaz; tüm şemalar sürümlüdür ve sözleşme testleri gerçek logla çalıştırılmıştır.

### Codex prompt zinciri

#### Prompt 1 Veri modeli envanteri

REP-02 iş akışına göre rail, load, regulator, source, event ve finding varlıklarını çıkar. Her varlık için zorunlu alan, isteğe bağlı alan, veri tipi, birim, kimlik, kaynak sınıfı ve UNKNOWN davranışını tablo halinde tasarla. Henüz dosya oluşturma; çelişki ve eksikleri CONFLICT veya UNKNOWN olarak raporla.

#### Prompt 2 Şema ve fixture üretimi

Onaylanan veri modeline göre src/contracts altında sürümlü JSON Schema ve CSV alan sözlüğü taslaklarını oluştur. Geçerli, eksik, yanlış tipli, bilinmeyen limitli ve çelişkili fixture örnekleri ekle. Her fixture için beklenen PASS/FAIL/UNKNOWN/BLOCKED sonucunu ve reason\_code değerini tanımla. Mevcut dosyanın üzerine yazmadan önce değişiklik listesini göster.

#### Prompt 3 Sözleşme testleri ve D1 kanıtı

Şemalar için otomatik sözleşme testleri yaz ve mevcut ortamda çalıştır. Yanlış tipin FAIL, eksik karar parametresinin UNKNOWN veya BLOCKED olduğunu doğrula. Test aracı/sürümü, komut, gerçek log, input hash ve başarısız testleri kaydet. Çalışmayan testi NOT\_RUN göster. D1 sözleşme onayı için kısa kanıt özeti üret.

## WF 02 02 Rail Budget

Amaç: Akım, güç, kapasite ve marj hesaplarını açık varsayımlarla yapan deterministik hesap motorunu geliştirmek.

Girdiler ve bağımlılıklar: WF-02-01 sözleşmeleri; rail ve load fixture verileri; Imin, Ityp, Imax; gerilim, kapasite, limit ve açık verim varsayımları.

Beklenen teslimatlar: rail\_budget modülü; hesap sonucu JSON; formül ve birim dokümanı; pozitif, negatif ve sınır testleri.

### Adım adım Codex görevleri

1. Rail ve load sözleşmelerini hesap motorunun giriş tiplerine bağla.

2. P=VI, toplam akım, yük yüzdesi, kapasite marjı ve derating için açık formüller ve yuvarlama kuralları tanımla.

3. Verim belirtilmişse kayıp/giriş gücü hesabını yap; belirtilmemişse varsayımı etiketle veya sonucu UNKNOWN bırak.

4. Aşırı yük, negatif akım, sıfır gerilim, bilinmeyen limit ve birim uyuşmazlığı senaryolarını uygula.

5. Sonucu kural bazlı JSON olarak rule\_id, reason\_code, kaynak ve input hash ile üret.

6. Elle hesaplanan referans örneklerle otomatik sonuçları karşılaştır.

### Sınırlar ve blokajlar

Eksik akım veya kapasite limiti sahte PASS üretmemelidir. Negatif akım ancak sözleşmede açıkça izin verilen geri besleme yönü varsa kabul edilebilir. Varsayılan verim değeri kaynaklı gerçek değer gibi gösterilmemelidir.

### Pozisyon kabul ölçütü

Elle hesaplanan fixture değerleriyle tutarlıdır; birim ve yuvarlama davranışı tanımlıdır; aşırı yük FAIL, bilinmeyen limit UNKNOWN/BLOCKED üretir; aynı giriş aynı sonucu verir.

### Codex prompt zinciri

#### Prompt 1 Hesap sözleşmesi

WF-02-01 şemalarını okuyarak rail budget hesap sözleşmesini tasarla. Imin/Ityp/Imax, gerilim, toplam akım, P=VI, kapasite kullanımı, marj, verim ve derating formüllerini birimleriyle tanımla. Yuvarlama ve tolerans politikasını açıkla. Kaynakta olmayan verim veya limit için sayı uydurma.

#### Prompt 2 Motor ve sonuç JSON

Onaylı hesap sözleşmesine göre src/rail\_budget altında saf ve deterministik hesap fonksiyonları oluştur. Sonuç JSON içinde hesap girdileri, formül sürümü, rule\_id, status, reason\_code, source reference ve input\_hash bulunsun. Aşırı yük, negatif akım, sıfır gerilim ve bilinmeyen limit davranışlarını açıkça uygula.

#### Prompt 3 Test ve elle doğrulama

Rail budget için küçük elle hesaplanabilir fixture seti oluştur. Otomatik sonuçları elle hesaplarla karşılaştır; tolerans dışını FAIL yap. Eksik limitin PASS olmadığını, birim uyuşmazlığının FAIL olduğunu ve aynı fixtureın iki çalıştırmada aynı teknik sonucu verdiğini test et. Gerçek test komutlarını ve loglarını raporla.

## WF 02 03 Sequencing

Amaç: VIN\_VALID, EN, PGOOD ve RESET olaylarını zaman pencereleriyle değerlendiren deterministik durum makinesi kurmak.

Girdiler ve bağımlılıklar: WF-02-01 event sözleşmesi; kaynaklı veya sentetik zaman sınırları; normal açılış, yanlış sıra, timeout ve power-off fixtureları.

Beklenen teslimatlar: sequencing FSM; olay izi; zaman kuralı tanımları; reason\_code kataloğu; otomatik testler.

### Adım adım Codex görevleri

1. Durumları, olayları, guard koşullarını ve zaman pencerelerini açık bir FSM sözleşmesine dönüştür.

2. Olay zaman damgası, sıralama, aynı zamanlı olay ve eksik olay politikasını tanımla.

3. Normal açılış için VIN\_VALID → EN/rail → PGOOD → RESET akışını fixture ile göster; exact timing yoksa sentetik etiketle.

4. Yanlış sıra, minimum/maksimum pencere ihlali, timeout, tekrar eden olay ve power-off senaryolarını uygula.

5. Her ihlal için rule\_id, ihlal edilen sınır, beklenen/ölçülen zaman ve reason\_code içeren deterministik olay izi üret.

6. Aynı olay listesinin aynı sonucu verdiğini ve giriş sıralama politikasının açık olduğunu test et.

### Sınırlar ve blokajlar

PGOOD, EN ve RESET eşikleri GAP-03 kapanmadan gerçek donanım davranışı sayılmaz. Sentetik zaman sınırları üretici gereksinimi gibi gösterilmez. Eksik olay otomatik olarak başarı kabul edilmez.

### Pozisyon kabul ölçütü

Normal iz PASS, yanlış sıra veya kaynaklı sınır ihlali FAIL, eksik/kararsız zaman sınırı UNKNOWN/BLOCKED üretir; ihlal edilen pencere ve reason\_code rapordadır.

### Codex prompt zinciri

#### Prompt 1 FSM sözleşmesi

VIN\_VALID, EN, PGOOD ve RESET için durum, olay, guard, timeout ve power-off davranışını tanımlayan FSM sözleşmesi hazırla. Kaynaklı zaman sınırı ile sentetik örnek sınırı ayrı etiketle. Aynı zaman damgası, eksik olay ve sırasız giriş politikasını açıkça belirt.

#### Prompt 2 Sequencing motoru

Onaylanan FSM sözleşmesine göre src/sequencing altında deterministik değerlendirme motoru geliştir. Çıktı olay izi her geçişi, beklenen durumu, gözlenen olayı, ihlal edilen pencereyi, rule\_id ve reason\_code değerini içersin. Eksik kaynaklı eşiklerde PASS üretme.

#### Prompt 3 Sınır ve negatif testler

Normal açılış, yanlış sıra, minimum ve maksimum sınırın hemen altı/üstü, timeout, tekrar eden olay, eksik PGOOD ve power-off fixtureları oluştur. Testleri çalıştır ve her senaryonun beklenen PASS/FAIL/UNKNOWN/BLOCKED sonucunu doğrula. GAP-03 etkisini ayrıca raporla.

## WF 02 04 Fault Injection

Amaç: Brownout, stuck-low, gecikme, ray kaybı ve yeniden başlatma arızalarını tekrarlanabilir biçimde enjekte etmek.

Girdiler ve bağımlılıklar: WF-02-02 rail motoru; WF-02-03 sequencing motoru; fault sözleşmesi; sentetik fixture ve kurtarma politikaları.

Beklenen teslimatlar: fault runner; fault fixtureları; negatif ve kurtarma testleri; deterministik sonuç logu.

### Adım adım Codex görevleri

1. Arıza tiplerini hedef, başlangıç zamanı, süre, şiddet ve kurtarma davranışıyla sözleşmeye bağla.

2. Brownout, stuck-low, gecikme, ray kaybı ve restart senaryoları için ayrı fixture oluştur.

3. Tekli arızalar ile seçilmiş arıza+kurtarma kombinasyonlarını tanımla.

4. Fault runnerı rail budget ve sequencing modüllerinden ayrık, tekrarlanabilir bir katman olarak uygula.

5. FAIL ile UNKNOWN durumlarını ayır; bilinmeyen kaynak sınırını ihlal edilmiş gerçek sınır gibi değerlendirme.

6. Aynı fixtureın tekrarlı çalıştırmada aynı teknik sonuç ve olay izini verdiğini test et.

### Sınırlar ve blokajlar

Arıza enjeksiyonu fiziksel donanım güvenilirliği kanıtı değildir. Rastgele senaryolar seed ve sürüm olmadan kabul edilmez. Kurtarma davranışı kaynakta yoksa ENGINEERING\_RULE veya ASSUMPTION olarak ayrılmalıdır.

### Pozisyon kabul ölçütü

Her arıza açık sözleşme ve beklenen sonuç taşır; aynı fixture aynı sonucu verir; FAIL ve UNKNOWN ayrıdır; kurtarma yolu gözlenebilir olay iziyle raporlanır.

### Codex prompt zinciri

#### Prompt 1 Fault sözleşmesi ve matrisi

Brownout, stuck-low, delay, rail-loss ve restart için fault sözleşmesini tasarla. Her arıza için target, start\_time, duration, severity, recovery ve beklenen status alanlarını tanımla. Kaynaksız kurtarma davranışını ASSUMPTION olarak işaretle.

#### Prompt 2 Fault runner

src/faults altında rail ve sequencing motorlarına kontrollü giriş üreten deterministik fault runner geliştir. Sabit fixture veya açık seed kullan. Çıktıda enjekte edilen arıza, etkilenen kurallar, olay izi, status ve reason\_code bulunsun. Fiziksel donanım sonucu iddia etme.

#### Prompt 3 Negatif ve kurtarma testleri

Tekli arıza, arıza+kurtarma, arıza sırasında restart ve bilinmeyen eşik senaryolarını test et. Aynı fixtureı iki kez çalıştırıp teknik sonuçların aynı olduğunu doğrula. FAIL ile UNKNOWN ayrımını ve her recovery yolunun gözlenebilir izini raporla.

## WF 02 05 Regulator Review

Amaç: Regülatör adaylarını VIN, IOUT, sıcaklık, derating ve model provenansı açısından kaynaklı kurallarla incelemek.

Girdiler ve bağımlılıklar: Regulator/source sözleşmeleri; revizyonlu üretici kaynakları; sentetik veya gerçek çalışma noktaları; varsa lisanslı SPICE modelleri.

Beklenen teslimatlar: review rules; regulator sonuç raporu; source ve model provenance register; transient/stability ayrım testleri.

### Adım adım Codex görevleri

1. VIN, IOUT, sıcaklık ve derating için kural girişlerini ve kaynak gereksinimlerini tanımla.

2. Kaynak revizyonu, sayfa/bölüm, model dosyası hash, lisans ve üretici kökeni alanlarını provenance kaydına ekle.

3. Çalışma noktasını mutlak maksimum, önerilen çalışma ve derating kurallarından ayrı değerlendir.

4. Eksik model, limit dışı yük, eksik sıcaklık ve çelişkili kaynak senaryolarını uygula.

5. Transient cevabı ile loop stability kanıtını farklı rule\_id ve sonuçlar olarak raporla.

6. GAP-02 kapanmadıysa exact model veya kararlılık için PASS üretme.

### Sınırlar ve blokajlar

Transient simülasyonu kararlılık kanıtı değildir. Üretici olmayan veya lisansı belirsiz model onaylı kanıt sayılmaz. Absolute maximum altında olmak tek başına önerilen çalışma uygunluğu değildir.

### Pozisyon kabul ölçütü

Her inceleme kaynak ve sürüm taşır; çalışma aralığı/derating değerlendirmesi tekrarlanabilir; model provenance eksikliği UNKNOWN/BLOCKED üretir; transient ve stability sonuçları ayrıdır.

### Codex prompt zinciri

#### Prompt 1 Review kural kataloğu

VIN, IOUT, sıcaklık, derating, absolute maximum, recommended operating range, transient ve stability için ayrı rule\_id kataloğu tasarla. Her kuralın gerekli kaynağını, girişlerini, PASS/FAIL/UNKNOWN/BLOCKED koşullarını ve reason\_code değerlerini tanımla.

#### Prompt 2 Provenance ve değerlendirme motoru

src/review ve source/evidence register yapısını oluştur. Üretici, belge revizyonu, sayfa/bölüm, model dosyası hash, lisans, tool version ve input hash alanlarını zorunlu kıl. Eksik model veya kaynakta exact-part kararı verme; UNKNOWN/BLOCKED üret.

#### Prompt 3 Transient stability ayrım testi

Transient cevabın loop stability kanıtı sayılmadığını otomatik testle doğrula. Transient verisi mevcut, stability kanıtı eksik bir fixture oluştur; transient kuralı kendi sonucunu üretirken stability kuralının UNKNOWN/BLOCKED kaldığını göster. Gerçek log ve reason\_code değerlerini raporla.

## WF 02 06 Evidence and Integration

Amaç: Kural bazlı bulguları kanıt zinciriyle raporlamak ve REP-01/03 için sürümlü dosya API sağlamak.

Girdiler ve bağımlılıklar: WF-02-01 ile WF-02-05 sonuçları; source/evidence register; çoklu konfigürasyonlar; REP-01/03 dosya tabanlı entegrasyon gereksinimi.

Beklenen teslimatlar: Markdown ve JSON rapor; evidence manifest; sürümlü dosya API sözleşmesi; entegrasyon fixture ve testleri.

### Adım adım Codex görevleri

1. Tüm modüllerin ortak finding yapısına uyumunu doğrula.

2. Raporlayıcıda rule\_id, status, reason\_code, kaynak, sürüm, input hash ve tool version alanlarını zorunlu kıl.

3. İnsan okunur Markdown ile makine okunur JSON çıktılarının aynı teknik bulguları taşımasını test et.

4. REP-01 ve REP-03 için giriş/çıkış dosya adları, şema sürümü, uyumluluk ve hata davranışını tanımla.

5. Eksik kaynak, çoklu konfigürasyon, uyumsuz şema ve bozuk dosya senaryolarını test et.

6. Evidence manifest içinde kullanılan girdiler, kaynaklar, modeller, araçlar ve çıktı hashlerini kaydet.

### Sınırlar ve blokajlar

Entegrasyon diğer repoların dosyalarını sessizce değiştirmemelidir. Şema sürümü uyumsuzluğu açık FAIL/BLOCKED olmalıdır. Markdown ve JSON raporları birbirinden farklı teknik sonuç taşımamalıdır.

### Pozisyon kabul ölçütü

Her bulguda kural, kaynak, sürüm ve hash vardır; Markdown/JSON semantik olarak tutarlıdır; REP-01/03 entegrasyonu bağımsız modül sınırlarını korur; eksik kaynak sahte PASS üretmez.

### Codex prompt zinciri

#### Prompt 1 Evidence şeması ve raporlayıcı

WF-02-01 finding sözleşmesini temel alarak src/evidence altında raporlayıcı tasarla. Her sonuçta rule\_id, status, reason\_code, source reference, source version, input\_hash ve tool/package version zorunlu olsun. Aynı bulgulardan Markdown ve JSON üret.

#### Prompt 2 REP 01 ve REP 03 dosya API

REP-01 ve REP-03 ile entegrasyon için sürümlü dosya API sözleşmesi hazırla. Dosya adı, schema\_version, producer, consumer, required fields, compatibility policy ve error behavior alanlarını tanımla. Diğer repoları değiştirme; yalnız sözleşme ve fixture üret.

#### Prompt 3 Entegrasyon ve tutarlılık testleri

Eksik kaynak, çoklu profil, uyumsuz şema, bozuk dosya ve normal entegrasyon fixtureları oluştur. Markdown ve JSON çıktılarındaki rule\_id/status/reason\_code setlerinin aynı olduğunu test et. Kullanılan tüm girdi ve çıktıların hashlerini evidence manifestine kaydet.

## WF 02 07 CLI and Release

Amaç: Çekirdek modülleri tek CLI üzerinden çalıştırmak ve temiz ortamda yeniden üretilebilir sürümlü MVP paketi hazırlamak.

Girdiler ve bağımlılıklar: WF-02-01 ile WF-02-06 kabul edilmiş çıktıları; önerilen modül mimarisi; sentetik fixturelar; kullanıcı kılavuzu ve lisans gereksinimi.

Beklenen teslimatlar: CLI; exit-code sözleşmesi; CI iş akışı; temiz kurulum logu; README ve quickstart; sürüm/hash manifesti; MVP release paketi.

### Adım adım Codex görevleri

1. CLI komut ağacını validate, rail-budget, sequence, inject-fault, review ve report görevlerine bağla.

2. Komutların giriş/çıkış dosyalarını, stdout/stderr davranışını ve exit-code sözleşmesini tanımla.

3. Bozuk fixture, farklı profil, eksik kaynak ve normal çalışma senaryolarını CLI üzerinden test et.

4. Bağımlılıkları ve sürümleri kilitle; temiz kurulum adımlarını README ve quickstart içinde yaz.

5. CI üzerinde şema, birim, negatif, entegrasyon ve tekrarlanabilirlik testlerini çalıştır.

6. Lisans, sınırlamalar, source/evidence register ve gelecekteki saha verisi adaptörlerini teslim paketine ekle.

7. D4 sürümlü MVP için release manifesti, output hashleri ve gerçek test logunu üret.

### Sınırlar ve blokajlar

Temiz kurulum gerçekten çalıştırılmadan PASS verilmez. Paket kurulumu, Git commit/push veya release yayımlama kullanıcı onayı gerektirir. Sentetik MVP fiziksel saha doğrulaması olarak sunulmaz.

### Pozisyon kabul ölçütü

Bağımsız temiz ortamda README komutlarıyla çalışır; CLI hataları tutarlı exit code üretir; CI gerçek log taşır; aynı kanonik fixture aynı teknik sonucu verir; release manifesti ve hashler eksiksizdir.

### Codex prompt zinciri

#### Prompt 1 CLI sözleşmesi

validate, rail-budget, sequence, inject-fault, review ve report komutlarından oluşan CLI sözleşmesini tasarla. Her komut için giriş, çıkış, stdout/stderr, exit code, status ve örnek kullanım tanımla. Uygulama yapmadan önce modül eşlemesini ve kırıcı kararları raporla.

#### Prompt 2 CLI CI ve dokümantasyon

Onaylanan sözleşmeye göre CLI katmanını oluştur; mevcut çekirdek modülleri kopyalamadan çağır. Şema, birim, negatif, entegrasyon ve tekrarlanabilirlik testlerini CI akışına ekle. README ve quickstart içinde sentetik kapsamı, sınırlamaları ve örnek konfigürasyonları açıkla.

#### Prompt 3 Temiz kurulum ve MVP kanıtı

Yeni ve temiz bir ortamda README kurulum ve quickstart adımlarını gerçekten çalıştır. Paket kurulumu veya Git işlemi gerekiyorsa önce kullanıcı onayı iste. Gerçek komutları, sürümleri, test loglarını, başarısızlıkları ve NOT\_RUN kalemlerini kaydet. Lisans, evidence register, sınırlamalar ve SHA-256 manifestiyle D4 MVP teslim özetini üret; fiziksel doğrulama iddiası ekleme.

## Oturum ve kapı planı

| **Oturum** | **Codex kapsamı** | **Kapı veya sonuç** |
| --- | --- | --- |
| 1 | WF-02-01 veri ve ürün sözleşmesi; araştırma boşluklarının çıkarılması | D1 sözleşme onayı |
| 2 | WF-02-02 rail budget çekirdek motoru | D2 sentetik uygulama parçası |
| 3 | WF-02-03 sequencing çekirdek motoru | D2 sentetik uygulama tamamlanır |
| 4 | WF-02-04 fault injection ve WF-02-06 ilk raporlama | D3 test ve kanıt başlangıcı |
| 5 | WF-02-05 negatif kurallar ve REP-01/03 entegrasyonu | D3 test ve kanıt kapanışı |
| 6 | WF-02-07 temiz kurulum, CI, bağımsız inceleme ve paket | D4 sürümlü MVP |

Bu oturumlar süre taahhüdü değildir. Donanım G1-G6 kapıları ayrı izlenir; yazılım D1-D4 kapıları fiziksel donanım onayı yerine geçmez.

## Görev kapanış raporu şablonu

- Pozisyon ve görev kimliği

- Sonuç durumu: PASS / FAIL / UNKNOWN / BLOCKED / NOT\_RUN

- Oluşturulan veya değiştirilen dosyalar

- Çalıştırılan komutlar, araçlar ve sürümler

- Test sayısı, başarısız testler ve gerçek log konumu

- Kullanılan kaynaklar, revizyonlar ve input/output hashleri

- Açık GAP-01…04 etkileri ve Claude araştırma ihtiyacı

- Bir sonraki görev ve başlaması için gereken kapı

## Sonuç

Yürütme WF-02-01 veri sözleşmesiyle başlamalıdır. WF-02-02 ve WF-02-03 çekirdek motorları sözleşme onayından sonra geliştirilir; fault injection ve evidence katmanı bunların üzerine kurulur. Regulator review kaynak ve model provenansını korur. CLI ve release görevi, önceki modüllerin kabul kanıtı olmadan tamamlanmış sayılmaz. G1 BLOCKED durumu gerçek şematik ve PCB faaliyetlerini engeller; sentetik yazılım MVP görevlerini durdurmaz.
