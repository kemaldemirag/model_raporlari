**REP-01 — ESP32-S3 24V Interface Board**

REPO-SPESİFİK İŞ AKIŞI RAPORU | v2.0 | 26.10.2026 | v1.0 (16.10.2026) yerine geçer

Repo: github.com/kemaldemirag/esp32-s3-24v-interface-board (HEAD b335eec) | JOB-01 | G0 PASS; G1 BLOCKED (portfolyo yolu tanımlı); G2–G6 NOT\_STARTED

**v2 değişiklik özeti:** (1) Portfolyo projesinde yetkili müşteri olmadığı için PORTFOLIO\_DECISION ve REFERENCE\_DESIGN kaynak sınıfları eklendi; bunlar müşteri uyumu iddiası üretmez, ayrı durum kodu (PORTFOLIO\_ACCEPTED, PASS\_REFERENCE) taşır. (2) Her iş akışına sabit commit/hash ile bağlanan referans repo ve tasarımlar eklendi. (3) WF03 güç hesapları REP-02 (power-rail-validation-and-review) aracına girdi olarak yönlendirildi. (4) WF05/WF06 tasarım sahibi kullanıcıdır; Codex inceleme, CI ve kanıt üretir. (5) 17–26 Ekim arasında tamamlanan iş ve kapanan kaynak soruları duruma işlendi.

# 1. Ürün ve müşteri kapsamı

İlan ekran görüntülerinde (SRC-L01) belirtilen kapsam değişmedi: ESP32-S3-WROOM-1-N8, 24 V araç girişi ve koruma, 24→5 V ve 5→3,3 V switching güç zinciri, FAT32 USB-A host, 5× 5 V TTL TX, 5× RS-232 TX + 1 RX, 8×2 HD44780 LCD, iki buton, Wi-Fi, CAN/TWAI ve TFT için DNP seçenekleri. 4 katman ve ≤64×65 mm; KiCad, ERC/DRC, Gerber, drill, CPL, BOM ve STEP. 100 adet için 10–15 USD PCBA hedefi fizibilite onayı değildir; kaba araştırma bu hedefin yalnız alt sınırda ve uygun köprü seçimiyle tutabileceğini gösteriyor (UNKNOWN, fiyatlandırılmış BOM gerekir).

**Karar yetkisi (v2):** Proje bir portfolyo projesidir; yetkili müşteri yoktur. Müşteriye atanmış sorular, proje sahibi (@kemaldemirag) tarafından imzalanan PORTFOLIO\_DECISION kayıtlarıyla kapatılabilir. Bu kayıtlar müşteri kanıtı değildir; gerçek bir müşteri gelirse ilgili soru yeniden açılır.

**Kaynak sınıfları:** SOURCE\_EXPLICIT, DERIVED, ENGINEERING\_RULE, HANDOFF\_ONLY, UNKNOWN (v1) + REFERENCE\_DESIGN (sabit commit, dosya SHA-256, lisans, satır/sayfa referansı) + PORTFOLIO\_DECISION (imzalı karar kaydı, docs/decisions/).

Kaynak temeli: CLAUDE-01/02/03/04/06/07/09/10; PKG-A ve PKG-B; Claude inceleme paketleri (25–26.10.2026, Tur 1–3) ve ChatGPT Ek A / Tur 3. İlan kaynaklı şartlar, iç mühendislik kuralları, referans tasarımdan alınan değerler ve portfolyo kararları ayrı tutulur. G1 geçmeden şematik/PCB, G3 geçmeden layout başlatılmaz.

# 2. Sistem mimarisi (taslak)

24 V INPUT → PROTECTION → 5 V BUCK → 3,3 V BUCK → ESP32-S3. 5 V hattında USB-A VBUS ve çevre yükleri; ESP32-S3 tarafında USB OTG, UART/GPIO, TTL level shifting, RS-232 transceiver, LCD ve DNP CAN. Bu, doğrulanmış şema değildir.

