# REP 03 Codex Görev Planlaması

ESP32-S3 Automotive Interfaces için pozisyon bazlı görev ve prompt raporu

Kaynak: REP-03 İş Akışı Raporu v1.0  |  JOB-04 / JOB-05  |  Tarih: 17.10.2026

## Amaç ve uygulama sınırı

Bu rapor, kaynak iş akışındaki WF-03-01 ile WF-03-07 arasındaki yedi pozisyonu Codex tarafından yürütülebilecek görevlere ve doğrudan kullanılabilir promptlara dönüştürür. Plan yalnız görev tanımıdır; repo üzerinde işlem yapmaz. JOB-04 CAN, LIN ve GNSS arayüzlerini; JOB-05 ise Raspberry Pi otomotiv güç ve kapanış akışını ayrı varyantlar olarak tutar.

G1 BLOCKED iken sentetik model, sözleşme, fixture ve otomatik testler hazırlanabilir. Gerçek şematik ve PCB faaliyetleri G1/G2 onayını bekler. Her sonuç SOURCE\_EXPLICIT, DERIVED, ENGINEERING\_RULE, ASSUMPTION, UNKNOWN veya CONFLICT sınıfıyla; PASS, FAIL, UNKNOWN, BLOCKED veya NOT\_RUN durumlarından biriyle raporlanmalıdır.

## Pozisyon haritası

| **Pozisyon** | **Sorumluluk** | **Ana teslim** |
| --- | --- | --- |
| WF-03-01 | Varyant sözleşmeleri | Variant matrix ve schema |
| WF-03-02 | CAN/TWAI | CAN olay motoru ve testler |
| WF-03-03 | LIN | LIN FSM ve negatif testler |
| WF-03-04 | GNSS | GNSS validator ve kaynak matrisi |
| WF-03-05 | Automotive power | Güç senaryo profilleri |
| WF-03-06 | Ignition shutdown | Shutdown FSM ve olay izi |
| WF-03-07 | Varyant test ve release | İki varyantlı MVP ve README |

## Ortak Codex çalışma sözleşmesi

- JOB-04 ve JOB-05 gereksinimlerini ayrı varyantlarda tut; yanlış birleşik profili negatif test olarak ele al.

- Genel rail-budget motorunu yeniden yazma; bu sorumluluğu REP-02 entegrasyonu üzerinden kullan.

- Mevcut dosyaları silme veya üzerine yazma, Git push yapma ve paket kurma işlemlerini kullanıcı onayı olmadan gerçekleştirme.

- Eksik müşteri veya üretici parametresi için sayı uydurarak PASS üretme; UNKNOWN ya da BLOCKED durumunu koru.

- Sentetik CAN, LIN, GNSS, transient veya shutdown sonuçlarını fiziksel ölçüm, EMC/ISO/AEC uygunluğu ya da saha başarısı olarak sunma.

- Her WF sonunda gerçek test logu, rule\_id, reason\_code, source/version, input hash ve tool/package version bildir.

- Aynı kanonik fixture iki çalıştırmada aynı teknik sonucu vermelidir.

## WF 03 01 Varyant Sözleşmeleri

Amaç: JOB-04 CAN/LIN/GNSS ile JOB-05 automotive power/shutdown kapsamlarını ortak çekirdek altında fakat sızıntısız biçimde ayırmak.

Girdiler ve bağımlılıklar: REP-03 kapsamı; kaynak sınıfları; sonuç durumları; REP-02 dosya entegrasyonu; A, B ve yanlış birleşik profil senaryoları.

Beklenen teslimatlar: variant matrix; ortak ve varyanta özel JSON schema; geçerli A/B fixtureları; birleşik profil negatif fixtureı; sözleşme testleri.

### Adım adım Codex görevleri

1. Ortak alanlarla JOB-04 ve JOB-05’e özgü alanları çıkarıp variant matrix oluştur.

