**REP-03 — İŞ AKIŞI RAPORU**

ESP32-S3 Automotive Interfaces

26.10.2026 | v2.0 | JOB-04 / JOB-05 | Repo-spesifik geliştirme planı | v1.0 (15.10.2026) yerine geçer

Repo: github.com/kemaldemirag/esp32-s3-automotive-interfaces (HEAD cf4b691; RaspPowerHat patch'i 61b4b00 uygulanmayı bekliyor) | D1–D3 tamam; D4 yerel PASS, dağıtım BLOCKED; G1 BLOCKED

**v2 değişiklik özeti:** (1) v1'deki yedi iş akışı sentetik kapsamda tamamlandı (207 test; RaspPowerHat patch'iyle 210). (2) GAP-01/03/04'ün bir kısmı, sabit commit'li açık referans tasarımlarla (REFERENCE\_DESIGN) ilerletilecek şekilde yeniden tanımlandı; ilk aktarım (RaspPowerHat → Varyant B shutdown) yapıldı ve bir model boşluğu buldu. (3) REFERENCE\_DESIGN ve PORTFOLIO\_DECISION kaynak sınıfları ile REFERENCE yetkisi, sözleşme sürümü artırılarak eklenir. (4) Lisans ve uzak CI kullanıcı kararıdır.

# 1. Ürün amacı ve mevcut durum

CAN/LIN/GNSS arayüzleri ile Raspberry Pi otomotiv güç yönetimini ayrı varyantlarda modelleyen dijital çalışma alanı.

**Mevcut durum (26.10.2026):** README durumu D1\_APPROVED / WF-03-01…06\_COMPLETE / D2\_COMPLETE / D3\_SESSION\_4\_COMPLETE / INT-02A\_COMPLETE / D4\_LOCAL\_PASS\_RELEASE\_BLOCKED / G1\_BLOCKED / G2\_NOT\_STARTED. Varyant A için CAN, LIN ve GNSS motorları; Varyant B için automotive-power profilleri ve shutdown FSM; REP-02 wheel entegrasyonu (INT-02A); 7 komutlu repo03 CLI ve CI taslağı mevcut.

Protokole ve kaynaklı eşiklere bağlı sonuçlar tasarım gereği BLOCKED/UNKNOWN kalıyor. v2'nin amacı bu kilitleri açık referans tasarımlarla, müşteri uyumu iddiası üretmeden ilerletmektir.

# 2. Kapsam ve sınırlar

- G1 BLOCKED iken sentetik model, sözleşme ve testler hazırlanabilir; gerçek şematik/PCB çalışması G1/G2 onayını bekler.

- Kaynak sınıfları SOURCE\_EXPLICIT, DERIVED, ENGINEERING\_RULE, ASSUMPTION, UNKNOWN, CONFLICT (v1) + REFERENCE\_DESIGN + PORTFOLIO\_DECISION. Yetki (authority) listesine REFERENCE eklenir. Mevcut motorlar yalnız PROJECT\_REQUIREMENT ve HOST\_PROTOCOL yetkisine PASS veriyor; REFERENCE yetkisi PASS\_REFERENCE gibi ayrı bir durum üretir. Değişiklik yeni şema sürümüyle yapılır, v1 fixture'ları korunur.

- GAP-04 motorda dışarıdan kapatılamayan sabit bir kapıdır; v2'de de genel sonuç BLOCKED kalır. Referans veya portfolyo kararı, bağımlılık durumlarını (host protokolü, zamanlama) ilerletir.

- Her kural sonucu PASS/FAIL/UNKNOWN/BLOCKED/NOT\_RUN, kaynak, reason\_code, input hash ve sürüm içerir.

- Fiziksel ölçüm, EMC/ISO/AEC uygunluğu veya gerçek saha başarısı sentetik veya referans testlerle iddia edilmez.

# 3. Repo-spesifik iş akışları

## WF-03-01 — 01 Varyant sözleşmeleri

| **Alan** | **Açıklama** |
| --- | --- |
| Girdi ve işlem | JOB-04 CAN/LIN/GNSS ve JOB-05 güç kartını ayır; ortak şema. v2: kaynak sınıfı ve yetki enum'larının yeni sürümü; karar kaydı (docs/decisions/). |
| Normal / hata / sınır senaryoları | A, B ve yanlış birleşik profil; v1/v2 şema sürümleri |
| Çıktı | Variant matrix, schema v2 |
| Kabul kriteri | Gereksinimler varyantlar arasında sızmaz; v1 fixture'ları kırılmaz |
| Durum (26.10) | D1 tamam; v2 NOT\_STARTED. |
| Referans | — |

## WF-03-02 — 02 CAN/TWAI