| **Blok** | **v2 taslak durumu** | **Kanıt / durum** |
| --- | --- | --- |
| Giriş koruması | Taban ISO 16750-2 Test B (24 V sistemde 58 V kenetli); Test A (~202 V) opsiyon ve ayrı maliyet kalemi; giriş tarafı ≥60 V dayanım | PROPOSED; tasarım girdisi, sertifikasyon kanıtı değil |
| Ters polarite | Akıllı diyot (ör. LM74610-Q1 sınıfı) veya seri diyot; WF02'de kıyas | UNKNOWN; ref. TI PMP15023 |
| Seri çıkışlar | MIRRORED\_5 (çift bazlı TTL\_i ↔ RS232\_i); seri kaynak sınıfı A (2 yerel UART + quad köprü) veya C (CH32V303/307 yardımcı MCU) | D-G1-PORTFOLIO-001 PROPOSED; sınıf UNKNOWN |
| RS-232 katı | 5 sürücü/3 alıcı tek entegre (TRS3238E ailesi); FORCEOFF ile banka OE adayı | CANDIDATE |
| UART0 | Programlama/log; akışa verilmesi ayrı soru (Q-UART0) | UNKNOWN |
| USB-A host | ESP-IDF MSC host + FATFS; S3 örneğinde 2026 forum raporlu sorun; esp\_msc\_ota çalışan S3 örneği | UNKNOWN; bench riski |

# 3. Altı repo-spesifik iş akışı

## WF01 — Requirement & Interface Contract

| **Alan** | **Açıklama** |
| --- | --- |
| Girdi/bağımlılık | REQ-001…005; CLAUDE-06/07; müşteri ilanları (SRC-L01); D-G1-PORTFOLIO-001; Claude/ChatGPT inceleme turları |
| İşlem | v1 işlemleri korunur. Ek: (a) karar kaydı — Q-C07, port varsayımları ve seri kaynak sınıfı PORTFOLIO\_DECISION olarak; (b) 11 portluk seri port envanteri (yön, mantıksal kaynak, baud, çift içi baud, eşzamanlılık, boot/power-off, konektör); (c) sources/ manifesti (SHA-256, lisans, provenans); (d) README, sözlük ve karar indeksi; (e) docx araştırmaların markdown'a çevrilmesi. |
| Teslimatlar | requirements-traceability.csv; interface-contract.md; gpio-budget.csv; open-decisions.md (v1.2 mevcut, yerinde güncellenir) + docs/decisions/; docs/serial-port-inventory.md; sources/MANIFEST; README + sözlük |
| Hata ve güvenlik davranışı | 10 TX kanalının anlamı veya frozen GPIO bilinmiyorsa UART mimarisi BLOCKED. Portfolyo kararı imzasızsa PROPOSED kalır; hiçbir kayıt PASS'e yükseltilmez. Yeni -v1.x dosyası açılmaz; sürüm git tag ile işaretlenir. |
| Kabul kriteri | Her arayüz için gerilim, yön, rol, power-off, kaynak ve test tanımlı; her karar kaynağı (müşteri / portfolyo / referans) ayırt edilebilir. |
| Durum (26.10) | v1 teslimatları mevcut (v1.2). Karar kaydı taslak (PROPOSED), port envanteri ve sources/ manifesti yok. |
| Referans | strictdoc-project/strictdoc (izlenebilirlik araçlaştırma, opsiyonel) |

## WF02 — Power Entry Architecture

| **Alan** | **Açıklama** |
| --- | --- |
| Girdi/bağımlılık | REQ-001; Q-C01…03; CF-01/06; ISO 16750-2 Test A/B ve ISO 7637-2 ayrımı; referans tasarımlar |
| İşlem | M1 TVS+uygun buck, M2 yüksek gerilim sınıfı buck+koruma, M3 aktif OV kesme/sınırlama+TVS seçeneklerini transient, SOA, termal, alan ve maliyet açısından karşılaştır. Her seçenek için alan ve BOM tahmini, yayınlanmış bir referans tasarımdan (REFERENCE\_DESIGN) okunur. |
| Teslimatlar | power-entry-options.md; protection-comparison.csv; transient-scenarios.json |
| Hata ve güvenlik davranışı | OEM darbe profili, VIN min/max yoksa exact-part seçme veya PASS verme. Test B tabanı portfolyo kararıdır; standart dalga şekilleri standart metninden alınmadan sertifikasyon iddiası yok. |
| Kabul kriteri | Her seçenek için koşullar, riskler, kanıt sınıfı, referans kaynağı ve açık karar kaydı. |
| Durum (26.10) | NOT\_STARTED. Test B taban önerisi PROPOSED. |
| Referans | TI TIDA-01167 (12/24 V bastırılmamış load dump, tasarım dosyalarıyla); TI PMP15023 (LM74610-Q1 ters polarite); blinken/vandal (KiCad, 12–24 V korumalı giriş) |

## WF03 — Power Tree & Regulator Design