2. variant\_id, schema\_version, enabled\_modules, source ve profile\_hash alanlarını ortak sözleşmeye ekle.

3. A varyantını CAN/LIN/GNSS; B varyantını automotive-power/shutdown modülleriyle sınırla.

4. Yanlış birleşik profil, eksik variant\_id, bilinmeyen modül ve sürüm uyuşmazlığı fixtureları üret.

5. Şema doğrulama ve varyant sızıntısı testlerini yaz.

6. D1 sözleşme onayı için kabul edilen alanlar, çatışmalar ve açık GAP özetini çıkar.

### Sınırlar ve blokajlar

Varyant B’ye CAN/LIN/GNSS gereksinimleri, varyant A’ya Raspberry Pi shutdown gereksinimleri taşınmamalıdır. Ortak şema yalnız gerçekten ortak alanları içermelidir. Yanlış birleşik profil sessizce normalize edilmemelidir.

### Pozisyon kabul ölçütü

A ve B ayrı doğrulanır; yanlış birleşik profil FAIL üretir; gereksinim sızıntısı testlerle yakalanır; şema sürümlüdür ve eksik karar girdisi PASS değildir.

### Codex prompt zinciri

#### Prompt 1 Varyant matrisi

REP-03 iş akışından JOB-04 ve JOB-05 gereksinimlerini çıkar. Ortak alanları, yalnız A varyantına ait CAN/LIN/GNSS alanlarını ve yalnız B varyantına ait automotive-power/shutdown alanlarını ayrı tablolaştır. Belirsizlikleri UNKNOWN, çelişkileri CONFLICT olarak göster; dosya oluşturma.

#### Prompt 2 Şema ve fixturelar

Onaylanan matrise göre src/contracts altında sürümlü ortak şema ile A ve B varyant şemalarını oluştur. Geçerli A, geçerli B, yanlış birleşik, eksik variant\_id, bilinmeyen modül ve sürüm uyuşmazlığı fixtureları ekle. Her fixture için beklenen status ve reason\_code tanımla.

#### Prompt 3 Sızıntı testleri ve D1

A profilinde B’ye özgü alanların ve B profilinde A’ya özgü alanların kabul edilmediğini sözleşme testleriyle doğrula. Yanlış birleşik profil FAIL üretmeli. Gerçek komut, araç sürümü, test logu ve input hash ile D1 kanıt özeti hazırla; çalışmayan testi NOT\_RUN yaz.

## WF 03 02 CAN ve TWAI

Amaç: ESP32-S3 TWAI denetleyicisi ile harici CAN transceiver sorumluluklarını ayıran, bus-off ve recovery davranışını modelleyen sentetik olay motoru geliştirmek.

Girdiler ve bağımlılıklar: A varyant sözleşmesi; MCU TWAI olayları; exact transceiver bilgisi varsa üretici kaynağı; normal frame, bus-off ve transceiver-off fixtureları.

Beklenen teslimatlar: CAN olay modeli; controller/transceiver sınır sözleşmesi; bus-off/recovery FSM; pozitif ve negatif testler; kural bazlı sonuç raporu.

### Adım adım Codex görevleri

1. TWAI controller olaylarıyla fiziksel transceiver durumlarını ayrı veri alanlarına bağla.

2. Normal frame, error counter, bus-off, recovery request, recovery complete ve transceiver-off olaylarını tanımla.

3. Bus-off/recovery FSM durum, geçiş ve timeout kurallarını yaz.

4. Normal frame, bus-off, transceiver-off, eksik recovery ve geçersiz sıra fixtureları oluştur.

5. Her ihlali rule\_id, reason\_code, olay izi ve input hash ile raporla.

6. GAP-01 kapanmadıysa exact transceiver power-off davranışını UNKNOWN/BLOCKED bırak.

### Sınırlar ve blokajlar