| **Alan** | **Açıklama** |
| --- | --- |
| Girdi ve işlem | MCU TWAI ve harici CAN transceiver ayrımı; bus-off ve recovery FSM. v2: GAP-01 için referans tasarımdaki transceiver parça numarası ve datasheet'i. |
| Normal / hata / sınır senaryoları | Normal frame, bus-off, transceiver off; referans transceiver power-off davranışı |
| Çıktı | CAN olay motoru, testler; transceiver kaynak kaydı |
| Kabul kriteri | Sentetik protokol sonucu fiziksel CAN kanıtı değildir |
| Durum (26.10) | WF tamam (sentetik). |
| Referans | MagnusThome/RejsaCAN-ESP32 (ESP32/ESP32-S3, 5–24 V, CAN; lisans kontrol edilecek); blinken/vandal (SN65HVD230 transceiver, GPL-3.0); ESP-IDF TWAI örnekleri |

## WF-03-03 — 03 LIN

| **Alan** | **Açıklama** |
| --- | --- |
| Girdi ve işlem | UART/LIN transceiver, wake/sleep ve timeout sözleşmesi. v2: sentetik LIN FSM'nin TJA1020/TJA1021 durum makinesiyle karşılaştırılması. |
| Normal / hata / sınır senaryoları | Wake, sleep, stuck dominant; referans durum makinesiyle uyumsuz geçişler |
| Çıktı | LIN FSM ve negatif test; referans karşılaştırma raporu |
| Kabul kriteri | Bilinmeyen parça davranışı UNKNOWN; referansla uyumsuzluk açıkça raporlanır |
| Durum (26.10) | WF tamam (sentetik). |
| Referans | mestrode/Lin-Interface-Library ve Lin-Transceiver-Library (ESP32 UART, TJA1020 durum makinesi); macchina/LIN (TJA1021, wake-up); lisanslar kontrol edilecek |

## WF-03-04 — 04 GNSS

| **Alan** | **Açıklama** |
| --- | --- |
| Girdi ve işlem | Modül/anten/RF, fix-quality, stale veri ve güç profili |
| Normal / hata / sınır senaryoları | No-fix, stale data, anten arızası |
| Çıktı | GNSS validator ve kaynak matrisi |
| Kabul kriteri | Sentetik konum ölçüm gibi sunulmaz |
| Durum (26.10) | WF tamam (sentetik); rf\_suitability BLOCKED (GAP-02). |
| Referans | Açık: uygun açık kaynak GNSS modül entegrasyon referansı henüz seçilmedi (Claude araştırması) |

## WF-03-05 — 05 Automotive power

| **Alan** | **Açıklama** |
| --- | --- |
| Girdi ve işlem | 12 V giriş, 5.1 V/5 A hedef, cold crank, load dump, jump-start; REP-02 ile arayüz. v2: transient profilleri için referans tasarım; REP-02 referans dikey dilimiyle ortak kaynak. |
| Normal / hata / sınır senaryoları | Suppressed/unsuppressed, brownout; referans güç katının 5 A hedefini karşılamaması |
| Çıktı | Güç senaryo profilleri; referans güç girişi kaydı |
| Kabul kriteri | OEM profili yoksa exact-part BLOCKED; referans değerler REFERENCE\_DESIGN etiketli |
| Durum (26.10) | WF tamam (sentetik); INT-02A offline PASS; canlı adaptör NOT\_RUN. |
| Referans | TI TIDA-01167 (12/24 V load dump); CarPiHAT (açık kaynak değil — yalnız yayınlanmış davranış); RaspPowerHat güç katı 2 A ile sınırlı, 5 A hedefi için referans değil |

## WF-03-06 — 06 Ignition shutdown

| **Alan** | **Açıklama** |
| --- | --- |
| Girdi ve işlem | Ignition-off, host notify, ACK, grace period, power cut FSM. v2: RaspPowerHat (MIT, commit 1cca12b) referans aktarımı; host protokolü ve eşikler script ile firmware'den çıkarıldı. |
| Normal / hata / sınır senaryoları | ACK yok, güç erken kesilir, hızlı restart; ACK zaman aşımından sonra zorunlu kesme |
| Çıktı | Shutdown FSM, olay izi; references/rasppowerhat/ (lock, 4 senaryo, rapor) |
| Kabul kriteri | Raspberry Pi protokolü bilinmiyorsa UNKNOWN; referans protokol benimsenirse PORTFOLIO\_DECISION ile kaydedilir |
| Durum (26.10) | WF tamam. Referans aktarımı patch olarak hazır: host protokolü BLOCKED → UNKNOWN (referans) / PASS (benimsenirse). Bulgu: FSM'de 'ACK zaman aşımından sonra zorunlu kesme' yolu yok → EARLY\_POWER\_CUT (kullanıcı kararı). |
| Referans | Benoit3/RaspPowerHat (12–24 V sürekli + ACC, ATtiny45, 600 s zorunlu kesme, 5 s OFF debounce) |

## WF-03-07 — 07 Varyant test ve release

| **Alan** | **Açıklama** |
| --- | --- |
| Girdi ve işlem | A/B için ayrı test, CLI, rapor, CI, temiz kurulum. v2: lisans seçimi, uzak CI, referans aktarımlarının CI'a eklenmesi. |
| Normal / hata / sınır senaryoları | A, B, bozuk ve birleşik profil; referans senaryoların tekrar üretimi |
| Çıktı | İki varyantlı MVP, README; uzak CI logu |
| Kabul kriteri | Tek tek tekrar üretilebilir; otomotiv sertifikası iddiası yok; lisans açık |
| Durum (26.10) | D4 yerel PASS; LICENSE yok; uzak CI NOT\_RUN; commit/push yapılmadı. |
| Referans | — |

