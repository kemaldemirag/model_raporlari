**REP-01 Codex Görev Planlaması**

REP-01 | ESP32-S3 24V Interface Board

ESP32-S3 24V Interface Board için pozisyon bazlı yürütme planı

Kaynak: REP-01 İş Akışı Raporu v2.0 | Tarih: 26.10.2026 | v1 (16.10.2026) yerine geçer | Repo: github.com/kemaldemirag/esp32-s3-24v-interface-board

# Amaç ve yürütme kararı

Bu rapor, İş Akışı Raporu v2.0'daki WF01–WF06 iş paketlerini Codex'in yürütebileceği sıralı görevlere ve kopyalanıp kullanılabilir promptlara dönüştürür. İş paketleri ayrı pozisyonlar olarak korunur; yeni pozisyon eklenmemiştir. v1'in WF01 envanteri ve CF-09 model çalışmaları tamamlanmıştır; v2 yürütmesi karar gerektirmeyen görevlerle (AGENTS.md, CI, kaynak manifesti, referans aktarımları) başlar. WF02–WF04 referans tasarımlara bağlı parametrik taslak seviyesinde yürütülür. G1 halen BLOCKED olduğundan WF05 şematik üretimi, G3 PASS olmadan da WF06 PCB yerleşimi başlamaz; WF05/WF06'da tasarımı kullanıcı yapar, Codex CI, araç çalıştırma ve inceleme üstlenir.

**Planlama ilkesi:** Codex mevcut dosyalardan ve sabit commit'li referanslardan iş üretir; her çıktı müşteri şartını, portfolyo kararını, referans tasarım değerini, türetilmiş sonucu, mühendislik kuralını ve bilinmeyeni ayırır. Karar kaydındaki alanları Codex doldurmaz; eksik bilgi UNKNOWN veya BLOCKED olarak kaydedilir.

# Durum ve kritik sınırlar

| **Durum / karar** | **Plan üzerindeki etkisi** |
| --- | --- |
| G0 PASS | Sürüm ve girdilerin güvenli başlangıç koşulları doğrulandı kabul edilir. |
| G1 BLOCKED | Müşteri yolu kapalı (yetkili müşteri yok). Portfolyo yolu: imzalı PORTFOLIO\_DECISION kayıtları + PKG-A/B lisans/provenans → PORTFOLIO\_ACCEPTED. Bu olmadan WF05 başlatılamaz. |
| G2–G6 NOT\_STARTED | Mimari, şema, PCB ve üretim paket kapıları sırayla kanıtlanmalıdır. |
| CF-09 / Q-C07 | MIRRORED\_5 PROPOSED (D-G1-PORTFOLIO-001); seri kaynak sınıfı (A: quad köprü, C: yardımcı MCU) UNKNOWN. Kullanıcı kararı olmadan topoloji kilitlenmez. |
| Tamamlanan iş | WF01 v1.2 teslimatları; CF-09 host, transport adapter ve bus scheduler modelleri (83/83 test, sentetik). |
| G3 PASS şartı | WF06 layout ancak G3 kanıt paketi kabul edildikten sonra başlar. |
| Ayrı onay gereken işlemler | Git commit/push, paket kurulumu, Drive yazma, silme ve mevcut dosyanın üzerine yazma için kullanıcı onayı gerekir. |

# Pozisyon haritası ve bağımlılık