Sentetik frame ve bus-off sonucu fiziksel CAN katmanı kanıtı değildir. MCU controller davranışı transceiver elektriksel davranışıyla karıştırılmamalıdır. Exact transceiver seçimi ve güç-kapalı davranışı kaynak olmadan kesinleştirilmez.

### Pozisyon kabul ölçütü

Normal olay izi deterministiktir; bus-off ve recovery geçişleri testlidir; transceiver-off senaryosu ayrı değerlendirilir; GAP-01 etkisi saklanmaz; fiziksel CAN iddiası yoktur.

### Codex prompt zinciri

#### Prompt 1 CAN rol sözleşmesi

ESP32-S3 TWAI controller ile harici CAN transceiver görevlerini ayrı sözleşmeler halinde tanımla. Controller state, transceiver state, frame event, error counter, bus-off, recovery ve power-off alanlarını belirle. Üretici kaynağı bulunmayan transceiver davranışını UNKNOWN bırak.

#### Prompt 2 Bus off motoru

src/can altında deterministik bus-off ve recovery FSM geliştir. Her geçiş olay, önceki/yeni durum, timeout, status ve reason\_code taşısın. Controller ile transceiver durumlarını tek bayrakta birleştirme. Normal frame, bus-off ve transceiver-off fixturelarını destekle.

#### Prompt 3 CAN negatif testleri

Normal frame, error escalation, bus-off, recovery success, recovery timeout, transceiver-off ve geçersiz olay sırası testlerini çalıştır. Aynı fixtureı iki kez çalıştırıp aynı teknik sonucu doğrula. Sonuçları sentetik protokol doğrulaması olarak etiketle; fiziksel CAN kanıtı deme.

## WF 03 03 LIN

Amaç: UART ile LIN transceiver ayrımını, wake/sleep durumlarını ve timeout davranışını kaynak sınıflarıyla modellemek.

Girdiler ve bağımlılıklar: A varyant sözleşmesi; UART/LIN transceiver sınırları; wake, sleep, stuck-dominant ve timeout fixtureları; varsa exact transceiver datasheeti.

Beklenen teslimatlar: LIN FSM; olay sözleşmesi; wake/sleep ve timeout kuralları; stuck-dominant negatif testleri; sonuç raporu.

### Adım adım Codex görevleri

1. UART çevre birimi, LIN transceiver ve bus durumu sorumluluklarını ayır.

2. Sleep, wake-request, awake, frame-wait, timeout ve fault durumlarını tanımla.

3. Wake kaynağı, süre, timeout ve stuck-dominant gözlemlerini olay sözleşmesine ekle.

4. Normal wake/sleep, wake timeout, stuck-dominant, eksik transceiver ve hızlı yeniden uyku fixtureları oluştur.

5. Exact parçaya bağlı davranışları source reference ile bağla; kaynak yoksa UNKNOWN üret.

6. FSM geçişlerini ve negatif senaryoları otomatik testle doğrula.

### Sınırlar ve blokajlar

Standart LIN davranışı ile exact transceiver güç-kapalı/wake davranışı aynı şey değildir. Datasheet bulunmayan eşik veya gecikme uydurulmaz. UART başarısı fiziksel LIN uygunluğu sayılmaz.

### Pozisyon kabul ölçütü

Wake/sleep FSM deterministiktir; stuck-dominant ve timeout reason\_code üretir; bilinmeyen parça davranışı UNKNOWN’dur; sentetik sonuç fiziksel LIN iddiasına dönüşmez.

### Codex prompt zinciri

#### Prompt 1 LIN durum sözleşmesi

UART, LIN transceiver ve bus sorumluluklarını ayırarak sleep, wake-request, awake, frame-wait, timeout ve stuck-dominant durumlarını tanımla. Exact transceiver gerektiren eşik/gecikmeleri kaynak alanına bağla; kaynak yoksa UNKNOWN bırak.

#### Prompt 2 LIN FSM