| **Alan** | **Açıklama** |
| --- | --- |
| Girdi/bağımlılık | REQ-002; WF02 taslakları; Q-C04…06; REP-02 power-rail-validation-and-review D5 intake sözleşmesi |
| İşlem | 5 V ve 3,3 V rayları için min/tipik/tepe akım, inrush, verim, kayıp, derating, kondansatör ve zamanlama; PGOOD/RESET; kararlılık kanıtını transientten ayır. v2: hesaplar REP-02 aracının rail-budget, sequencing ve regulator-review modüllerinde çalıştırılır; REP-01 yalnız intake descriptor ve kaynakları üretir. |
| Teslimatlar | rail-budget.csv; regulator-candidates.csv; sequencing.csv; thermal-budget.md; simulation-plan.md + REP-02 intake descriptor ve REP-02 raporu (hash'li) |
| Hata ve güvenlik davranışı | PKG-A CM5 yükünü JOB-01'e aktarma; eksik yükte UNKNOWN; ngspice yoksa NOT\_RUN. REP-02 sürüm/şema uyumsuzluğu BLOCKED. |
| Kabul kriteri | Worst-case hesap izi, varsayım ve kaynak mevcut; REP-02 raporu girdi hash'iyle bağlı; kesin parça kararı ertelenmiş. |
| Durum (26.10) | NOT\_STARTED. REP-02 D4 tamam, D5 intake kapısı hazır; REP-01 dilimini bekliyor. |
| Referans | REP-02 (kendi aracın); WF02 referans tasarımlarının BOM ve yük verileri |

## WF04 — MCU & Peripheral Integration

| **Alan** | **Açıklama** |
| --- | --- |
| Girdi/bağımlılık | REQ-003/004; CLAUDE-03; GPIO ve arayüz kayıtları; seri port envanteri; CF-09 Python modelleri |
| İşlem | WROOM-1-N8 pin/strapping, USB-A host VBUS/ESD, UART0 debug, 5 V TTL buffer, RS-232 transceiver, LCD, boot glitch ve power-off backfeed analizi. v2: Q-UART0 (ROM boot logu, konsol alternatifi); seri kaynak sınıfı A–D parametrik kıyası (resmî datasheet sayfasına bağlı); CF-09 modellerinin köprü kanal sayısı, FIFO ve SPI saatiyle parametrik hale getirilmesi; firmware mimarisi (görev yapısı, aynalama, OE sıralaması). |
| Teslimatlar | pin-allocation.csv; usb-host-contract.md; serial-architecture.md; power-domains.md; boot-reset-checklist.md + serial-source-comparison.md; firmware-architecture.md; senaryo başına SPI kullanım tablosu |
| Hata ve güvenlik davranışı | CF-09 çözülmeden 10 TX için topoloji seçme; PHY/JTAG davranışını TRM ile doğrula. RMT 48 word değeri UART FIFO gibi kullanılmaz (ESP32-S3 UART FIFO 128 byte, soc\_caps.h). |
| Kabul kriteri | GPIO çakışması yok veya açık blocker; boot, aktif ve power-off durumları tanımlı; mevcut 83 CF-09 testi geçer. |
| Durum (26.10) | CF-09 host/transport/scheduler modelleri mevcut (83/83 test, sentetik). Seri kaynak sınıfı ve köprü exact-part UNKNOWN. |
| Referans | ESP32 Serial Swiss Army Knife (ESP32-S3 + MAX3243, aşamalı firmware, güvenli seri varsayılanlar); espressif/esp\_msc\_ota (S3 MSC host); nopnop2002/esp-idf-sc16is750; openwch/ch32v307 ve espressif/esp-hosted (senaryo C, SPI taşıma); MAX14830, CH9434, SC16IS752/762, TRS3238E datasheet'leri |

## WF05 — Schematic & Pre-layout Verification

| **Alan** | **Açıklama** |
| --- | --- |
| Girdi/bağımlılık | G1 PASS (müşteri yolu) veya G1 PORTFOLIO\_ACCEPTED (portfolyo yolu) + G2 mimari kabulü + WF01–04 |
| İşlem | Onaylı girdilerle KiCad şematik, ERC, model kökenli simülasyon, BOM, pre-layout review; stackup, dönüş yolu, hot-loop, anten keepout ve termal riskler. v2: şematik kullanıcı tarafından çizilir; Codex ERC/BOM üretimini KiBot ile CI'da çalıştırır, kicad-happy ile inceleme yapar, bulguları kanıt klasörüne yazar. |
| Teslimatlar | KiCad şematik; ERC; BOM; simulation-evidence; prelayout-review.md; G3 evidence; CI workflow |
| Hata ve güvenlik davranışı | G1 BLOCKED iken şema yok; ERC başarısı EMC veya loop stability kanıtı değildir; yapay zekâ incelemesi bağımsız doğrulama yerine geçmez. |
| Kabul kriteri | G3 kriterleri ve bağımsız kritik bulgu kontrolü sağlanmış. |
| Durum (26.10) | NOT\_STARTED |
| Referans | KiBot; aklofas/kicad-happy; OLIMEX/ESP32-GATEWAY (ESP32 çekirdek, KiCad); SurrealityLabs/sx16is752-breakout |

## WF06 — PCB & Manufacturing Package

| **Alan** | **Açıklama** |
| --- | --- |
| Girdi/bağımlılık | G3 PASS; WF05 çıktıları |
| İşlem | 4 katman ≤64×65 mm PCB; DRC; Gerber, drill, CPL, BOM, STEP, DFM ve 100 adet maliyet incelemesi. v2: layout kullanıcı tarafından; Codex üretim çıktılarını CI'da aynı revizyondan üretir ve checksum manifestini yazar. |
| Teslimatlar | KiCad PCB; DRC; Gerber; NC drill; CPL; BOM; STEP; release checksums |
| Hata ve güvenlik davranışı | G3 olmadan layout yok; fiziksel test ve otomotiv sertifikası iddiası yok. |
| Kabul kriteri | Üretim dosyaları yeniden üretilebilir; açıklanmamış DRC hatası yok; maliyet hedefi kaynaklı değerlendirilmiş. |
| Durum (26.10) | NOT\_STARTED |
| Referans | KiBot üretim çıktıları; WF02/WF04 referans tasarımlarının alan kullanımı |

# 4. Normal / hata / sınır testleri

- T01 Açılış — VIN\_VALID → 5 V → 3,3 V → reset; sentetik olay izi REP-02 sequencing motorunda.

- T02 Transient — Test B (58 V) taban profili; Test A opsiyon; koruma mimarisi kanıtı yoksa UNKNOWN.

- T03 Brownout — Ray gerilimi düşüşü, RESET\_N ve timeout davranışı; REP-02 fault injection.

- T04 USB overload — VBUS akım sınırlama/kapama senaryosu; eşik portfolyo kararı veya müşteri onayı bekler.

- T05 10 TX / 3 UART — Seri kaynak sınıfı seçilmediyse tasarım BLOCKED; CF-09 modelleri senaryo başına SPI yükünü raporlar.

- T06 Power-off — USB ve RS-232 üzerinden geri besleme risk analizi; TRS3238E FORCEOFF yüksek empedans doğrulaması.

- T07 Boot glitch — Strapping, UART0 ROM logu ve TX sahte start biti riskleri; OE kapalı tutma.

- T08 ERC/DRC — G1/G3 sonrasında KiBot ile CI raporları; araç yoksa NOT\_RUN.

- T09 BOM — 100 adet hedef maliyetin güncel kaynak ve alternatiflerle karşılaştırılması.

- T10 Referans aktarımı — Her REFERENCE\_DESIGN girdisi sabit commit ile iki kez üretildiğinde bayt bayt aynı çıktı.

- T11 CF-09 regresyonu — cd tests && python3 -m unittest ile 83 testin tamamı geçer; kök dizinden çalıştırma yöntemi belgelenir.

# 5. Müşteri ve kaynak açıkları

| **Alan** | **Açıklama** | **v2 durumu / sahibi** |
| --- | --- | --- |
| Q-C01…03 | VIN çalışma aralığı, OEM/ISO transient profili, jump-start ve ters polarite. | Test B taban PROPOSED; kullanıcı |
| Q-C04…06 | USB maksimum akım, LCD besleme/arka ışık, ortam sıcaklığı. | UNKNOWN; kullanıcı (portfolyo varsayımı) |
| Q-C07 | 5 TTL + 5 RS-232 kanallarının bağımsız veri akışı olup olmadığı; frozen GPIO belgesi. | MIRRORED\_5 PROPOSED (D-G1-PORTFOLIO-001); kullanıcı onayı |
| Q-C08…10 | RS-232 baud/kablo/yük, ESD hedefi, PCB üretici ve montaj sınıfı. | UNKNOWN; kullanıcı |
| Q-C12 | Port başına baud, çift içi baud, eşzamanlılık, RX'in eşi. | UNKNOWN; seri port envanteri |
| Q-UART0 | UART0'ın akışa verilip verilemeyeceği; ROM boot logu ve konsol alternatifi. | Yeni, UNKNOWN; Claude (TRM) |
| SRC-HASH | PKG-A/B SHA-256 doğrulaması; PKG-C kapsam/manifest kararı. | Hash var; lisans/provenans açık; Codex manifest + kullanıcı lisans |
| SOURCE-LINK | Birincil PDF revizyon ve sayfa eşleştirmesi; doğrulanamayan UNKNOWN. | Açık; sources/ klasörü |
| D-CF09-SRC-01 | "48 bytes / 48 word" çelişkisi. | SOURCE\_RESOLVED: RMT kanal belleği, 48 word (soc\_caps.h); CF-09 etkisi yok |
| REQ-COST | 10–15 USD @100 ve 64×65 mm fizibilitesi. | UNKNOWN; kaba tahmin 14–23 USD (LCD hariç) |

# 6. Codex görev sırası ve sınırlar

1. Repoya AGENTS.md ekle (durum kodları, test komutu, dokunulmayacak dosyalar, yeni -v dosyası yasağı) ve testleri kök dizinden çalışır hale getirip CI'a bağla.

2. WF01 eklerini üret: sources/ manifest script'i, README + sözlük + karar indeksi, docx → md dönüşümü, seri port envanteri iskeleti (değerler kullanıcıda).

3. Referans aktarım görevleri: her referans repo sabit commit'te references/<ad>/ altına lock dosyası, script ve fixture ile bağlanır.

4. WF04: CF-09 modellerini parametrik hale getir; senaryo A–D için SPI kullanım tablosu üret (kullanıcı mimari kararından sonra).

5. WF03: REP-02 intake descriptor'ını üret ve REP-02 CLI'ı çalıştır; raporu hash'iyle REP-01 kanıtına bağla.

6. G1 geçmeden WF05, G3 geçmeden WF06 uygulamasını başlatma; WF05/WF06'da Codex yalnız CI, ERC/DRC çalıştırma ve inceleme yapar.

7. Git commit/push, paket kurulumu, Drive yazma, dosya silme ve mevcut dosya üzerine yazma için ayrıca onay al. Karar kaydı alanlarını Codex doldurmaz.

# 7. Claude görevleri

ESP32-S3 TRM: USB OTG/USB Serial-JTAG, strapping ve ROM boot log kontrolü (Q-UART0). Resmî datasheet doğrulaması: SC16IS752/762 SPI saat tavanı ve kristal tablosu; MAX14830 FIFO word genişliği; CH9434 resmî İngilizce datasheet, saat kaynağı ve reset/IRQ; TRS3238E FORCEOFF=L iken çıkış empedansı. TI koruma ve regulator datasheetleri; PGOOD/RESET supervisor; 5 V TTL/RS-232 power-off; üretici stackup/anten keepout. Referans repo seçimi ve lisans kontrolü. Her iddia için SOURCE\_EXPLICIT / DERIVED / ENGINEERING\_RULE / HANDOFF\_ONLY / REFERENCE\_DESIGN / UNKNOWN, kaynak revizyonu ve sayfa/bölüm zorunludur. Claude karar vermez; yapay zekâ üretimi özetler kaynak olarak kullanılmaz.

# 8. Kabul kapıları ve ürün teslimi

| **Alan** | **Açıklama** |
| --- | --- |
| G1 | Kaynak hash/lisans/provenans ve gereksinimler. Müşteri yolu: müşteri onayı → PASS. Portfolyo yolu: imzalı PORTFOLIO\_DECISION kayıtları + PKG-A/B kapanışı → PORTFOLIO\_ACCEPTED (müşteri uyumu iddiası yok). Halen BLOCKED. |
| G2 | Blok mimari, seri kaynak sınıfı, rail budget (REP-02 raporu), exact-part kararları, timing, açık sorular, kaba maliyet/alan. |
| G3 | Şema, ERC (CI), model provenanslı benzetim, pre-layout inceleme. |
| G4 | PCB, DRC, fab paketi ve bağımsız risk incelemesi. |
| G6 | Lisans, checksum, yeniden üretilebilirlik, iddia matrisi; referans aktarımlarının lock dosyaları. |

# 9. Sonuç ve ilk yürütme kararı

WF01 temel teslimatları ve CF-09 modelleri tamamlandı; ilerlemeyi bloke eden şey artık teknik değil, karar yetkisidir. İlk yürütme: kullanıcının üç kararı (D-G1-PORTFOLIO-001 onayı, port varsayımları, seri kaynak sınıfı A/C) ve paralelde Codex'in karar gerektirmeyen görevleri (AGENTS.md + CI, sources/ manifesti, README/sözlük, referans aktarımları). WF02–04 referans tasarımlarla parametrik ilerler; WF03 hesapları REP-02'de çalışır. WF05/06 gerçek tasarım faaliyetleri G1 (müşteri veya portfolyo yolu) ve G3 kabulüne bağlıdır. Çıktılar müşteri gereksinimi, portfolyo kararı, referans tasarım değeri, mühendislik çıkarımı ve varsayımı karıştırmamalıdır.
