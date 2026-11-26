**REP 02 Codex Görev Planlaması**

REP-02 | Power Rail Validation and Review

Power Rail Validation and Review için pozisyon bazlı görev ve prompt raporu

Kaynak: REP-02 İş Akışı Raporu v2.0 | JOB-02 / JOB-03 | Tarih: 26.10.2026 | v1 (17.10.2026) yerine geçer | Repo: github.com/kemaldemirag/power-rail-validation-and-review

# Amaç ve uygulama sınırı

Bu rapor, iş akışındaki WF-02-01 ile WF-02-07 arasındaki yedi pozisyonu korur. v1 görevlerinin tamamı sentetik kapsamda yürütüldü (D1–D4, D5 intake; 404 test). v2 görevleri yeni pozisyon açmadan mevcut pozisyonların içine yerleştirilmiştir: sözleşme v2 (WF-02-01), gerçek regülatör kanıtı (WF-02-05), referans dikey dilim (WF-02-06) ve lisans/uzak CI/release (WF-02-07). WF-02-02/03/04 için yalnız regresyon ve referans girdisiyle karşılaştırma görevi vardır.

Her çıktı SOURCE\_EXPLICIT, DERIVED, ENGINEERING\_RULE, ASSUMPTION, UNKNOWN, CONFLICT, REFERENCE\_DESIGN veya PORTFOLIO\_DECISION sınıfını korumalıdır. Her kural sonucu PASS, FAIL, UNKNOWN, BLOCKED veya NOT\_RUN; rule\_id, reason\_code, kaynak, sürüm ve input hash ile raporlanır.

# Pozisyon haritası

| **Pozisyon** | **Sorumluluk** | **Ana teslim** | **Durum (26.10)** |
| --- | --- | --- | --- |
| WF-02-01 | Veri sözleşmesi | Sözleşme v2 (yeni kaynak sınıfları) | D1 tamam; v2 NOT\_STARTED |
| WF-02-02 | Rail budget | Referans girdisiyle karşılaştırma | D2 tamam |
| WF-02-03 | Sequencing | Referans eşikleriyle karşılaştırma | D2 tamam |
| WF-02-04 | Fault injection | Regresyon | D3 tamam |
| WF-02-05 | Regulator review | Tek exact regülatör review (GAP-02) | D3 tamam; GAP-02 açık |
| WF-02-06 | Evidence and integration | Referans dikey dilim kanıt paketi | D5 intake var; dilim BLOCKED |
| WF-02-07 | CLI and release | Lisans, uzak CI, tag, release | D4 yerel PASS |

# Ortak Codex çalışma sözleşmesi