src/lin altında wake/sleep ve timeout odaklı deterministik FSM geliştir. Olay izinde önceki/yeni durum, event, elapsed time, threshold source, status ve reason\_code bulunsun. UART seviyesindeki olayları fiziksel LIN doğrulaması gibi sunma.

#### Prompt 3 LIN sınır testleri

Normal wake, normal sleep, wake timeout, stuck dominant, eksik transceiver profili ve hızlı yeniden uyku fixturelarını test et. Exact parça davranışı bilinmiyorsa beklenen sonuç UNKNOWN/BLOCKED olmalı. Gerçek test loglarını ve GAP-01 etkisini raporla.

## WF 03 04 GNSS

Amaç: GNSS modül, anten/RF, fix-quality, stale data ve güç profili bilgilerini sentetik konum üretmeden doğrulayan validator geliştirmek.

Girdiler ve bağımlılıklar: A varyant sözleşmesi; GNSS modül varyantı; anten ve RF entegrasyon kaynağı; fix-quality ve timestamp sözleşmesi; no-fix, stale-data ve anten arızası fixtureları.

Beklenen teslimatlar: GNSS validator; kaynak matrisi; fix ve freshness kuralları; anten/RF risk kaydı; pozitif/negatif testler.

### Adım adım Codex görevleri

1. Modül, anten, RF feed, güç profili, fix-quality ve veri zamanı alanlarını şemaya bağla.

2. No-fix, valid-fix, stale-data, timestamp rollback ve antenna-fault durumlarını tanımla.

3. Freshness eşiğini kaynaklı veya açık ASSUMPTION olarak kaydet.

4. GNSS verisini sentetik payload ile test et; gerçek konum ölçümü iddiasından ayır.

5. GAP-02 için modül entegrasyon kılavuzu, anten tipi ve RF yerleşim kanıtını kaynak matrisinde izle.

6. Validator sonuçlarını kural bazlı raporla ve tekrarlanabilirliği doğrula.

### Sınırlar ve blokajlar

Sentetik koordinat ölçüm değildir. Anten/RF yerleşim uygunluğu yalnız veri şemasıyla kanıtlanamaz. Modül varyantı ve entegrasyon kılavuzu eksikse RF kararları BLOCKED kalır.

### Pozisyon kabul ölçütü

No-fix ve stale-data PASS değildir; freshness kuralı kaynaklıdır veya ASSUMPTION etiketi taşır; anten arızası ayrı reason\_code üretir; sentetik veri gerçek ölçüm gibi sunulmaz.

### Codex prompt zinciri

#### Prompt 1 GNSS sözleşmesi ve kaynak matrisi

GNSS modül varyantı, anten tipi, RF feed, güç profili, fix\_quality, timestamp, age ve antenna\_status alanlarını tanımla. Her alanı üretici kaynağı, türetilmiş kural, varsayım veya UNKNOWN sınıfına bağlayan kaynak matrisi hazırla.

#### Prompt 2 GNSS validator

src/gnss altında no-fix, valid-fix, stale-data, timestamp rollback ve antenna-fault durumlarını değerlendiren deterministik validator geliştir. Çıktıda rule\_id, status, reason\_code, threshold source, input\_hash ve synthetic\_data etiketi bulunsun.

#### Prompt 3 GNSS negatif testleri

No-fix, stale-data sınırının altı/üstü, timestamp rollback, anten arızası, eksik modül varyantı ve geçerli sentetik fix fixturelarını test et. Sentetik koordinatı ölçüm gibi sunma. GAP-02 kapanmadıysa anten/RF uygunluğunu BLOCKED bırak.

## WF 03 05 Automotive Power

Amaç: JOB-05 için 12 V girişten 5.1 V/5 A hedefe uzanan cold-crank, load-dump, jump-start ve brownout senaryolarını REP-02 ile uyumlu profillere dönüştürmek.

