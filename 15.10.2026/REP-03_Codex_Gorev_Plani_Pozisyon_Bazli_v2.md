**REP 03 Codex Görev Planlaması**

REP-03 | ESP32-S3 Automotive Interfaces

ESP32-S3 Automotive Interfaces için pozisyon bazlı görev ve prompt raporu

Kaynak: REP-03 İş Akışı Raporu v2.0 | JOB-04 / JOB-05 | Tarih: 26.10.2026 | v1 (17.10.2026) yerine geçer | Repo: github.com/kemaldemirag/esp32-s3-automotive-interfaces

# Amaç ve uygulama sınırı

Bu rapor, WF-03-01 ile WF-03-07 arasındaki yedi pozisyonu korur. v1 görevleri sentetik kapsamda tamamlandı. v2 görevleri yeni pozisyon açmadan mevcut pozisyonlara yerleştirilmiştir: şema v2 (WF-03-01), referans aktarımları (WF-03-02/03/05/06), lisans/uzak CI (WF-03-07). JOB-04 (Varyant A) ve JOB-05 (Varyant B) ayrımı korunur.

Her sonuç SOURCE\_EXPLICIT, DERIVED, ENGINEERING\_RULE, ASSUMPTION, UNKNOWN, CONFLICT, REFERENCE\_DESIGN veya PORTFOLIO\_DECISION sınıfıyla; PASS, FAIL, UNKNOWN, BLOCKED veya NOT\_RUN durumlarından biriyle raporlanır.

# Pozisyon haritası

| **Pozisyon** | **Sorumluluk** | **Ana teslim** | **Durum (26.10)** |
| --- | --- | --- | --- |
| WF-03-01 | Varyant sözleşmeleri | Şema v2 (yeni sınıf ve yetki) | D1 tamam; v2 başlamadı |
| WF-03-02 | CAN/TWAI | Referans transceiver kaydı | Tamam (sentetik) |
| WF-03-03 | LIN | TJA1020/1021 referans karşılaştırması | Tamam (sentetik) |
| WF-03-04 | GNSS | Değişiklik yok; GAP-02 açık | Tamam (sentetik) |
| WF-03-05 | Automotive power | TIDA-01167 profili; REP-02 ortak dilim | Tamam; canlı adaptör NOT\_RUN |
| WF-03-06 | Ignition shutdown | RaspPowerHat aktarımı; zorunlu kesme kararı | Patch hazır |
| WF-03-07 | Varyant test ve release | Lisans, uzak CI, release | D4 yerel PASS |

# Ortak Codex çalışma sözleşmesi

- Repodaki AGENTS.md dosyasını oku; yoksa ilk görevde oluştur (durum kodları, PYTHONPATH=firmware python -m unittest discover -s tests, dokunulmayacak dosyalar, şema sürüm kuralı).

- JOB-04 ve JOB-05 gereksinimlerini ayrı varyantlarda tut; yanlış birleşik profili negatif test olarak ele al.

- Genel rail-budget motorunu yeniden yazma; REP-02 entegrasyonunu kullan.

- Mevcut dosyaları silme veya üzerine yazma, Git push yapma ve paket kurma işlemlerini kullanıcı onayı olmadan gerçekleştirme.

- Eksik müşteri veya üretici parametresi için sayı uydurarak PASS üretme; UNKNOWN ya da BLOCKED durumunu koru. GAP-04 motor kapısını kapatmaya çalışma.

- Referans tasarım kullanırken sabit commit, SHA-256, lisans ve satır referansını references/<ad>/reference.lock.json içine yaz; değerleri script ile çıkar; referans senaryolarını mevcut fixture dizinlerine karıştırma; iki çalıştırmada bayt bayt aynı çıktıyı doğrula. GPL lisanslı kaynaklardan kod veya tasarım kopyalama.

- Sentetik veya referans CAN, LIN, GNSS, transient ya da shutdown sonuçlarını fiziksel ölçüm, EMC/ISO/AEC uygunluğu veya saha başarısı olarak sunma.

- Her WF sonunda gerçek test logu, rule\_id, reason\_code, source/version, input hash ve tool/package version bildir.

# WF 03 01 Varyant Sözleşmeleri

**Amaç:** Varyant ayrımını koruyarak kaynak sınıfı ve yetki listelerini yeni şema sürümüyle genişletmek.

**Girdiler ve bağımlılıklar:** Mevcut variant ve senaryo şemaları; motorlardaki SOURCE\_CLASSIFICATIONS ve SOURCE\_AUTHORITIES tanımları; REP-01 İş Akışı v2 kaynak sınıfları.