- Repodaki AGENTS.md dosyasını oku; yoksa ilk görevde oluştur (durum kodları, test komutu, dokunulmayacak D4 artifact'ları, sözleşme sürüm kuralı).

- Kaynak iş akışındaki yedi pozisyonu koru; yeni rol veya donanım tasarım faaliyeti ekleme.

- Mevcut dosyaları silme veya üzerine yazma, Git push yapma ve yeni paket kurma işlemlerini kullanıcı onayı olmadan gerçekleştirme. Dondurulmuş D4 (0.5.0) artifact'larına dokunma.

- Eksik müşteri veya kaynak parametresini tahmin ederek PASS üretme; UNKNOWN veya BLOCKED durumunu açık tut.

- Referans tasarım kullanırken sabit commit veya doküman revizyonu, SHA-256, lisans ve sayfa/satır referansını references/<ad>/reference.lock.json içine yaz; değerleri script ile çıkar; iki çalıştırmada bayt bayt aynı çıktıyı doğrula. GPL lisanslı tasarımlardan yalnız veri çıkar, tasarım dosyası kopyalama.

- Aynı kanonik fixture aynı teknik sonucu ve mümkünse aynı içerik hashini üretmelidir.

- Sentetik veya referans test sonuçlarını fiziksel ölçüm, EMC, ISO, AEC veya saha başarısı olarak sunma.

- Her pozisyon sonunda gerçek test durumunu ve 404+ testin sonucunu bildir; çalıştırılmayan testi geçmiş gösterme.

# WF 02 01 Veri Sözleşmesi

**Amaç:** Mevcut veri sözleşmesini kırmadan REFERENCE\_DESIGN ve PORTFOLIO\_DECISION kaynak sınıflarını eklemek.

**Girdiler ve bağımlılıklar:** Mevcut src/power\_rail\_validation/contracts şemaları ve field\_dictionary.csv; REP-01 İş Akışı v2 kaynak sınıfı tanımları.

**Beklenen teslimatlar:** Sürümü artırılmış şemalar; v1 ve v2 fixture'ları; uyumluluk testleri; sözleşme değişiklik notu.

**Durum (26.10):** D1 tamam; v2 eklemesi başlamadı.

## Adım adım Codex görevleri

1. Kaynak sınıfı enum'unun geçtiği tüm şema, sözlük ve doğrulayıcı noktalarını listele.

2. Yeni sınıflar için zorunlu alanları tanımla (revizyon/commit, SHA-256, lisans, sayfa/satır; karar kaydı yolu ve imza).

3. Sözleşme sürümünü artır; v1 belgeleri v1 olarak doğrulanmaya devam etsin.

4. v1, v2 ve hatalı v2 fixture'ları ekle.

5. Tüm testleri çalıştır; değişiklik notu yaz.

## Sınırlar ve blokajlar

Sözleşme sürümü değişmeden kırıcı alan değişikliği yapılmaz. REFERENCE\_DESIGN bir müşteri veya REP-01 kanıtı gibi raporlanmaz.

## Pozisyon kabul ölçütü

v1 fixture'ları kırılmaz; yeni sınıflar zorunlu provenans alanları olmadan FAIL üretir; 404+ test geçer.

## Codex prompt zinciri

### Prompt 1 Veri modeli envanteri

Kaynak sınıfı enum'unun ve source şemasının kullanıldığı tüm dosyaları listele. REFERENCE\_DESIGN ve PORTFOLIO\_DECISION eklemek için gereken alanları ve kırılma riskini tablo halinde raporla. Dosya değiştirme.

### Prompt 2 Şema ve fixture üretimi

Onaylanan tasarıma göre şema sürümünü artır, yeni sınıfları ve zorunlu provenans alanlarını ekle. v1 belgelerinin v1 olarak doğrulanmasını koru. Geçerli ve eksik provenanslı v2 fixture'ları ekle; her birinin beklenen status ve reason\_code değerini tanımla.

### Prompt 3 Sözleşme testleri ve D1 kanıtı

Tüm test paketini çalıştır. v1 uyumluluğunu, yeni sınıfların provenans zorunluluğunu ve hatalı girdinin FAIL ürettiğini doğrula. Komut, sürüm, test sayısı ve gerçek log ile sözleşme v2 kanıt özetini üret.

# WF 02 02 Rail Budget

**Amaç:** Mevcut rail-budget motorunu referans tasarımın yayınlanmış yük ve verim verileriyle karşılaştırmak.

**Girdiler ve bağımlılıklar:** Mevcut motor; referans dikey dilimin intake descriptor'ı (WF-02-06).

**Beklenen teslimatlar:** Referans karşılaştırma fixture'ı ve raporu.

**Durum (26.10):** D2 tamam.

## Adım adım Codex görevleri

1. Referans dilimdeki ray ve yükleri rail-budget girdisine dönüştür.

2. Motoru çalıştır; sonuçları yayınlanmış değerlerle karşılaştır.

3. Sapmaları tolerans politikasıyla FAIL veya UNKNOWN olarak raporla.

## Sınırlar ve blokajlar

Motor kodu yalnız hata bulunursa ve ayrı onayla değiştirilir. Referans değeri kaynaksız düzeltilmez.

## Pozisyon kabul ölçütü

Karşılaştırma raporu yayınlanmış değerlere sayfa referansıyla bağlıdır; mevcut testler geçer.

## Codex prompt zinciri

### Prompt 1 Hesap sözleşmesi

Referans dilimin yük, gerilim ve verim değerlerinin mevcut rail-budget sözleşmesine eşlenmesini tablolaştır; eşlenemeyen alanları UNKNOWN bırak. Dosya değiştirme.

### Prompt 2 Motor ve sonuç JSON

Eşlemeyi bir fixture olarak ekle ve mevcut CLI'nın rail-budget komutuyla çalıştır. Sonuç JSON'unu evidence altına hash'iyle kaydet. Motor koduna dokunma.

### Prompt 3 Test ve elle doğrulama

Sonuçları referansın yayınlanmış değerleriyle karşılaştıran bir test ekle; tolerans dışını FAIL yap. Sapmaları ve olası nedenlerini raporla; motoru değiştirme önerisi varsa ayrı listele.

# WF 02 03 Sequencing

**Amaç:** Sequencing motorunu referans regülatörün datasheet eşikleriyle çalıştırmak ve GAP-03'ü REFERENCE\_DESIGN olarak kapatmak.

**Girdiler ve bağımlılıklar:** Mevcut FSM; referans regülatör datasheet'inin EN/PGOOD/zamanlama değerleri.

**Beklenen teslimatlar:** Kaynaklı zaman pencereli fixture; GAP-03 kapanış kaydı.

**Durum (26.10):** D2 tamam; GAP-03 açık.

## Adım adım Codex görevleri

1. Datasheet'ten EN eşiği, PGOOD gecikmesi ve soft-start süresini sayfa referansıyla çıkar.

2. Bu değerlerle normal ve sınır fixture'ları oluştur.

3. Motoru çalıştır; GAP-03 kaydını REFERENCE\_DESIGN olarak güncelle.

## Sınırlar ve blokajlar

Datasheet'te olmayan değer uydurulmaz. Referans eşikleri REP-01 tasarımı için geçerli sayılmaz.

## Pozisyon kabul ölçütü

Her eşik sayfa referanslıdır; normal iz PASS, sınır ihlali FAIL üretir.

## Codex prompt zinciri

### Prompt 1 FSM sözleşmesi

Referans regülatör datasheet'inden zamanlama ve eşik değerlerini sayfa numarasıyla tablola. Mevcut sequencing sözleşmesinde karşılığı olmayanları UNKNOWN bırak.

### Prompt 2 Sequencing motoru

Bu değerlerle normal açılış ve sınırın hemen altı/üstü fixture'larını ekle; mevcut motoru sequence komutuyla çalıştır. Motoru değiştirme.

### Prompt 3 Sınır ve negatif testler

Fixture'ların beklenen sonuçlarını testle doğrula. GAP-03 kaydını kaynak sınıfı REFERENCE\_DESIGN olarak güncelle ve hangi tasarım için geçerli olduğunu yaz.

# WF 02 04 Fault Injection

**Amaç:** Mevcut fault runner'ı referans dilimin üzerinde çalıştırarak regresyon kanıtı üretmek.

**Girdiler ve bağımlılıklar:** Mevcut fault runner; referans dilim fixture'ları.

**Beklenen teslimatlar:** Referans dilim için brownout ve rail-loss sonuçları.

**Durum (26.10):** D3 tamam.

## Adım adım Codex görevleri

1. Referans dilim için brownout, rail-loss ve restart senaryoları tanımla.

2. Runner'ı çalıştır; sonuçları evidence altına kaydet.

3. Aynı fixture'ın iki çalıştırmada aynı sonucu verdiğini doğrula.

## Sınırlar ve blokajlar

Arıza enjeksiyonu fiziksel güvenilirlik kanıtı değildir.

## Pozisyon kabul ölçütü

Sonuçlar deterministik; FAIL ve UNKNOWN ayrı.

## Codex prompt zinciri

### Prompt 1 Fault sözleşmesi ve matrisi

Referans dilim için anlamlı arıza senaryolarını mevcut fault sözleşmesiyle tablola; kaynaksız kurtarma davranışını ASSUMPTION olarak işaretle.

### Prompt 2 Fault runner

Senaryoları fixture olarak ekle ve inject-fault komutuyla çalıştır. Runner koduna dokunma.

### Prompt 3 Negatif ve kurtarma testleri

Sonuçların tekrarlanabilirliğini test et; FAIL/UNKNOWN ayrımını ve recovery izlerini raporla.

# WF 02 05 Regulator Review

**Amaç:** GAP-02'yi tek bir exact regülatör için üretici kaynağı ve provenanslı modelle kapatmak.

**Girdiler ve bağımlılıklar:** Referans dilimdeki regülatörün revizyonlu datasheet'i; lisansı uygunsa üretici SPICE modeli; mevcut review kuralları.

**Beklenen teslimatlar:** Regülatör review raporu; model provenance kaydı; transient/stability ayrım kanıtı.

**Durum (26.10):** D3 tamam; gerçek model yok.

## Adım adım Codex görevleri

1. Datasheet revizyonunu, SHA-256'sını ve ilgili sayfaları kaydet.

2. Model dosyası varsa lisansını kontrol et; uygun değilse BLOCKED kaydet.

3. VIN, IOUT, sıcaklık ve derating kurallarını gerçek değerlerle çalıştır.

4. Simülatör ve model varsa transient çalıştır; stability'yi ayrı tut; yoksa NOT\_RUN.

## Sınırlar ve blokajlar

Transient stability kanıtı değildir. Lisansı belirsiz model onaylı kanıt sayılmaz. Simülasyon çalışmadıysa NOT\_RUN.

## Pozisyon kabul ölçütü

Tek regülatör için review kaynaklı ve tekrarlanabilir; model provenansı eksikse BLOCKED.

## Codex prompt zinciri

### Prompt 1 Review kural kataloğu

Mevcut kural kataloğunu referans regülatörün datasheet parametreleriyle eşle; her kural için gerekli sayfa referansını ve eksik veriyi listele.

### Prompt 2 Provenance ve değerlendirme motoru

Datasheet ve (varsa) modelin hash, lisans ve revizyon kaydını register'a ekle; review komutunu çalıştır. Lisansı belirsiz modeli BLOCKED bırak.

### Prompt 3 Transient stability ayrım testi

Simülatör ve model mevcutsa transient çalıştır ve sonucu kaydet; stability kuralının kendi kanıtı olmadan UNKNOWN/BLOCKED kaldığını testle göster. Simülatör yoksa NOT\_RUN raporla.

# WF 02 06 Evidence and Integration

**Amaç:** D5 intake kapısının ilk gerçek girdisi olarak bir referans tasarım dikey dilimini uçtan uca çalıştırmak.

**Girdiler ve bağımlılıklar:** D5 intake sözleşmesi; referans tasarım: TI TIDA-01167 (kullanıcı indirir) veya blinken/vandal (GitHub, GPL-3.0, yalnız veri); WF-02-02/03/05 çıktıları.

**Beklenen teslimatlar:** references/<ad>/ lock dosyası ve çıkarım script'i; intake descriptor; uçtan uca JSON/Markdown rapor; evidence manifest.

**Durum (26.10):** D5 intake uygulandı; REP-01 dilimi BLOCKED; referans dilim başlamadı.

## Adım adım Codex görevleri

1. Referansı seç ve lock dosyasını oluştur (commit veya doküman revizyonu, hash, lisans).

2. Bir ray, bir exact regülatör ve ilgili artifact'ı script ile çıkar.

3. Intake descriptor'ını üret ve intake komutuyla doğrula.

4. rail-budget, sequence, inject-fault, review ve report komutlarını sırayla çalıştır.

5. Evidence manifest'e tüm girdi ve çıktı hash'lerini yaz; REP-01 dilimi için aynı akışı hazır bırak.

## Sınırlar ve blokajlar

Referans dilim REP-01 veya müşteri tasarımı kanıtı değildir. GPL lisanslı tasarım dosyaları repoya kopyalanmaz. Diğer repolar değiştirilmez.

## Pozisyon kabul ölçütü

Dilim uçtan uca çalışır; her bulgu kaynak, sürüm ve hash taşır; script iki çalıştırmada aynı çıktıyı verir.

## Codex prompt zinciri

### Prompt 1 Evidence şeması ve raporlayıcı

Referans adaylarını (TIDA-01167, vandal) lisans, erişilebilirlik ve veri kapsamı açısından kıyasla; birini önerip lock dosyası taslağını hazırla. Erişim yoksa NOT\_RUN ve kullanıcıdan istenecek dosyaları listele.

### Prompt 2 REP 01 ve REP 03 dosya API

Seçilen referanstan bir ray, bir regülatör ve ilgili artifact'ı script ile çıkar; D5 intake descriptor'ını üret ve intake komutuyla doğrula. REP-01 ve REP-03 dosya API sözleşmelerini değiştirme.

### Prompt 3 Entegrasyon ve tutarlılık testleri

Dilimi uçtan uca çalıştır; Markdown ve JSON raporların aynı rule\_id/status/reason\_code setini taşıdığını test et; evidence manifest'i üret; script'in deterministik olduğunu doğrula.

# WF 02 07 CLI and Release

**Amaç:** Uzak CI, lisans ve sürüm etiketleriyle aracı dışarıdan doğrulanabilir hale getirmek.

**Girdiler ve bağımlılıklar:** D4 release adayı; D5 kaynak ağacı; kullanıcının lisans kararı.

**Beklenen teslimatlar:** Uzak CI logu; LICENSE ve SPDX; git tag'leri (v0.5.0, v0.6.0); güncel README.

**Durum (26.10):** D4 yerel PASS; lisans All rights reserved; uzak CI NOT\_RUN.

## Adım adım Codex görevleri

1. Kullanıcının seçtiği lisansı LICENSE ve pyproject'e yansıt.

2. README'ye CI rozeti ve referans dilim sonucunu ekle.

3. Kullanıcı push ettikten sonra uzak CI sonucunu kaydet.

4. Tag ve release notlarını hazırla (yayımlama kullanıcıda).

## Sınırlar ve blokajlar

Lisans Codex tarafından seçilmez. Push ve release yayımlama kullanıcı onayı gerektirir.

## Pozisyon kabul ölçütü

Uzak CI yeşil; lisans açık; README komutları temiz ortamda çalışır.

## Codex prompt zinciri

### Prompt 1 CLI sözleşmesi

Mevcut CLI komutlarını, çıkış kodlarını ve D5 intake komutunu README ile karşılaştır; tutarsızlıkları raporla.

### Prompt 2 CLI CI ve dokümantasyon

Kullanıcının seçtiği lisansı uygula; README'ye referans dilim özetini ve CI rozetini ekle; CI workflow'unu uzak runner için gözden geçir.

### Prompt 3 Temiz kurulum ve MVP kanıtı

Temiz ortamda kurulumu ve quickstart'ı çalıştır; uzak CI sonucunu kullanıcı push ettikten sonra kaydet; tag ve release notu taslağını hazırla.

# Oturum ve kapı planı

| **Oturum** | **Codex kapsamı** | **Kapı veya sonuç** |
| --- | --- | --- |
| 1 | AGENTS.md; WF-02-01 sözleşme v2; lisans kararı (kullanıcı) | D1 v2 |
| 2 | WF-02-06 referans seçimi ve aktarım; intake | Referans intake PASS |
| 3 | WF-02-02/03/04 referans çalıştırmaları | Karşılaştırma raporları |
| 4 | WF-02-05 regülatör review (GAP-02/03) | Tek regülatör kanıtı |
| 5 | WF-02-06 uçtan uca dilim ve evidence manifest | D6 referans dikey dilim |
| 6 | WF-02-07 uzak CI, tag, release | Dışarıdan doğrulanabilir sürüm |

Bu oturumlar süre taahhüdü değildir. REP-01 dilimi açıldığında 2–5 aynı akışla REP-01 artifact'ı için tekrarlanır. Donanım G1–G6 kapıları ayrı izlenir.

# Görev kapanış raporu şablonu

- Pozisyon ve görev kimliği

- Sonuç durumu: PASS / FAIL / UNKNOWN / BLOCKED / NOT\_RUN

- Oluşturulan veya değiştirilen dosyalar; dokunulmayan D4 artifact'ları

- Çalıştırılan komutlar, araçlar ve sürümler

- Test sayısı, başarısız testler ve gerçek log konumu

- Kullanılan kaynaklar ve referanslar: revizyon/commit, lisans, lock dosyası, input/output hashleri

- Açık GAP etkileri ve Claude/kullanıcı ihtiyacı

- Bir sonraki görev ve başlaması için gereken kapı

# Sonuç

v1 planı sentetik kapsamda tamamlandı. v2 yürütmesi sözleşme v2 ve lisans kararıyla başlar; ana hedef, REP-01'i beklemeden bir referans tasarım dikey dilimini uçtan uca çalıştırmaktır. Bu dilim aracın gerçek bir tasarımda doğru sonuç verdiğini gösterir ve REP-01 dilimi açıldığında aynı akış tekrar kullanılır. G1 BLOCKED gerçek şematik ve PCB faaliyetlerini engeller; referans ve sentetik yazılım görevlerini durdurmaz.

Codex görev planlaması • 26.10.2026