Girdiler ve bağımlılıklar: B varyant sözleşmesi; müşteri/OEM transient profili; 12 V giriş ve 5.1 V/5 A hedefi; REP-02 sürümlü dosya API; suppressed/unsuppressed ve brownout senaryoları.

Beklenen teslimatlar: Automotive power scenario profiles; REP-02 entegrasyon fixtureları; GAP-03 karar kaydı; kural bazlı sonuç raporu.

### Adım adım Codex görevleri

1. 12 V giriş, 5.1 V/5 A hedef ve transient olay alanlarını B varyant şemasına bağla.

2. Cold-crank, load-dump, jump-start, brownout ve nominal profilleri ayrı senaryo dosyaları olarak tanımla.

3. Suppressed ve unsuppressed profilleri karıştırmadan source/version alanlarıyla kaydet.

4. Profilleri REP-02 rail-budget/sequencing giriş sözleşmesine dönüştüren adaptörü tanımla.

5. OEM profili yoksa exact-part ve gerçek geçiş seviyesi sonuçlarını BLOCKED bırak.

6. Normal, sınır, eksik kaynak ve uyumsuz REP-02 sürümü testlerini çalıştır.

### Sınırlar ve blokajlar

Genel rail-budget motoru REP-02 sorumluluğundadır; burada yeniden uygulanmaz. OEM/müşteri profili olmadan transient genlik ve süreleri nihai gerçek gereksinim sayılmaz. Sentetik profil otomotiv uygunluğu değildir.

### Pozisyon kabul ölçütü

Her güç senaryosu sürümlü ve kaynak sınıflıdır; REP-02 arayüzü bağımsız modül sınırını korur; OEM profili yoksa exact-part BLOCKED; sentetik sonuç sertifikasyon iddiası taşımaz.

### Codex prompt zinciri

#### Prompt 1 Güç senaryo sözleşmesi

JOB-05 için nominal, cold-crank, load-dump, jump-start ve brownout senaryo alanlarını tanımla. Suppressed ve unsuppressed profilleri ayır. 12 V giriş ve 5.1 V/5 A hedefini gereksinim olarak kaydet; OEM kaynaklı olmayan genlik/süreleri ASSUMPTION veya UNKNOWN yap.

#### Prompt 2 REP 02 adaptörü

REP-03 automotive-power profillerini REP-02 rail-budget/sequencing dosya API girişlerine dönüştüren sürümlü adaptör sözleşmesi hazırla. REP-02 motorunu kopyalama. Schema version, units, source, profile\_hash ve uyumsuzluk davranışını tanımla.

#### Prompt 3 Güç profili testleri

Nominal, cold-crank, load-dump suppressed/unsuppressed, jump-start, brownout, eksik OEM profili ve uyumsuz REP-02 sürümü fixturelarını test et. OEM profili yokken exact-part kararının BLOCKED kaldığını doğrula; sentetik sonucu otomotiv uygunluğu olarak sunma.

## WF 03 06 Ignition Shutdown

Amaç: Ignition-off, host notification, ACK, grace period ve power-cut adımlarını Raspberry Pi sözleşmesine bağlı deterministik FSM olarak modellemek.

Girdiler ve bağımlılıklar: B varyant sözleşmesi; Raspberry Pi modeli ve onaylı host protokolü; ignition-off, ACK, grace-period, power-cut ve restart olayları.

Beklenen teslimatlar: Shutdown FSM; host notification sözleşmesi; olay izi; ACK yok, erken kesme ve hızlı restart negatif testleri.

### Adım adım Codex görevleri

1. Ignition-on, notify-pending, wait-ack, grace-period, ready-to-cut, power-cut ve restart durumlarını tanımla.

2. Host notification, ACK, timeout, cut command ve restart olaylarını sürümlü sözleşmeye bağla.

3. Grace period ve timeout değerlerini source reference veya ASSUMPTION sınıfıyla kaydet.