**Beklenen teslimatlar:** Şema v2 dosyaları; motorlarda v2 desteği; v1/v2 fixture'ları; uyumluluk testleri; docs/decisions/ karar kaydı şablonu.

**Durum (26.10):** D1 tamam; v2 başlamadı.

## Adım adım Codex görevleri

1. Kaynak sınıfı ve yetki enum'larının geçtiği tüm şema ve motor dosyalarını listele.

2. REFERENCE\_DESIGN, PORTFOLIO\_DECISION ve REFERENCE yetkisi için zorunlu provenans alanlarını tanımla.

3. Yeni şema sürümünü ekle; REFERENCE yetkisinin PASS değil ayrı bir durum ürettiği karar kuralını yaz.

4. v1 fixture'larının v1 olarak doğrulanmaya devam ettiğini test et.

5. Karar kaydı şablonunu ekle (kullanıcı doldurur).

## Sınırlar ve blokajlar

v1 şemaları değiştirilmez; yeni sürüm eklenir. Varyant sızıntısı yasakları korunur. REFERENCE yetkisi PROJECT\_REQUIREMENT veya HOST\_PROTOCOL yerine geçmez.

## Pozisyon kabul ölçütü

A ve B ayrı doğrulanır; v1 fixture'ları kırılmaz; REFERENCE yetkisi ayrı durum üretir; tüm testler geçer.

## Codex prompt zinciri

### Prompt 1 Varyant matrisi

Kaynak sınıfı ve yetki enum'larının kullanıldığı tüm şema ve motor noktalarını listele. Yeni sınıfların ve REFERENCE yetkisinin her motorun karar kuralına etkisini tablolaştır. Dosya değiştirme.

### Prompt 2 Şema ve fixturelar

Yeni şema sürümünü ekle; motorlara v2 desteği ver; REFERENCE yetkisi için ayrı durum kuralını uygula. v1 ve v2 geçerli/eksik provenanslı fixture'ları ekle; beklenen status ve reason\_code değerlerini tanımla.

### Prompt 3 Sızıntı testleri ve D1

Varyant sızıntı testlerini ve v1 uyumluluğunu yeniden çalıştır; yeni sınıfların provenans zorunluluğunu testle doğrula. Gerçek komut, sürüm, test sayısı ve log ile v2 sözleşme kanıtını üret.

# WF 03 02 CAN ve TWAI

**Amaç:** GAP-01'in CAN kısmını referans tasarımdaki transceiver ile REFERENCE\_DESIGN olarak ilerletmek.

**Girdiler ve bağımlılıklar:** Mevcut CAN motoru; RejsaCAN-ESP32 ve vandal referansları; transceiver datasheet'i.

**Beklenen teslimatlar:** references/<ad>/ lock dosyası; transceiver kaynak kaydı; transceiver-off senaryosunun referans eşikli versiyonu.

**Durum (26.10):** WF tamam (sentetik).

## Adım adım Codex görevleri

1. Referans repoları sabit commit'te aktar; lisansları kaydet.

2. Transceiver parça numarasını şematik/BOM dosyasından script ile çıkar.

3. Datasheet'ten power-off ve standby davranışını sayfa referansıyla kaydet (Claude doğrular).

4. Transceiver-off senaryosunu referans kaynakla yeniden üret ve motoru çalıştır.

## Sınırlar ve blokajlar

Sentetik frame ve bus-off sonucu fiziksel CAN kanıtı değildir. GPL lisanslı şematikten yalnız parça numarası gibi veri çıkarılır.

## Pozisyon kabul ölçütü

Transceiver kaydı commit ve sayfa referanslı; senaryo tekrar üretilebilir; motor kodu değişmeden çalışır.

## Codex prompt zinciri

### Prompt 1 CAN rol sözleşmesi

RejsaCAN-ESP32 ve vandal repolarının lisanslarını ve CAN transceiver bilgisini içeren dosyaları listele; hangisinin referans olarak uygun olduğunu öner. Dosya değiştirme.

### Prompt 2 Bus off motoru

Seçilen referansı sabit commit'te references/ altına lock dosyasıyla bağla; transceiver parça numarasını script ile çıkar; transceiver-off senaryosunun referans kaynaklı versiyonunu üret. Motoru değiştirme.

### Prompt 3 CAN negatif testleri

Referans senaryosunu şema doğrulamasından geçir, motoru çalıştır, raporu kaydet; script'in deterministik olduğunu test et. GAP-01 kaydını güncelle; fiziksel CAN iddiası ekleme.