| **Pozisyon** | **İş kapsamı** | **Başlama koşulu** |
| --- | --- | --- |
| WF01 | Requirement & Interface Contract + karar kaydı, port envanteri, sources/ manifesti, README/sözlük | Hemen; karar alanları kullanıcıda |
| WF02 | Power Entry Architecture (referans: TIDA-01167, PMP15023, vandal) | WF01 REQ-001; Test B taban kararı |
| WF03 | Power Tree & Regulator Design (hesap REP-02 aracında) | REQ-002, WF02 taslakları; REP-02 D5 intake |
| WF04 | MCU & Peripheral Integration (referans: Swiss Army Knife, esp\_msc\_ota, köprü datasheet'leri) | Port envanteri; seri kaynak sınıfı kararı (parametrik kısım hemen) |
| WF05 | Schematic & Pre-layout Verification (tasarım: kullanıcı; CI/inceleme: Codex) | G1 PASS veya PORTFOLIO\_ACCEPTED + G2 + WF01–04 |
| WF06 | PCB & Manufacturing Package (tasarım: kullanıcı; CI/manifest: Codex) | G3 PASS + WF05 çıktıları |

WF02–WF04 bilgi toplama bakımından paralel yürütülebilir; kabul sırası WF01 → WF02/WF03/WF04 → G1/G2 → WF05 → G3 → WF06.

# Ortak Codex çalışma sözleşmesi

- Repodaki AGENTS.md dosyasını her görevde oku ve uygula. Yoksa ilk görev onu oluşturmaktır.

- Mevcut dosyaları değiştirmeden önce listele ve planla. Yeni -v1.x dosyası açma; değişikliği mevcut dosyada yap, sürümü git tag ile işaretle.

- Dosya silme, üzerine yazma, Git commit/push, paket kurma veya Drive'a yazma yapma; gerekiyorsa işlem ve etkiyi raporla, onay bekle.

- Her iddiayı SOURCE\_EXPLICIT, DERIVED, ENGINEERING\_RULE, HANDOFF\_ONLY, REFERENCE\_DESIGN, PORTFOLIO\_DECISION veya UNKNOWN olarak etiketle. Kaynakta yoksa sayı, eşik, parça numarası ya da karar uydurma.

- Başarı raporunda PASS / BLOCKED / UNKNOWN / NOT\_RUN / PROPOSED / PORTFOLIO\_ACCEPTED kullan. Hiçbir kaydı kendiliğinden PASS'e yükseltme. Araç yoksa ERC, DRC, simülasyon veya fiziksel doğrulama yapılmış gibi gösterme.

- Referans repo kullanıyorsan sabit commit, dosya SHA-256, lisans ve satır/sayfa referansını references/<ad>/reference.lock.json içine yaz; değerleri script ile çıkar, elle yazma; çıktının iki çalıştırmada bayt bayt aynı olduğunu doğrula. İnternet erişimi yoksa kullanıcıdan referans klonunu ister ve NOT\_RUN raporlarsın.

- Her PR'da cd tests && python3 -m unittest çalıştır; mevcut testlerin tamamı geçmeli.

- Her görev sonunda değişen dosyaları, değişmeyen dosyaları, kanıtları, açık soruları ve sonraki güvenli adımı listele.

# WF01 — Requirement & Interface Contract

**Amaç:** Müşteri şartlarını, portfolyo kararlarını ve iç tasarım kurallarını izlenebilir gereksinimlere, arayüz sözleşmesine, karar kaydına ve açık karar listesine çevirmek.

**Girdiler:** REQ-001…005; CLAUDE-06/07; müşteri ilanları (SRC-L01); mevcut v1.2 dosyaları; D-G1-PORTFOLIO-001 taslağı; inceleme turları (25–26.10).

**Beklenen teslimatlar:** requirements-traceability.csv; interface-contract.md; gpio-budget.csv; open-decisions.md (yerinde güncelleme) + AGENTS.md; docs/decisions/; docs/serial-port-inventory.md; sources/MANIFEST + script; README, sözlük, karar indeksi; docx araştırmaların md karşılıkları.

## Görev adımları

1. AGENTS.md oluştur; testleri kök dizinden çalışır hale getir ve CI workflow'u ekle.

2. sources/ manifest script'i yaz: SHA-256, dosya yolu, kaynak URL; lisans sütunu boş (kullanıcı doldurur).

3. README, sözlük ve karar indeksini yalnız mevcut dosyalardan derle; boşlukları UNKNOWN bırak.

4. Araştırma ve karar docx'lerini markdown'a çevir; her md başına kaynak docx yolu ve hash'i yaz; orijinalleri silme.

5. 11 portluk seri port envanteri iskeletini REQ ve CSV'den üret; değer sütunlarını boş bırak.

6. D-G1-PORTFOLIO-001'i Tur 2/3 düzeltmeleriyle PROPOSED olarak güncelle; onay satırı kullanıcıya ait.

## Bloklayıcılar ve sınırlar

G1 halen BLOCKED; gereksinim dosyalarının anlamını onaysız değiştirme. CF-09 seri kaynak sınıfı kullanıcı kararı olmadan kilitlenmez. Karar kaydı ve port varsayımı değerleri Codex tarafından doldurulmaz.

## Pozisyon kabul ölçütü

Her gereksinim kaynak ve kanıt sınıfına bağlıdır; müşteri, portfolyo ve referans kaynaklı satırlar ayırt edilebilir; CI yeşil; manifest script'i iki çalıştırmada aynı çıktıyı verir.

## Codex prompt zinciri

### Prompt 1 — Repo ve kanıt envanteri

AGENTS.md'yi oku (yoksa oluştur: durum kodları, cd tests && python3 -m unittest, dokunulmayacak dosyalar: evidence/\*.xlsx ve docs/decisions/, yeni -v dosyası yasağı). Testleri repo kökünden çalışır hale getir ve GitHub Actions CI ekle. sources/ için SHA-256 manifest script'i yaz; lisans sütununu boş bırak. Değiştirdiğin ve değiştirmediğin dosyaları, CI sonucunu ve G1 için eksik kanıtları raporla.

### Prompt 2 — Gereksinim ve arayüz taslakları

Yalnız repodaki dosyalardan README (amaç, mevcut çıktı, nasıl doğrulanır, MODEL\_ONLY/NOT\_RUN ayrımı), sözlük (WF, CF, Q, PKG, JOB, REP, G kodları) ve karar indeksi üret. docs/\*.docx araştırma ve karar paketlerini md'ye çevir, orijinali koru. docs/serial-port-inventory.md iskeletini 11 port için oluştur (yön ve REQ bağlantısı dolu, değer alanları boş + ilgili Q-ID). Değer uydurma; yalnız ID ve bağlantı ver, başka dosyadaki değerleri kopyalama.

### Prompt 3 — CF-09 ve G1 durum değerlendirmesi

G1 kriterlerini iki yol için ayrı değerlendir: müşteri yolu ve portfolyo yolu. Her kriteri PASS / BLOCKED / UNKNOWN / PROPOSED olarak, kanıt dosyası ve SHA ile listele. D-G1-PORTFOLIO-001'in durumunu oku; imzasızsa PROPOSED bırak. CF-09 seri kaynak sınıfı kararının var olup olmadığını kontrol et. Hiçbir durumu yükseltme; yalnız eksikleri ve sahiplerini (kullanıcı / Claude / Codex) raporla.

# WF02 — Power Entry Architecture

**Amaç:** 24 V giriş için koruma mimarisi alternatiflerini kanıt sınıflarıyla ve referans tasarımlarla kıyaslamak; koşullu seçenekleri sunup kesin parça seçimini ertelemek.

**Girdiler:** REQ-001; Q-C01…03; CF-01/06; Test B taban kararı (PROPOSED); referans tasarımlar: TI TIDA-01167, TI PMP15023, blinken/vandal.

**Beklenen teslimatlar:** power-entry-options.md; protection-comparison.csv; transient-scenarios.json; references/vandal/ lock dosyası.

## Görev adımları

1. WF01'deki 24 V VIN aralığı ve koruma şartlarını al; eksikleri kaydet.

2. vandal referansını sabit commit'te aktar; giriş koruma ve buck bloklarının parça listesini KiCad dosyalarından script ile çıkar.

3. M1 TVS + uygun buck, M2 yüksek gerilim sınıfı buck + koruma ve M3 aktif OV kesme/sınırlama + TVS seçeneklerini karşılaştır; her seçeneğe bir referans tasarım bağla.

4. Her seçeneği transient, SOA, termal, alan ve maliyet eksenlerinde kaynaklı değerlendir.

5. Test B (58 V) ve opsiyonel Test A için sentetik senaryo tanımla; standart dalga şekli olmadığını işaretle.

6. G1/G2'ye bağlanan karar kaydı taslağı üret (kullanıcı onayına).

## Bloklayıcılar ve sınırlar

OEM darbe profili ve VIN min/max portfolyo kararı olmadan exact-part seçme veya PASS verme. Referans tasarım değerleri kendi tasarımın gibi sunulmaz; REFERENCE\_DESIGN etiketi taşır. Simülasyon yapılmadıysa plan veya sentetik senaryo olarak adlandır.

## Pozisyon kabul ölçütü

Üç mimari seçenek aynı ölçütlerle kıyaslanmıştır; her satır kaynak/referans/varsayım sınıfını taşır; referans aktarımı tekrar üretilebilir; exact part kararı verilmez.

## Codex prompt zinciri

### Prompt 1 — Girdi uygunluğu

WF02 için kaynak ve gereksinimleri incele. Q-C01…03 ve Test B taban kararının durumunu oku. Karar verilebilen ve verilemeyenleri ayır. references/vandal/ yoksa github.com/blinken/vandal'ı sabit commit'te aktar: lock dosyası (commit, lisans, dosya hash'leri) ve giriş/güç bloklarının parça listesini script ile çıkar. İnternet yoksa NOT\_RUN raporla.

### Prompt 2 — Alternatiflerin parametrik karşılaştırması

M1, M2 ve M3 için protection-comparison.csv ve power-entry-options.md taslaklarını üret. Transient dayanımı, SOA, termal, alan ve maliyet alanları olsun; her satırda kaynak sınıfı ve varsa referans tasarım bağlantısı (vandal lock, TIDA-01167 doküman numarası) bulunsun. Kanıt yoksa UNKNOWN. Kesin parça numarası, PASS veya güvenlik iddiası verme.

### Prompt 3 — Sentetik senaryo ve karar kapısı

transient-scenarios.json içine yalnız test planı amacıyla Test B taban ve Test A opsiyon senaryolarını tanımla; bunları standart profili gibi sunma. Her senaryo için gerekli dalga şekli kaynağı, beklenen gözlem ve eksik girdiyi belirt. Hangi kararların kullanıcı onayı beklediğini ve G1/G2'ye taşınacak kanıtları raporla.

# WF03 — Power Tree & Regulator Design

**Amaç:** 5 V ve 3,3 V güç ağacının yük, kayıp, derating ve sıralama bütçesini REP-02 aracıyla hesaplanabilir ve kanıtlanabilir hale getirmek.

**Girdiler:** REQ-002; WF02 taslakları; Q-C04…06; REP-02 power-rail-validation-and-review sürümü ve D5 intake sözleşmesi.

**Beklenen teslimatlar:** rail-budget.csv; regulator-candidates.csv; sequencing.csv; thermal-budget.md; simulation-plan.md + REP-02 intake descriptor, REP-02 JSON/Markdown raporu ve hash bağları.

## Görev adımları

1. Her güç rayı için minimum/tipik/tepe yükleri ve kaynaklarını çıkar.

2. REP-02'nin sabit sürümünü bağımlılık kaydına yaz (sürüm, wheel veya commit hash'i).

3. Yük ve regülatör adaylarını REP-02 intake descriptor'ına dönüştür; kaynaksız alanları UNKNOWN bırak.

4. REP-02 CLI ile rail-budget, sequence ve review komutlarını çalıştır; raporları kanıt klasörüne yaz.

5. PGOOD/RESET ve kararlılık kanıtlarını transientten ayrı kalemler olarak tut.

6. Ngspice veya SPICE modeli varsa sürüm ve model provenansını kaydet; yoksa NOT\_RUN.

## Bloklayıcılar ve sınırlar

PKG-A CM5 yükünü JOB-01'e kopyalama. Yük akımları eksikse sonuç UNKNOWN. REP-02 şema/sürüm uyumsuzluğu BLOCKED. Kararlılığı transient hesabından kanıtlamış sayma.

## Pozisyon kabul ölçütü

REP-02 raporu girdi hash'iyle REP-01'e bağlıdır; worst-case hesap izi ve varsayımlar görünürdür; PGOOD/RESET, kararlılık ve transient kanıtları ayrıdır; exact-part kararı verilmemiştir.

## Codex prompt zinciri

### Prompt 1 — Ray yük modeli

WF03 girdilerinden 5 V ve 3,3 V yüklerini ayıkla: USB-A VBUS, LCD/arka ışık, RS-232 transceiver, TTL tampon, köprü/yardımcı MCU ve ESP32-S3. Her yük için min/tipik/tepe, kaynak ve kanıt sınıfı yaz; kaynaksız yükleri UNKNOWN bırak. rail-budget.csv'yi yalnız ID ve kaynak bağıyla üret; hesaplamayı burada yapma.

### Prompt 2 — Regülatör ve termal hesap planı

REP-02'nin sabit sürümünü bağımlılık kaydına yaz. rail-budget.csv ve regulator-candidates.csv'yi REP-02 D5 intake descriptor'ına dönüştür ve intake komutuyla doğrula. Eksik datasheet veya model için REP-02'nin artifact-gap kaydını kullan. Sayısal sonuç uydurma, exact-part seçme.

### Prompt 3 — Sıralama ve simülasyon planı

REP-02 CLI ile rail-budget, sequence (VIN\_VALID → 5 V → 3,3 V → RESET\_N) ve review komutlarını çalıştır; JSON ve Markdown raporlarını hash'leriyle kanıt klasörüne yaz. Brownout ve power-off için inject-fault senaryolarını ekle. Simülatör/model yoksa NOT\_RUN. Hangi kanıtın G2'ye, hangisinin G3'e gireceğini ayır.

# WF04 — MCU & Peripheral Integration

**Amaç:** ESP32-S3-WROOM-1-N8 pinleri, boot/strapping, USB host, seri kanallar, seri kaynak sınıfı ve güç alanları için çakışma ve durum matrisi kurmak; CF-09 modellerini senaryo bazlı çalıştırmak.

**Girdiler:** REQ-003/004; CLAUDE-03; WF01 GPIO, arayüz ve port envanteri; ESP32-S3 TRM/datasheet; CF-09 modelleri; referanslar: ESP32 Serial Swiss Army Knife, espressif/esp\_msc\_ota, nopnop2002/esp-idf-sc16is750, openwch/ch32v307, espressif/esp-hosted; MAX14830, CH9434, SC16IS752/762, TRS3238E datasheet'leri.

**Beklenen teslimatlar:** pin-allocation.csv; usb-host-contract.md; serial-architecture.md; power-domains.md; boot-reset-checklist.md + serial-source-comparison.md; firmware-architecture.md; senaryo başına SPI kullanım tablosu.

## Görev adımları

1. WROOM-1-N8 pin/strapping bilgilerini birincil dokümandan doğrula; UART0 ROM boot logu kontrolünü (Q-UART0) ayrı satırda kaydet.

2. USB-A host VBUS, akım sınırlama/kapama ve ESD sözleşmesini yaz; esp\_msc\_ota'yı S3 MSC host referansı olarak kaydet.

3. Seri kaynak sınıfı A–D için parametrik kıyas tablosu üret (kanal, FIFO birimi, max baud, host saat, reset/IRQ, ESP-IDF sürücü, fiyat kaynağı).

4. CF-09 modellerini köprü kanal sayısı, FIFO boyutu ve SPI saatiyle parametrik hale getir; mevcut 83 testi koru.

5. Güç kapalı durumda USB/RS-232 geri besleme yollarını ve TRS3238E FORCEOFF davranışını analiz et.

6. Firmware mimarisi taslağını referans firmware'in aşama ve güvenli varsayılan yapısından türet.

## Bloklayıcılar ve sınırlar

Seri kaynak sınıfı kullanıcı kararı olmadan topoloji seçme. PHY/JTAG davranışını TRM ile doğrulamadan sabitleme. RMT belleğini (48 word) UART FIFO gibi kullanma; ESP32-S3 UART FIFO 128 byte. Linux sürücüsü ESP-IDF sürücüsü yerine geçmez.

## Pozisyon kabul ölçütü

Pin tahsisi kaynaklıdır veya açık blocker gösterir; boot, aktif, reset ve power-off durumları tanımlıdır; kıyas tablosunun her hücresi datasheet sayfasına bağlıdır; CF-09 testleri geçer ve senaryo tablosu üretilir.

## Codex prompt zinciri

### Prompt 1 — Pin ve boot kanıtı

ESP32-S3-WROOM-1-N8 için pin, strapping, USB OTG/USB Serial-JTAG ve UART davranışını birincil TRM/datasheet üzerinden doğrula. pin-allocation.csv ve boot-reset-checklist.md taslaklarını hazırla; her pine kaynak ve sayfa referansı yaz. Q-UART0 için ROM boot log kontrolünün (eFuse/GPIO46) belgede nerede geçtiğini bul; bulamazsan UNKNOWN.

### Prompt 2 — USB ve seri mimari sözleşmesi

usb-host-contract.md ve serial-architecture.md taslaklarını üret; serial-source-comparison.md içinde A (2 UART + quad köprü), B (3 UART + dual köprü), C (yardımcı MCU), D (iki dual köprü) senaryolarını aynı ölçütlerle kıyasla; datasheet'e bağlanamayan hücreyi UNKNOWN bırak. tests/ altındaki CF-09 modellerine köprü kanal sayısı, FIFO ve SPI saati parametrelerini ekle; senaryo başına SPI kullanım tablosu üret; mevcut 83 test geçmeli. Seçim yapma.

### Prompt 3 — Güç alanı, geri besleme ve risk matrisi

power-domains.md içinde açılış, aktif, brownout, reset ve power-off senaryolarını çıkar. USB ve RS-232 geri besleme, boot sırasında TX sahte start biti, UART0 ROM logu ve OE sıralamasını risk olarak kaydet. firmware-architecture.md taslağını referans firmware'in aşamalı yapısı ve güvenli varsayılanlarından türet; referansı REFERENCE\_DESIGN olarak etiketle. Tasarım kilitleme kararı verme.

# WF05 — Schematic & Pre-layout Verification

**Amaç:** G1 ve G2 kabulünden sonra kullanıcının çizdiği şematiği CI'da doğrulamak ve model provenanslı pre-layout kanıtlarını hazırlamak.

**Girdiler:** G1 PASS veya PORTFOLIO\_ACCEPTED; G2 mimari kabulü; WF01–WF04 kabul edilmiş girdiler; kullanıcının KiCad şematiği; referanslar: KiBot, aklofas/kicad-happy, OLIMEX/ESP32-GATEWAY, sx16is752-breakout.

**Beklenen teslimatlar:** ERC raporu (CI); BOM; simulation-evidence; prelayout-review.md; G3 kanıt paketi; KiBot/CI workflow.

## Görev adımları

1. Başlangıçta G1/G2 ve WF01–WF04 onay durumunu kontrol et; herhangi biri geçmemişse STOP ve blocker raporu ver.

2. Kabul edilmiş mimariyi şematik bloklarına eşleyen kontrol listesini üret (şematiği kullanıcı çizer).

3. KiBot yapılandırmasıyla ERC, PDF şematik ve BOM'u CI'da üret; sembol/footprint provenansını raporla.

4. kicad-happy ile inceleme çalıştır; bulguları bağımsız inceleme yerine geçmeyen ek kanıt olarak kaydet.

5. Hot-loop, dönüş yolu, decoupling, anten keepout, termal ve EMC riskleri için pre-layout incelemesi yap.

6. G3 kanıtlarını ve bağımsız kritik bulgu kontrol listesini hazırla.

## Bloklayıcılar ve sınırlar

G1 BLOCKED iken şematik dosyası oluşturma. Kullanıcının tasarım dosyalarını onaysız değiştirme. ERC başarısı EMC veya loop stability kanıtı değildir. Araç ya da model yoksa NOT\_RUN.

## Pozisyon kabul ölçütü

G1/G2 geçiş kanıtları mevcut; ERC ve simülasyon gerçek çalıştırma durumuyla CI logunda; model kökeni izlenebilir; bağımsız kritik bulgular kapatılmış veya açık blocker'dır.

## Codex prompt zinciri

### Prompt 1 — Giriş kapısı denetimi

WF05 başlatmadan önce G1 (PASS veya PORTFOLIO\_ACCEPTED), G2 kabulü ve WF01–WF04 çıktılarının durumunu denetle. Her kapının kanıt dosyasını ve SHA'sını listele. Herhangi biri eksik veya BLOCKED ise hiçbir tasarım dosyasına dokunma; yalnız blocker, gerekli kanıt ve güvenli sonraki adımı raporla.

### Prompt 2 — Onaylı şematik çalışma planı

Kabul edilmiş arayüz ve güç mimarisini şematik bloklarına eşleyen kontrol listesini üret. Kullanıcının şematiği için KiBot yapılandırması ve CI workflow'u ekle (ERC, PDF, BOM). Sembol/footprint/model provenansını ve BOM kaynaklandırmasını raporla. Şematik dosyalarını değiştirme; önerileri ayrı listele.

### Prompt 3 — ERC, simülasyon ve G3 kanıtı

CI'da ERC'yi çalıştır; KiCad/KiBot sürümünü, raporu ve açık ihlalleri kaydet. kicad-happy incelemesini çalıştır ve bulguları ayrı bölümde raporla. Simülasyonda model provenansını belirt; yoksa NOT\_RUN. prelayout-review.md'de hot-loop, dönüş yolu, decoupling, anten keepout, termal ve EMC risklerini ayır. G3 için eksik kanıtları listele.

# WF06 — PCB & Manufacturing Package

**Amaç:** G3 PASS sonrasında kullanıcının çizdiği 4 katmanlı PCB için üretim paketini CI'da yeniden üretilebilir kontrollerle hazırlamak.

**Girdiler:** G3 PASS; onaylı WF05 şematik ve bileşen kaynakları; kullanıcının PCB dosyası; üretici stackup/üretim sınıfı ve anten kısıtları.

**Beklenen teslimatlar:** DRC raporu; Gerber; NC drill; CPL; BOM; STEP; release checksums; DFM ve maliyet değerlendirmesi.

## Görev adımları

1. G3 PASS ve girdilerin değişmediğini kontrol et; değilse STOP. ≤64×65 mm ve anten keepout kısıtlarını kontrol listesine bağla.

2. Yerleşim/routing kararları için inceleme listesi üret (layout kullanıcıda).

3. KiCad DRC'yi CI'da çalıştır ve tüm hataları sınıflandır.

4. Gerber, drill, CPL, BOM ve STEP dosyalarını KiBot ile aynı revizyondan üret.

5. Dosya bütünlüğü için checksum manifesti ve yeniden üretim talimatlarını ekle.

6. 100 adet PCBA için 10–15 USD hedefini fiyatlandırılmış BOM ile karşılaştır; fizibilite onayı olarak sunma.

## Bloklayıcılar ve sınırlar

G3 olmadan layout yok. Üretici stackup, assembly class veya maliyet teklifi yoksa ilgili sonuç UNKNOWN. Açıklanmamış DRC hatasıyla üretim release'i verme. Fiziksel test veya otomotiv sertifikası iddia etme.

## Pozisyon kabul ölçütü

G3 kanıtı PASS ve girdi sürümü sabittir; DRC'de açıklanmamış hata yok; üretim çıktıları aynı revizyondan ve checksum'lıdır; maliyet hedefi güncel kaynaklarla değerlendirilmiş veya UNKNOWN'dur.

## Codex prompt zinciri

### Prompt 1 — G3 ve release girdisi kapısı

WF06'ya başlamadan G3 PASS kanıtını, WF05 şematik revizyonunu, üretici stackup/anten keepout ve montaj sınıfı girdilerini doğrula. Her dosya/revizyon/hash için manifest hazırla. G3 PASS değilse veya girdiler değişmişse üretim dosyası üretme; yalnız blocker raporla.

### Prompt 2 — Layout ve DRC planı

Kullanıcının PCB dosyası için yerleşim/routing inceleme listesi üret (hot-loop, dönüş yolu, decoupling, RF/anten keepout, termal, konektör erişimi). DRC'yi CI'da çalıştır; sürüm, ayarlar, rapor ve istisnaları sakla; açıklanmamış hataları release blocker yap. PCB dosyasını değiştirme.

### Prompt 3 — İmalat paketi ve maliyet kontrolü

Onaylı PCB revizyonundan KiBot ile Gerber, NC drill, CPL, BOM ve STEP üret; hepsini SHA-256 manifestiyle eşleştir. DFM kontrollerini ve yeniden üretim adımlarını yaz. 100 adet PCBA hedefini fiyatlandırılmış BOM ile kıyasla; fiyat verisi yoksa UNKNOWN. Fiziksel test veya sertifika iddiası ekleme.

# Codex yürütme sırası

| **Sıra** | **Görev / durum** | **Çıkış koşulu** |
| --- | --- | --- |
| 0 | AGENTS.md, kök dizinden test, CI (WF01 Prompt 1) | CI yeşil; 83/83 test. |
| 1 | WF01 ekleri: sources/ manifesti, README/sözlük/indeks, docx → md, port envanteri iskeleti | Manifest deterministik; iskelette değer uydurulmamış. |
| 2 | Referans aktarımları (vandal, gerekirse diğerleri) ve WF02 parametrik kıyas | Lock dosyaları ve bayt bayt aynı yeniden üretim. |
| 3 | Kullanıcı kararları: D-G1 onayı, port varsayımları, seri kaynak sınıfı | Karar kaydı imzalı (Codex beklemede). |
| 4 | WF04 parametrik modeller ve kıyas; WF03 REP-02 intake ve raporları | Senaryo SPI tablosu; REP-02 raporu hash'li. |
| 5 | G1 (portfolyo yolu) ve G2 kabul değerlendirmesi | PORTFOLIO\_ACCEPTED veya açık blocker listesi. |
| 6 | WF05 CI/ERC/inceleme (şematik kullanıcıda) | G3 kanıt paketi. |
| 7 | WF06 CI/DRC/üretim paketi (layout kullanıcıda) | Checksum'lı release; G3 PASS ve sabit girdiler. |

# Kapanış raporu şablonu

Her Codex görevi sonunda aşağıdaki özeti iste:

- Görev ve iş paketi: WFxx / adım numarası

- Sonuç: PASS, BLOCKED, UNKNOWN, NOT\_RUN veya PROPOSED

- Değiştirilen/oluşturulan dosyalar ve sürümleri; dokunulmayan dosyalar

- Çalıştırılan araçlar, sürüm, test sayısı ve CI/kanıt konumları

- Kullanılan referanslar: commit, lisans, lock dosyası

- Açık kullanıcı kararları ve teknik belirsizlikler

- Kapı etkisi: G1/G2/G3/G4/G6 durumu

- Bir sonraki güvenli görev; başlaması için gereken koşullar

# Kaynakla uyum notu

Plan, REP-01 İş Akışı Raporu v2.0'daki altı WF'yi, teslimat adlarını, açık soruları ve kapı koşullarını korur; v1 başlık yapısı değiştirilmemiştir. v2 eklemeleri: portfolyo ve referans kaynak sınıfları, referans aktarım kuralı, REP-02 entegrasyonu, WF05/WF06 rol ayrımı ve tamamlanan işin duruma işlenmesi. Bu görev planı müşteri onayı, portfolyo kararı, kaynak doğrulaması, kesin bileşen seçimi, devre/PCB tasarımı veya fiziksel doğrulama yerine geçmez. Codex promptları taslak üretimi, araç çalıştırma ve kanıt yönetimi içindir; kapı koşulları aşılmaz.

Codex görev planlaması • 26.10.2026