4. Normal shutdown, ACK yok, geç ACK, erken power-cut, hızlı restart ve duplicate ACK fixtureları oluştur.

5. Her geçişi beklenen/gözlenen olay, elapsed time, status ve reason\_code ile raporla.

6. GAP-04 kapanmadıysa Raspberry Pi protokolüne bağlı kararları UNKNOWN/BLOCKED bırak.

### Sınırlar ve blokajlar

Raspberry Pi modeli ve kapanış protokolü bilinmiyorsa güvenli shutdown kanıtı verilemez. Sentetik ACK gerçek işletim sistemi kapanışını kanıtlamaz. Güç kesme eylemi fiziksel donanım kontrolü değildir.

### Pozisyon kabul ölçütü

Normal shutdown deterministiktir; ACK yok/erken kesme/hızlı restart ayrı reason\_code üretir; protokol eksikliği UNKNOWN/BLOCKED’dur; olay izi tüm kararları açıklar.

### Codex prompt zinciri

#### Prompt 1 Shutdown protokol sözleşmesi

Ignition-off, host notify, ACK, grace period, ready-to-cut, power-cut ve restart olaylarını tanımla. Raspberry Pi modeli ve host protokolü kaynak alanlarını zorunlu kıl. Bilinmeyen timeout ve grace period değerlerini uydurma; ASSUMPTION/UNKNOWN olarak işaretle.

#### Prompt 2 Shutdown FSM

src/shutdown altında deterministik ignition shutdown FSM geliştir. Her geçişte previous\_state, event, next\_state, elapsed\_time, threshold\_source, status ve reason\_code üret. Sentetik ACK olayını gerçek OS shutdown kanıtı olarak sunma.

#### Prompt 3 Shutdown negatif testleri

Normal ACK, ACK yok, geç ACK, grace period dolmadan power cut, duplicate ACK, ignition bounce ve hızlı restart fixturelarını test et. Raspberry Pi protokolü eksikse ilgili sonuç UNKNOWN/BLOCKED olmalı. Gerçek test logu ve GAP-04 etkisini raporla.

## WF 03 07 Varyant Test ve Release

Amaç: A ve B varyantlarını ayrı çalıştıran CLI, CI, raporlama ve temiz kurulum akışıyla tekrarlanabilir iki varyantlı MVP paketi hazırlamak.

Girdiler ve bağımlılıklar: WF-03-01 ile WF-03-06 kabul edilmiş çıktıları; sentetik fixturelar; evidence reporter; README, lisans ve manifest gereksinimi.

Beklenen teslimatlar: A/B varyant CLI komutları; CI; temiz kurulum logu; README/quickstart; source/evidence register; sürüm/hash manifesti; iki varyantlı MVP.

### Adım adım Codex görevleri

1. CLI komutlarını validate, can, lin, gnss, automotive-power, shutdown ve report görevlerine bağla.

2. A ve B için ayrı komut/profile akışları ve yanlış birleşik profil hata davranışı tanımla.

3. Pozitif, negatif, eksik veri, tekrarlanabilirlik ve REP-02 entegrasyon testlerini CI üzerinde çalıştır.

4. README ve quickstart içinde iki varyantı, sentetik kapsamı ve sınırlamaları açıkla.

5. Temiz ortamda kurulum ve örnek komutları gerçekten çalıştır; gerçek log üret.

6. Lisans, source/evidence register, karar/kısıt raporu ve future field-data adapter notlarını ekle.

7. D4 release manifesti, output hashleri ve NOT\_RUN kalemleriyle MVP teslim özeti üret.

### Sınırlar ve blokajlar

A ve B tek birleşik ürün gibi paketlenmemelidir. Temiz kurulum çalıştırılmadan PASS verilmez. Paket kurulumu, Git commit/push veya release yayımlama kullanıcı onayı gerektirir. Otomotiv sertifikası iddiası yoktur.

### Pozisyon kabul ölçütü