# WF 03 03 LIN

**Amaç:** Sentetik LIN FSM'yi açık kaynak ESP32 LIN kütüphanelerinin TJA1020/TJA1021 durum makinesiyle karşılaştırmak.

**Girdiler ve bağımlılıklar:** Mevcut LIN motoru; mestrode/Lin-Interface-Library, mestrode/Lin-Transceiver-Library, macchina/LIN; TJA1020/TJA1021 datasheet'leri.

**Beklenen teslimatlar:** references/ lock dosyaları; durum ve geçiş eşleme tablosu; uyumsuzluk raporu; referans kaynaklı senaryolar.

**Durum (26.10):** WF tamam (sentetik).

## Adım adım Codex görevleri

1. Kütüphaneleri sabit commit'te aktar; lisanslarını kaydet.

2. Transceiver durum makinesini (sleep, normal, standby, wake) kaynak koddan ve datasheet'ten çıkar.

3. Mevcut FSM durumlarıyla eşleme tablosu üret.

4. Uyumsuz geçişler için referans senaryolar üret ve motoru çalıştır.

## Sınırlar ve blokajlar

Kütüphane davranışı standart veya datasheet yerine geçmez; ikisi ayrı kaynak olarak kaydedilir. UART başarısı fiziksel LIN uygunluğu sayılmaz.

## Pozisyon kabul ölçütü

Eşleme tablosu kaynak satırlarına bağlı; uyumsuzluklar açık reason\_code ile raporlanmış; motor kodu değişmeden.

## Codex prompt zinciri

### Prompt 1 LIN durum sözleşmesi

Üç kütüphanenin lisansını ve transceiver durum makinesini tanımlayan dosya/satırları listele; TJA1020/TJA1021 durumlarını mevcut FSM durumlarıyla eşleyen taslak tablo üret. Dosya değiştirme.

### Prompt 2 LIN FSM

Referansları references/ altına lock dosyalarıyla bağla; eşleme tablosunu kaynak satırlarıyla tamamla; uyumsuz geçişleri gösteren referans senaryolarını üret ve mevcut motorla çalıştır.

### Prompt 3 LIN sınır testleri

Referans senaryolarının şema uyumunu ve tekrar üretimini test et. Uyumsuzluk raporunu yaz; FSM'de değişiklik gerekiyorsa öneri olarak listele, uygulama. GAP-01 kaydını güncelle.

# WF 03 04 GNSS

**Amaç:** GNSS validator'ı korumak ve GAP-02 için referans ihtiyacını kayda geçirmek.

**Girdiler ve bağımlılıklar:** Mevcut GNSS validator; GAP-02 kaydı.

**Beklenen teslimatlar:** GAP-02 için aranan referans türünün tanımı (modül entegrasyon kılavuzu, anten yerleşimi).

**Durum (26.10):** WF tamam (sentetik); rf\_suitability BLOCKED.

## Adım adım Codex görevleri

1. Mevcut testlerin geçtiğini doğrula.

2. GAP-02'yi kapatabilecek kaynak türünü ve kabul ölçütünü kayda geçir.

3. Referans seçilene kadar başka değişiklik yapma.

## Sınırlar ve blokajlar

Sentetik koordinat ölçüm değildir. Referans olmadan RF kararları BLOCKED kalır.

## Pozisyon kabul ölçütü

Testler geçer; GAP-02 kaydı aranan kaynağı açıkça tanımlar.

## Codex prompt zinciri

### Prompt 1 GNSS sözleşmesi ve kaynak matrisi

Kaynak matrisindeki GAP-02'ye bağlı alanları listele ve kapanış için gereken belge türünü tanımla. Dosya değiştirme.

### Prompt 2 GNSS validator

Validator'da değişiklik yapma; yalnız mevcut testleri çalıştır ve sonucu kaydet.

### Prompt 3 GNSS negatif testleri

Mevcut negatif testlerin sonucunu raporla; GAP-02 kaydını aranan kaynak türüyle güncelle.

# WF 03 05 Automotive Power

**Amaç:** Varyant B transient profillerini referans tasarım kaynağıyla etiketlemek ve REP-02 referans dilimiyle ortak kaynak kullanmak.

**Girdiler ve bağımlılıklar:** Mevcut power profilleri; TI TIDA-01167 (kullanıcı indirir); REP-02 referans dikey dilimi; INT-02A bağımlılık kaydı.

**Beklenen teslimatlar:** Referans kaynaklı load-dump/cold-crank profilleri; REP-02 ile ortak referans kaydı; 5 A hedefine uygun referans bulunamadığına dair kayıt.

