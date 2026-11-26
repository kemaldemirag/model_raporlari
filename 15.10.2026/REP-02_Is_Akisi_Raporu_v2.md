**REP-02 — İŞ AKIŞI RAPORU**

Power Rail Validation and Review

26.10.2026 | v2.0 | JOB-02 / JOB-03 | Repo-spesifik geliştirme planı | v1.0 (15.10.2026) yerine geçer

Repo: github.com/kemaldemirag/power-rail-validation-and-review (HEAD 66f2073) | D1–D4 tamam; D5 intake kapısı uygulandı; REP-01 dikey dilimi BLOCKED

**v2 değişiklik özeti:** (1) v1'deki yedi iş akışının tamamı sentetik kapsamda bitti (404 test geçiyor); v2 durumları buna göre güncellendi. (2) Ürünün tek açık hedefi olan gerçek dikey dilim, REP-01'i beklemek yerine yayınlanmış bir referans tasarımla (REFERENCE\_DESIGN) çalıştırılacak şekilde yeniden tanımlandı. (3) REFERENCE\_DESIGN ve PORTFOLIO\_DECISION kaynak sınıfları, sözleşme sürümü artırılarak eklenir. (4) Lisans (All rights reserved) ve uzak CI kullanıcı kararı olarak işaretlendi.

# 1. Ürün amacı ve mevcut durum

Güç rayı, sıralama, hata enjeksiyonu ve kanıt tabanlı inceleme için yeniden kullanılabilir yazılım ürünü.

**Mevcut durum (26.10.2026):** README durumu D5\_PROJECT\_INTAKE\_GATE\_IMPLEMENTED — CURRENT\_REP01\_VERTICAL\_SLICE\_BLOCKED. D1 veri sözleşmesi, D2 rail-budget ve sequencing, D3 fault injection ve evidence/integration, D4 CLI ve yerel release adayı (0.5.0) tamamlandı; D5 intake kapısı 0.6.0.dev0 kaynak ağacında. 404 birim testi geçiyor. Ürün artık sentetik geliştirme sözleşmesi değil, çalışan bir araçtır; eksik olan, aracın gerçek bir tasarım girdisiyle uçtan uca kanıtlanmasıdır.

Ürün hedefi değişmedi: parametrik, yeniden üretilebilir ve yeni saha senaryolarıyla genişletilebilir dijital araç.

# 2. Kapsam ve sınırlar

- G1 BLOCKED iken sentetik model, sözleşme ve testler hazırlanabilir; gerçek şematik/PCB çalışması bu ürünün kapsamında değildir.

- Kaynak sınıfları SOURCE\_EXPLICIT, DERIVED, ENGINEERING\_RULE, ASSUMPTION, UNKNOWN, CONFLICT (v1) + REFERENCE\_DESIGN (sabit commit veya doküman revizyonu, SHA-256, lisans, sayfa/satır) + PORTFOLIO\_DECISION (imzalı karar kaydı). Yeni sınıflar sözleşme sürümü artırılarak eklenir; v1 sözleşmesi kırılmaz.

- Referans tasarım girdisiyle üretilen sonuç PASS olabilir ama raporda kaynak sınıfı görünür; müşteri tasarımı veya REP-01 tasarımı için kanıt sayılmaz.

- Her kural sonucu PASS/FAIL/UNKNOWN/BLOCKED/NOT\_RUN, kaynak, reason\_code, input hash ve sürüm içerir.

- Fiziksel ölçüm, EMC/ISO/AEC uygunluğu veya gerçek saha başarısı sentetik veya referans testlerle iddia edilmez.

# 3. Repo-spesifik iş akışları

## WF-02-01 — 01 Veri sözleşmesi

| **Alan** | **Açıklama** |
| --- | --- |
| Girdi ve işlem | Rail, load, regulator, source, event ve finding için JSON/CSV şeması. v2: kaynak sınıfı enum'una REFERENCE\_DESIGN ve PORTFOLIO\_DECISION eklenir (sözleşme sürümü artırılır, v1 fixture'ları geçerli kalır). |
| Normal / hata / sınır senaryoları | Geçerli, eksik ve bozuk veri; v1 ve v2 sözleşme sürümlü fixture'lar |
| Çıktı | Şema, fixture, sözleşme testleri; v2 sözleşme notu |
| Kabul kriteri | UNKNOWN açık tutulur; yanlış tip FAIL; v1 fixture'ları v2 doğrulayıcıda kırılmaz |
| Durum (26.10) | D1 tamam. v2 sınıf eklemesi NOT\_STARTED. |
| Referans | — |

## WF-02-02 — 02 Rail budget