A ve B tek tek tekrar üretilebilir; yanlış birleşik profil FAIL üretir; CI gerçek log taşır; README komutları temiz ortamda çalışır; release manifesti ve hashler eksiksizdir; sertifikasyon iddiası yapılmaz.

### Codex prompt zinciri

#### Prompt 1 CLI ve varyant komutları

validate, can, lin, gnss, automotive-power, shutdown ve report komutları için CLI sözleşmesi tasarla. A ve B profile seçimini açık yap; yanlış birleşik profili reddet. Her komutun giriş/çıkış, stdout/stderr, exit-code ve örnek kullanımını tanımla.

#### Prompt 2 CI ve dokümantasyon

Onaylanan sözleşmeye göre CLI katmanını oluştur; modülleri kopyalamadan çağır. A/B pozitif testleri, yanlış birleşik profil, eksik veri, negatif senaryolar, tekrarlanabilirlik ve REP-02 entegrasyonunu CI akışına ekle. README ve quickstartta sentetik kapsamı ve sınırlamaları yaz.

#### Prompt 3 Temiz kurulum ve D4

Temiz ortamda README kurulum ve A/B quickstart adımlarını gerçekten çalıştır. Paket kurma veya Git işlemi gerekiyorsa önce kullanıcı onayı iste. Araç sürümleri, gerçek test logları, FAIL/NOT\_RUN kalemleri, lisans, evidence register ve SHA-256 manifestiyle iki varyantlı D4 MVP teslim özeti üret; otomotiv sertifikası iddiası ekleme.

## Oturum ve kapı planı

| **Oturum** | **Codex kapsamı** | **Kapı veya sonuç** |
| --- | --- | --- |
| 1 | WF-03-01 varyant ve ürün sözleşmeleri; araştırma boşlukları | D1 sözleşme onayı |
| 2 | WF-03-02 CAN/TWAI ve WF-03-03 LIN çekirdek motorları | D2 sentetik uygulama parçası |
| 3 | WF-03-04 GNSS ve WF-03-05 automotive-power profilleri | D2 sentetik uygulama tamamlanır |
| 4 | WF-03-06 shutdown ve evidence raporlama | D3 test ve kanıt başlangıcı |
| 5 | Varyant negatif testleri ve REP-02 entegrasyonu | D3 test ve kanıt kapanışı |
| 6 | WF-03-07 temiz kurulum, CI ve bağımsız inceleme | D4 iki varyantlı MVP |

Bu oturumlar süre taahhüdü değildir. Donanım G1-G6 kapıları ayrı izlenir; yazılım D1-D4 kapıları fiziksel donanım onayı veya otomotiv sertifikasyonu yerine geçmez.

## Görev kapanış raporu şablonu

- Pozisyon, varyant ve görev kimliği

- Sonuç durumu: PASS / FAIL / UNKNOWN / BLOCKED / NOT\_RUN

- Oluşturulan veya değiştirilen dosyalar

- Çalıştırılan komutlar, araçlar ve sürümler

- Test sayısı, başarısız testler ve gerçek log konumu

- Kaynak revizyonları ile input/output hashleri

- Açık GAP-01…04 etkileri ve Claude araştırma ihtiyacı

- Bir sonraki görev ve başlaması için gereken kapı

## Sonuç

Yürütme WF-03-01 varyant sözleşmeleriyle başlamalıdır. CAN/TWAI, LIN ve GNSS yalnız A varyantına; automotive power ve ignition shutdown yalnız B varyantına bağlanmalıdır. Genel rail-budget işlevi REP-02 üzerinden sürümlü dosya API ile tüketilir. G1 BLOCKED gerçek şematik ve PCB faaliyetlerini engeller; sentetik yazılım görevlerini durdurmaz. İki varyant bağımsız çalıştırılmadan ve yanlış birleşik profil negatif testi geçmeden MVP release tamamlanmış sayılmaz.