**Durum (26.10):** WF tamam; INT-02A offline PASS; canlı adaptör NOT\_RUN.

## Adım adım Codex görevleri

1. TIDA-01167 dokümanının revizyonunu ve hash'ini sources/ altına kaydet.

2. Load-dump ve diğer transient değerlerini sayfa referansıyla çıkar; suppressed/unsuppressed ayrımını koru.

3. Profillerin referans kaynaklı versiyonlarını üret ve power motorunu çalıştır.

4. REP-02 referans dilimi hazırsa aynı kaynağı paylaş; değilse NOT\_RUN.

## Sınırlar ve blokajlar

Referans profil OEM profili değildir; exact-part kararı BLOCKED kalır. RaspPowerHat güç katı (2 A) 5.1 V/5 A hedefi için referans alınmaz.

## Pozisyon kabul ölçütü

Her profil kaynak sınıflı ve sayfa referanslı; REP-02 sınırı korunur.

## Codex prompt zinciri

### Prompt 1 Güç senaryo sözleşmesi

Mevcut power profillerindeki değerleri ve kaynak sınıflarını listele; TIDA-01167 ile karşılanabilecek olanları işaretle. Doküman yoksa kullanıcıdan istenecek dosyayı belirt ve NOT\_RUN raporla.

### Prompt 2 REP 02 adaptörü

Referans kaynaklı profilleri üret; REP-02 bağımlılık kaydını ve adaptör sözleşmesini değiştirmeden çalıştır. REP-02 motorunu kopyalama.

### Prompt 3 Güç profili testleri

Referans profillerin şema uyumunu ve tekrar üretimini test et; exact-part kararının BLOCKED kaldığını doğrula; otomotiv uygunluğu iddiası ekleme.

# WF 03 06 Ignition Shutdown

**Amaç:** RaspPowerHat referans aktarımını repoya almak ve bulunan model boşluğunu karara bağlamak.

**Girdiler ve bağımlılıklar:** Patch: 0001-feat-import-RaspPowerHat-as-pinned-reference-for-Var.patch (commit 61b4b00); RaspPowerHat @ 1cca12b (MIT).

**Beklenen teslimatlar:** references/rasppowerhat/ (lock, 4 senaryo, 4 rapor); import script'i; 3 test; kullanıcı kararından sonra FSM v2 (zorunlu kesme yolu).

**Durum (26.10):** Patch hazır, uygulanmadı. Bulgular: host protokolü BLOCKED → UNKNOWN/PASS(benimsenirse); zamanlama UNKNOWN (grace ve restart türetilmiş); ACK zaman aşımından sonra zorunlu kesme FSM'de yok → EARLY\_POWER\_CUT.

## Adım adım Codex görevleri

1. Patch'i uygula (git am) ve tüm testleri çalıştır.

2. Kullanıcının host protokolü benimseme kararını PORTFOLIO\_DECISION olarak kaydet.

3. Kullanıcı onaylarsa FSM'e yeni şema sürümüyle 'ACK zaman aşımı → zorunlu kesme' geçişini ekle; mevcut EARLY\_POWER\_CUT davranışını koru.

4. Referans senaryoları yeniden üret ve farkları raporla.

## Sınırlar ve blokajlar

Sentetik ACK gerçek OS kapanışını, sentetik cut fiziksel güç kesmeyi kanıtlamaz. GAP-04 motor kapısı kapatılmaz. FSM değişikliği kullanıcı kararı olmadan yapılmaz.

## Pozisyon kabul ölçütü

Patch uygulanmış ve testler geçiyor; karar kaydı mevcut; FSM değişikliği yapıldıysa v1 senaryoları aynı sonucu verir.

## Codex prompt zinciri

### Prompt 1 Shutdown protokol sözleşmesi

Patch'i uygula ve tüm testleri çalıştır. references/rasppowerhat/ raporlarını özetle; host protokolü ve zamanlama bağımlılıklarının durumlarını listele.

### Prompt 2 Shutdown FSM

Yalnız kullanıcı kararı kayıtlıysa: yeni şema sürümünde WAIT\_ACK + timeout → READY\_TO\_CUT (FORCED\_CUT\_AFTER\_ACK\_TIMEOUT) geçişini ekle. v1 senaryolarının sonuçlarını değiştirme. Karar yoksa NOT\_RUN raporla.

### Prompt 3 Shutdown negatif testleri

Referans senaryolarını yeniden üret; v1 ve v2 FSM sonuçlarını karşılaştıran testleri ekle; GAP-04 dependency durumunu raporla; güvenli shutdown iddiası ekleme.