| **Alan** | **Açıklama** |
| --- | --- |
| Girdi ve işlem | Imin/typ/max, P=VI, kapasite ve marj hesapları; açık verim varsayımları |
| Normal / hata / sınır senaryoları | Aşırı yük, negatif akım, bilinmeyen limit; v2: referans tasarımın yayınlanmış yük ve verim değerleri |
| Çıktı | Hesap motoru, sonuç JSON |
| Kabul kriteri | Elle hesapla tutarlılık; eksik limit PASS değildir; referans girdide sonuç yayınlanmış veriyle kıyaslanır |
| Durum (26.10) | D2 tamam (Decimal tabanlı motor). |
| Referans | TI TIDA-01167 tasarım kılavuzu ve test verisi (kullanıcı indirir, sources/ altına hash'li) |

## WF-02-03 — 03 Sequencing

| **Alan** | **Açıklama** |
| --- | --- |
| Girdi ve işlem | VIN\_VALID, EN, PGOOD, RESET olayları ve zaman pencereleriyle FSM |
| Normal / hata / sınır senaryoları | Normal açılış, yanlış sıra, timeout, power-off; v2: referans tasarımın PGOOD/EN eşikleri |
| Çıktı | Deterministik olay izi |
| Kabul kriteri | İhlal edilen zaman sınırı ve reason\_code kayıtlı; GAP-03 referans eşikle REFERENCE\_DESIGN olarak kapanabilir |
| Durum (26.10) | D2 tamam. |
| Referans | Referans dikey dilimdeki regülatörün datasheet'i |

## WF-02-04 — 04 Fault injection

| **Alan** | **Açıklama** |
| --- | --- |
| Girdi ve işlem | Brownout, stuck-low, gecikme, ray kaybı, yeniden başlatma |
| Normal / hata / sınır senaryoları | Arıza ve kurtarma kombinasyonları |
| Çıktı | Fault runner, negatif testler |
| Kabul kriteri | Aynı fixture aynı sonuç; FAIL/UNKNOWN ayrılır |
| Durum (26.10) | D3 tamam. |
| Referans | — |

## WF-02-05 — 05 Regulator review

| **Alan** | **Açıklama** |
| --- | --- |
| Girdi ve işlem | VIN/IOUT, sıcaklık ve derating kuralları; kaynaklı SPICE model kaydı. v2: GAP-02 için tek bir exact regülatörün üretici datasheet'i ve (lisansı uygunsa) SPICE modeli hash'li olarak bağlanır. |
| Normal / hata / sınır senaryoları | Eksik model, limit dışı yük, transient/stability ayrımı; lisansı belirsiz model |
| Çıktı | Review rules, model provenance; tek regülatör için gerçek review raporu |
| Kabul kriteri | Transient stability kanıtı sayılmaz; lisansı belirsiz model BLOCKED |
| Durum (26.10) | D3 tamam; gerçek model yok (GAP-02 açık). |
| Referans | Üretici datasheet ve SPICE modelleri; PyLTSpice/PySpice \[doğrulanmalı\] |

## WF-02-06 — 06 Evidence & integration

| **Alan** | **Açıklama** |
| --- | --- |
| Girdi ve işlem | Kural bazlı bulgu raporu; REP-01/03 için sürümlü dosya API. v2: D5 intake'in ilk gerçek girdisi bir referans tasarım dikey dilimi (bir ray, bir exact regülatör, bir datasheet profili, bir referans artifact). |
| Normal / hata / sınır senaryoları | Eksik kaynak, çoklu konfigürasyon; referans artifact lisansı; REP-01 dilimi BLOCKED kalırken referans dilimi |
| Çıktı | Markdown/JSON rapor, API sözleşmesi; referans dikey dilim kanıt paketi |
| Kabul kriteri | Kural ID, kaynak, sürüm ve hash her sonuçta; referans dilimi uçtan uca çalışır ve yayınlanmış veriyle karşılaştırılır |
| Durum (26.10) | D3 tamam; D5 intake uygulandı; REP-01 dilimi BLOCKED. |
| Referans | TI TIDA-01167; blinken/vandal (KiCad, GPL-3.0 — yalnız veri çıkarımı, tasarım kopyalanmaz); REP-03 automotive-power adaptörü |

## WF-02-07 — 07 CLI & release

| **Alan** | **Açıklama** |
| --- | --- |
| Girdi ve işlem | CLI, temiz kurulum, CI, sentetik fixture ve kullanıcı kılavuzu. v2: uzak CI çalıştırması, lisans kararı, git tag ile sürüm. |
| Normal / hata / sınır senaryoları | Temiz ortam, bozuk fixture, farklı profil; uzak runner'da Python 3.9/3.12 |
| Çıktı | Sürüm paketi, test logu, README; uzak CI logu |
| Kabul kriteri | Bağımsız tekrar çalıştırılabilir MVP; uzak CI yeşil; lisans açıkça seçilmiş |
| Durum (26.10) | D4 yerel PASS; LICENSE.txt All rights reserved (portfolyo için karar gerekli); uzak CI NOT\_RUN. |
| Referans | — |

# 4. Çalıştırılabilir ürün mimarisi

configuration loader → schema validator → rail-budget / sequencing / fault-injection → review rules → evidence reporter → CLI (+ D5 intake). Klasörler: src/power\_rail\_validation/{contracts, rail\_budget, sequencing, fault\_injection, regulator\_review, integration, project\_intake, reporting}, scenarios, examples, tests, docs, evidence.

v2: references/<ad>/ klasörü — sabit commit veya doküman revizyonu, lock dosyası (hash, lisans, sayfa/satır), çıkarım script'i ve intake descriptor. REP-01/03 dosya tabanlı sürümlü entegrasyon değişmedi.

# 5. Test ve kabul planı

| **Test türü** | **Kontrol** | **Beklenen kanıt** |
| --- | --- | --- |
| Pozitif | Geçerli konfigürasyon | Kural bazlı PASS ve rapor |
| Negatif | Çatışma, sınır ihlali veya timeout | FAIL + reason\_code |
| Eksik veri | Müşteri veya kaynak parametresi yok | UNKNOWN/BLOCKED, asla sahte PASS değil |
| Tekrarlanabilirlik | Aynı kanonik fixture iki kez çalışır | Aynı teknik sonuç/hash |
| Entegrasyon | Diğer repo ile sürümlü sözleşme | Bağımsız modül sınırları |
| Temiz kurulum | Yeni ortamda README komutları | Gerçek test logu ve sürüm manifesti |
| Referans dilimi (v2) | Referans tasarım girdisi intake → rail-budget → sequence → review | Yayınlanmış veriyle karşılaştırma; sapmalar FAIL/UNKNOWN olarak raporlanır |
| Sözleşme uyumu (v2) | v1 fixture'ları v2 sözleşmede | Kırılma yok |

# 6. Claude ve Codex görev dağılımı

**Codex:** sözleşme sürümü artırma, referans aktarım script'i ve lock dosyası, intake descriptor, CLI çalıştırma, CI ve teslim paketi; her WF sonunda gerçek PASS/FAIL/NOT\_RUN logu.

**Claude:** referans tasarım seçimi ve lisans kontrolü, datasheet/model doğrulaması, sayfa/bölüm numaralı kanıt.

**Kullanıcı:** lisans seçimi, referans dokümanlarının indirilmesi (TI dosyaları bu ortamlardan indirilemeyebilir), uzak CI için push, kapı ve kapsam onayı.

# 7. Eksik bilgiler ve kapanış kanıtları

| **GAP** | **Konu** | **Kapanış kanıtı** | **v2 durumu / yol** |
| --- | --- | --- | --- |
| GAP-01 | PKG-A/B kaynak ve hash doğrulaması | Manifest ve hash karşılaştırması | Açık; REP-01 sources/ manifesti ile birlikte kapanır |
| GAP-02 | Gerçek regülatör modeli ve sınırları | Revizyonlu üretici kaynağı | Referans dilimindeki tek regülatör için REFERENCE\_DESIGN olarak kapanabilir |
| GAP-03 | PGOOD/EN/RESET eşikleri | Kaynaklı zaman sınırı | Aynı regülatörün datasheet'iyle kapanabilir |
| GAP-04 | Gerçek müşteri şemaları | Müşteri girdisi veya sentetik etiketi | Portfolyo yolu: referans tasarım + REP-01 PORTFOLIO\_DECISION |
| GAP-LIC | Repo lisansı | LICENSE dosyası ve SPDX | Yeni; kullanıcı kararı |

# 8. Sıralama ve teslim kapıları

v1 oturumları (1–6) tamamlandı. v2 sırası: (1) sözleşme v2 ve lisans kararı; (2) referans dikey dilim — aktarım, intake, uçtan uca çalıştırma; (3) GAP-02/GAP-03 kapanışı için regülatör review; (4) uzak CI, tag ve release; (5) REP-01 dilimi açıldığında aynı akışın REP-01 artifact'ıyla tekrarı.

D1 → D2 → D3 → D4 tamam; D5 intake uygulandı. v2 kapısı: D6 referans dikey dilim. Donanım G1–G6 kapıları ayrı izlenir. Kullanıcı onayı olmadan mevcut dosyalar silinmez, Git push yapılmaz.

# 9. Portföy teslim paketi

README ve quickstart; örnek konfigürasyonlar; CLI; test fixture ve otomatik testler; source/evidence register; karar ve kısıt raporları; lisans; sürüm/hash manifesti; sınırlamalar ve gelecekteki saha verisi adaptörleri. v2 ekleri: referans dikey dilim raporu (portfolyoda en güçlü kanıt), uzak CI rozeti, seçilmiş açık kaynak lisansı.