# 4. Çalıştırılabilir ürün mimarisi

variant loader → schema validator → CAN/LIN/GNSS veya automotive-power/shutdown senaryo motorları → evidence reporter → CLI. Mevcut klasörler: firmware/repo03\_{contract, can, lin, gnss, power, shutdown, integration, cli}, variants/{schema, fixtures}, scripts, tests, evidence. Genel rail-budget motoru REP-02 sorumluluğundadır.

v2: references/<ad>/ — sabit commit, reference.lock.json (hash, lisans, satır referansı), scripts/import\_reference\_<ad>.py, üretilen senaryolar ve raporlar. Referans senaryolar mevcut fixture dizinlerine karıştırılmaz.

# 5. Test ve kabul planı

| **Test türü** | **Kontrol** | **Beklenen kanıt** |
| --- | --- | --- |
| Pozitif | Geçerli konfigürasyon | Kural bazlı PASS ve rapor |
| Negatif | Çatışma, sınır ihlali veya timeout | FAIL + reason\_code |
| Eksik veri | Müşteri veya kaynak parametresi yok | UNKNOWN/BLOCKED, asla sahte PASS değil |
| Tekrarlanabilirlik | Aynı kanonik fixture iki kez çalışır | Aynı teknik sonuç/hash |
| Entegrasyon | Diğer repo ile sürümlü sözleşme | Bağımsız modül sınırları |
| Temiz kurulum | Yeni ortamda README komutları | Gerçek test logu ve sürüm manifesti |
| Referans aktarımı (v2) | Sabit commit'ten script ile senaryo üretimi | İki çalıştırmada bayt bayt aynı çıktı; şema doğrulaması; raporların tekrar üretimi |
| Referans uyumu (v2) | Sentetik FSM ile referans davranışı | Uyumsuz geçişler açık reason\_code ile raporlanır |

# 6. Claude ve Codex görev dağılımı

**Codex:** şema v2, referans aktarım script'leri, senaryo ve test üretimi, CLI/CI, teslim paketi; her WF sonunda gerçek PASS/FAIL/NOT\_RUN logu.

**Claude:** referans seçimi ve lisans kontrolü, transceiver/GNSS datasheet doğrulaması, sayfa/bölüm numaralı kanıt.

**Kullanıcı:** referans protokolün benimsenmesi (PORTFOLIO\_DECISION), FSM'e zorunlu kesme yolunun eklenmesi kararı, lisans seçimi, patch'in uygulanması ve push.

# 7. Eksik bilgiler ve kapanış kanıtları

| **GAP** | **Konu** | **Kapanış kanıtı** | **v2 durumu / yol** |
| --- | --- | --- | --- |
| GAP-01 | CAN/LIN exact transceiver ve güç-kapalı davranışı | Üretici datasheet | Referans tasarımlardaki transceiver'lar (ör. SN65HVD230, TJA1020) üzerinden REFERENCE\_DESIGN olarak ilerler |
| GAP-02 | GNSS anten/RF yerleşimi | Modül entegrasyon kılavuzu | Açık; referans araştırması gerekiyor |
| GAP-03 | OEM transient ve ignition koşulları | Müşteri/OEM profili | Transient: TIDA-01167 referansı; ignition: RaspPowerHat eşikleri |
| GAP-04 | Raspberry Pi modeli ve kapanış protokolü | Onaylı host sözleşmesi | RaspPowerHat protokolü (I2C 0x27 durum çerçevesi + PI\_HALTED GPIO); benimsenmesi PORTFOLIO\_DECISION. Motor kapısı sabit olduğu için genel sonuç BLOCKED kalır |
| GAP-LIC | Repo lisansı | LICENSE ve SPDX | Yeni; kullanıcı kararı |

# 8. Sıralama ve teslim kapıları

v1 oturumları (1–6) tamamlandı. v2 sırası: (1) RaspPowerHat patch'inin uygulanması ve şema v2; (2) kullanıcı kararları (host protokolü benimseme, zorunlu kesme yolu, lisans); (3) LIN ve CAN referans aktarımları; (4) automotive-power için TIDA-01167 profili ve REP-02 referans diliminin ortak kullanımı; (5) uzak CI ve release.

D1 → D2 → D3 → D4 yerel PASS. v2 kapısı: D5 referans aktarımları. Donanım G1–G6 kapıları ayrı izlenir. Kullanıcı onayı olmadan mevcut dosyalar silinmez, Git push yapılmaz.

# 9. Portföy teslim paketi

README ve quickstart; örnek konfigürasyonlar; CLI; test fixture ve otomatik testler; source/evidence register; karar ve kısıt raporları; lisans; sürüm/hash manifesti; sınırlamalar ve gelecekteki saha verisi adaptörleri. v2 ekleri: referans aktarım paketleri (lock + script + rapor) ve sentetik model ile gerçek tasarım arasındaki farkları gösteren bulgular — portfolyoda mühendislik muhakemesini en iyi gösteren kısım.