# WF 03 07 Varyant Test ve Release

**Amaç:** Lisans, uzak CI ve referans aktarımlarıyla iki varyantlı MVP'yi dışarıdan doğrulanabilir hale getirmek.

**Girdiler ve bağımlılıklar:** D4 yerel release adayı; referans aktarımları; kullanıcının lisans kararı.

**Beklenen teslimatlar:** LICENSE ve SPDX; CI'da referans tekrar üretim işi; uzak CI logu; güncel README.

**Durum (26.10):** D4 yerel PASS; LICENSE yok; uzak CI NOT\_RUN.

## Adım adım Codex görevleri

1. Kullanıcının seçtiği lisansı uygula.

2. CI'a referans aktarımlarının tekrar üretimini ekle (ağ yoksa lock dosyası ve kayıtlı raporlarla doğrulama).

3. README'ye referans bulgularını ve sınırlamaları ekle.

4. Kullanıcı push ettikten sonra uzak CI sonucunu kaydet; release notu taslağını hazırla.

## Sınırlar ve blokajlar

A ve B tek birleşik ürün gibi paketlenmez. Lisans Codex tarafından seçilmez. Push ve yayımlama kullanıcıdadır. Otomotiv sertifikası iddiası yoktur.

## Pozisyon kabul ölçütü

Uzak CI yeşil; lisans açık; referans raporları CI'da tekrar üretiliyor.

## Codex prompt zinciri

### Prompt 1 CLI ve varyant komutları

Mevcut CLI komutlarını README ile karşılaştır; referans senaryolarını CLI ile çalıştırma yöntemini belgele.

### Prompt 2 CI ve dokümantasyon

Kullanıcının seçtiği lisansı uygula; CI'a referans tekrar üretim işini ekle; README'ye referans bulgularını ve sınırlamaları yaz.

### Prompt 3 Temiz kurulum ve D4

Temiz ortamda kurulum ve A/B quickstart'ı çalıştır; uzak CI sonucunu kullanıcı push ettikten sonra kaydet; release notu taslağını hazırla; sertifikasyon iddiası ekleme.

# Oturum ve kapı planı

| **Oturum** | **Codex kapsamı** | **Kapı veya sonuç** |
| --- | --- | --- |
| 1 | AGENTS.md; WF-03-06 patch uygulaması; WF-03-01 şema v2 | D1 v2 |
| 2 | Kullanıcı kararları; WF-03-06 FSM v2 (onaylıysa) | Shutdown v2 |
| 3 | WF-03-03 LIN referans karşılaştırması | LIN uyumsuzluk raporu |
| 4 | WF-03-02 CAN referans transceiver; WF-03-05 TIDA-01167 profili | GAP-01/03 ilerlemesi |
| 5 | WF-03-04 GAP-02 kaydı; REP-02 ortak dilim | Kaynak matrisi güncel |
| 6 | WF-03-07 lisans, uzak CI, release | D5 referans aktarımlı MVP |

Bu oturumlar süre taahhüdü değildir. Donanım G1-G6 kapıları ayrı izlenir; yazılım D kapıları fiziksel donanım onayı veya otomotiv sertifikasyonu yerine geçmez.

# Görev kapanış raporu şablonu

- Pozisyon, varyant ve görev kimliği

- Sonuç durumu: PASS / FAIL / UNKNOWN / BLOCKED / NOT\_RUN

- Oluşturulan veya değiştirilen dosyalar

- Çalıştırılan komutlar, araçlar ve sürümler

- Test sayısı, başarısız testler ve gerçek log konumu

- Kaynak revizyonları, referans commit'leri, lisanslar ve input/output hashleri

- Açık GAP-01…04 etkileri ve Claude/kullanıcı ihtiyacı

- Bir sonraki görev ve başlaması için gereken kapı

# Sonuç

v1 planı sentetik kapsamda tamamlandı. v2 yürütmesi RaspPowerHat patch'inin uygulanması ve şema v2 ile başlar. Referans aktarımları GAP-01/03/04'ü müşteri uyumu iddiası üretmeden ilerletir ve sentetik modellerin gerçek tasarımlardan nerede ayrıldığını gösterir; ilk aktarım bir FSM boşluğu buldu. CAN/TWAI, LIN ve GNSS yalnız A varyantına; automotive power ve ignition shutdown yalnız B varyantına bağlı kalır. Genel rail-budget işlevi REP-02 üzerinden tüketilir.

Codex görev planlaması • 26.10.2026
