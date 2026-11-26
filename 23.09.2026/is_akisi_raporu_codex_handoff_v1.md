**23.09.2026**

# İŞ AKIŞI ÜRETİM RAPORU

**Codex Handoff v1.0 üzerinden türetilmiş yürütme planı**

| **Alan** | **Tanım** |
| --- | --- |
| Kaynak | 23.09.2026\_Codex\_Handoff\_v1(1).docx |
| Yürütme modu | PLAN\_AND\_DOCUMENT |
| Ürünler | P1 — Reconfigurable Measurement & Perception Workbench<br>P2 — Industrial Electronics Design & Verification Blueprint |
| Temel amaç | İki ürünü ayrı tutarak standart çalışma alanına eşlemek; spesifikasyon taslakları, artefakt yolları, referans/karar kayıtları ve doğrulama planı üretmek. |
| Bu raporun statüsü | Uygulama veya fiziksel doğrulama sonucu değil; uygulanabilir iş akışının ve karar kapılarının çalışma planıdır. |

| **Temel yorum —** Handoff, ürün kapsamını yeniden keşfetmeyi değil, mevcut kaynak kapsamını çalışma alanında yürütülebilir hale getirmeyi istiyor. Bu nedenle ürün çıkarımı tekrar başlatılmıyor; açık v1 kararları ayrı karar kayıtlarına taşınıyor. |
| --- |

## 1. Yönetici Özeti

Bu iş akışı iki bağımsız ürün için tek bir ortak çalışma alanı standardı kullanır; fakat ürün kökleri, gereksinimler, artefaktlar ve kanıtlar birbirinden ayrıdır. Handoff’un ana problemi ürün fikri bulmak değil, mevcut ürün bağlamını kaybetmeden spesifikasyon–artefakt–görev–kanıt zincirine dönüştürmektir.

- P1, sensör/telemetri/sentetik girdileri doğrulama, yorumlama, işleme, özellik çıkarımı ve detector/decision zinciriyle event/perception ve diagnostic çıktılara dönüştüren yeniden yapılandırılabilir bir yazılım çalışma tezgâhıdır.

- P2, elektronik geliştirmeyi gereksinimden tasarıma, doğrulamaya, DFM/DFT’ye ve üretim/handoff paketine taşıyan yeniden kullanılabilir mühendislik blueprint’idir; belirli bir kart P2’nin kendisi değil, bir design-case konfigürasyonudur.

- İş akışının gerçek başlangıç işi H0–H3 paketleridir: kaynak/hedef doğrulama → ürün sınırlarını belgeye çevirme → artefaktları standarda eşleme → READY/BLOCKED görev paketlerini üretme.

- 16 ürün aşaması korunur: P1 için 8, P2 için 8. H0–H3 bu aşamaların yerine geçmez; onları çalıştırılabilir hale getiren hazırlık katmanıdır.

- Açık karar tüm zinciri durdurmaz. Yalnız ilgili karara bağımlı görev BLOCKED olur; kaynak kayıtları, sınır belgeleri, artefakt haritası ve diğer hazır görevler ilerler.

## 2. Yürütme İlkeleri ve Sınırlar

| **İlke** | **Uygulama** |
| --- | --- |
| Kaynak otoritesi | Ürün kapsamı Handoff’un referans verdiği kaynaklardan korunur; öneri, kullanıcı onayı veya gerçekleşmiş uygulama gibi işaretlenmez. |
| Tek asıl kopya | Üretilen mühendislik varlıklarının asıl kopyası 002-Proje Çıktıları altındaki ilgili ürün kökünde tutulur. Dokümantasyon alanı bağımsız ikinci asıl kopya üretmez. |
| R/O/E ayrımı | R = incelenen referans, O = üretilen mühendislik varlığı, E = çalıştırma/doğrulama kanıtı. |
| Ürün bağımsızlığı | P1 ve P2 ortak CAN/sensör/doğrulama kavramları taşısa da tek ürün, ortak repo veya zorunlu entegrasyon sayılmaz. |
| Kanıt sınırı | Plan, checklist, boş klasör, ERC/DRC taslağı veya sanal test tek başına fiziksel doğrulama/PASS değildir. |
| Karar izolasyonu | Sürüm kapsamı, performans eşikleri, dil/framework, ECAD aracı, kart sınıfı gibi açık seçimler DECISIONS kaydında tutulur; yalnız bağımlı görevleri bloklar. |

## 3. Standart Çalışma Alanına Eşleme

Handoff’taki standart kabuk korunur. İş akışı raporu bu kabuğu yeniden tasarlamaz; hangi artefaktın hangi sorumluluk alanına gideceğini tarif eder.

| **Kısaltma** | **Hedef alan** | **Sorumluluk** |
| --- | --- | --- |
| N | 001-Kayıt Notları ve Dokümantasyon / 001-Kayıt Notları | Handoff, context baseline, kaynak indeksi, karar kaydı, çalışma alanı planı, artefakt haritası, görev paketleri, handoff receipt. |
| D | 001-Kayıt Notları ve Dokümantasyon / 002-Proje Dokümantasyonu | Ürün rehberi, indeksler, okunabilir dışa aktarımlar. Repo belgelerinin ikinci asıl kopyası değildir. |
| R | 001-Kayıt Notları ve Dokümantasyon / 003-Referans Kaynaklar | Oynatma listeleri, PDF, referans repo, müşteri referansı, iş ilanı. REFERENCE-REGISTRY.json bu alanın kayıt katmanıdır. |
| O | 002-Proje Çıktıları | P1 ve P2 için ayrı ürün kökleri; gereksinim, mimari, sözleşme, tasarım/kod, test ve doğrulama belgelerinin asıl sürümlü kopyası. |
| E | 003-Proje Kayıtları | Görsel, teknik çıktı ve video kanıtları; evidence matrix koşu kimliği ve dosya özetiyle buraya bağlanır. |

| **Çalışma alanı kapısı —** Canlı Drive kökü doğrulanmadan mevcut uzak klasörlere yazılmaz. Gerekirse yerel staging üretilir ve bu sonuç “Drive’a kurulmuş çalışma alanı” diye raporlanmaz. |
| --- |

## 4. P1 — Reconfigurable Measurement & Perception Workbench

### 4.1 Ürün sınırı

P1’in çekirdeği adapter → contract → validation → interpretation → processing → diagnostics → features → detector/decision → event/perception model → output zinciridir. Replay/scenario motoru bu zinciri besleyen yatay doğrulama sorumluluğudur.

| **Kapsam birimi** | **Kaynakta korunacak işlev** | **v1 için açık karar** |
| --- | --- | --- |
| ROS 2 / LaserScan | Açı–indeks eşleme, sektör seçimi, zaman/range geçerliliği, proximity/obstacle özellikleri, diagnostics. | Sektörler, eşikler, arayüz ayrıntıları ve v1 üyeliği. |
| CAN / DBC | Encode/decode, scale/offset, validity, heartbeat/timeout, telemetri, fault/event zinciri. | DBC, sinyal seti, fault davranışı ve v1 üyeliği. |
| Generic Event Detection | Baseline/residual; threshold, peak, change-point; hysteresis/persistence/debounce/confidence. | İlk detector kümesi ve kabul örnekleri. |
| Replay / Scenario | Kayıtlı/sentetik iz; invalid, stale, drop, stuck senaryoları; golden-output karşılaştırması. | Karşılaştırılacak alanlar, toleranslar ve replay girdi biçimi. |

### 4.2 P1 sekiz aşamalı iş akışı

| **ID** | **Aşama** | **Ana çıktı** | **Çıkış kriteri** | **Kanıt** |
| --- | --- | --- | --- | --- |
| P1-01 | Requirements & Boundary | requirements/requirements.md; product-boundary.md; use-cases.md; release-scope.md | Ürün girdileri/çıktıları, kullanıcı/consumer, sınırlar ve v1 üyeliği açık; bilinmeyenler karar kaydında. | Doküman incelemesi |
| P1-02 | Architecture | architecture/architecture.md; interface-map.md; measurement-to-decision.mmd | Validation, interpretation, diagnostics, features ve decision sorumlulukları izlenebilir; profil ile çekirdek ayrımı açık. | Mimari/arayüz incelemesi |
| P1-03 | Contracts | contracts/\*.schema.json; profile-mappings/\*.md; sample-configs/ | Measurement/event/diagnostic alanları, birim ve zaman semantiği taslak sözleşmede; açık seçimler etiketli. | Schema örnek doğrulaması + doküman |
| P1-04 | ROS2 / LaserScan Profile | profiles/ros2\_laserscan/; tests/profile/laserscan/; examples/laserscan/ | Profil sözleşmesi ve test senaryoları tanımlı; uygulama varsa sentetik LaserScan ile tekrarlanabilir koşu. | Unit/profile test + replay log |
| P1-05 | CAN / DBC Profile | profiles/can\_dbc\_telemetry/; tests/profile/can\_dbc/; golden/can\_frames/ | DBC/sinyal kapsamı seçildiyse encode/decode/validity/timeout davranışı testlenebilir. | Contract/profile test + golden frames |
| P1-06 | Generic Event Detection | src/processors/; src/detectors/; tests/profile/timeseries/; golden/event\_labels/ | Seçilen detector kümesi, edge-case ve expected events ile tanımlı. | Unit/regression + golden labels |
| P1-07 | Replay / Faults | replay/; scenarios/; golden/; reports/regression/ | Invalid/stale/drop/stuck senaryoları tekrar çalıştırılabilir; farklar kayıtlı. | Replay koşusu + regression report |
| P1-08 | Evidence & Docs | docs/test-plan.md; limitations.md; evidence-matrix.md; new-profile-guide.md | Test planı, sınırlamalar, evidence matrix ve yeni profil ekleme rehberi tutarlı; PASS yalnız gerçek koşu varsa. | Belge kontrolü + koşu kanıtı |

### 4.3 P1 sürüm kapıları

- P1-v1’in A/B/C profillerinden hangilerini gerçekten teslim edeceği release-scope.md içinde açıkça seçilmeli.

- Measurement/event/diagnostic sözleşmeleri varsa mevcut sözleşmeden türetilmeli; yoksa taslak alan tanımları “öneri” statüsünde üretilmeli.

- Dil, runtime, ROS 2 sürümü, CAN/DBC kütüphaneleri ve bağımlılık sürümleri kaynakta verilmemişse uygulama dosyaları uydurulmamalı.

- Replay/golden motoru profil başına kopyalanmamalı; yatay tek sorumluluk olarak tasarlanmalı.

- SLAM/navigation, fiziksel safety fonksiyonu, zorunlu dashboard veya LaserScan→CAN gateway varsayılan kapsam değildir.

## 5. P2 — Industrial Electronics Design & Verification Blueprint

### 5.1 Ürün sınırı

P2’nin kalıcı ürünü tek bir kart değil; gereksinim, gerekçe, kontrol listesi, doğrulama ve handoff düzenidir. Somut sensör kartı, controller, gateway veya power-control board yalnızca design-cases/<case-id>/ altında bir konfigürasyon olur.

| **Kritik ayrım —** Kart sınıfı, gerilimler, MCU, konektör, ECAD aracı, çevre koşulları ve üretici kuralları kaynakla veya açık mühendislik varsayımıyla seçilmeden sayısal tasarım, parça seçimi ve layout aşamalarına geçilmez. |
| --- |

### 5.2 P2 sekiz aşamalı iş akışı

| **ID** | **Aşama** | **Ana çıktı** | **Çıkış kriteri** | **Kanıt** |
| --- | --- | --- | --- | --- |
| P2-01 | Requirements & Product Class | design-cases/<case>/requirements/requirements.md; assumptions.md; compliance-targets.md | Güç, I/O, haberleşme, ortam, üretim/test hedefleri tanımlı; örnek tasarım sınıfı seçilmiş veya BLOCKED. | Gereksinim incelemesi |
| P2-02 | Functional Architecture | architecture/functional-blocks.mmd; interface-control.md; power-tree.md | Blok giriş/çıkışları ve sorumlulukları gereksinimlere izlenebilir. | Mimari inceleme |
| P2-03 | Component Selection | component-selection/component-selection.md; bom/bom-draft.csv; alternates.csv | Kritik parçalar gereksinim ve risk gerekçesine bağlı; alternatifler kayıtlı. | Seçim matrisi/BOM kontrolü |
| P2-04 | Schematic & Electrical Verification | schematic/<native-project>; verification/erc-report; calculations/; simulation/ | Şema hazırsa ERC, hesap/simülasyon yapılmış; açık hatalar/varsayımlar kayıtlı. | ERC + hesap/simülasyon |
| P2-05 | PCB Architecture & Layout | pcb/<native-layout>; stackup.md; layout-rationale.md; verification/drc-report | Layout varsa DRC temiz veya belgeli istisna; kritik placement/routing gerekçeleri yazılı. | DRC + layout review |
| P2-06 | Integrity / Robustness | integrity/pi-si-review.md; emi-emc-checklist.md; risk-register.csv | PI/SI/EMI/ESD/robustness riskleri sınıflı; yüksek riskli netler ve önlemler izlenebilir. | Analiz/checklist/risk kaydı |
| P2-07 | DFM / DFT / Manufacturing | dfm-dft/dfm-report.md; dft-plan.md; manufacturing/gerber; drill; model.step; final-bom.csv | Üretim paketi kapsamı tanımlı; DFM/DFT bulguları ve istisnalar kayıtlı. | DFM/DFT + paket bütünlük kontrolü |
| P2-08 | Verification & Handoff | verification/verification-report.md; bring-up-plan.md; test-plan.md; handoff/<case>/package-index.json | Sanal/elektriksel kanıt ile açık fiziksel testler ayrılmış; handoff indeksi eksiksiz. | Verification report + package index |

### 5.3 P2 sürüm seçenekleri

| **Seçenek** | **İçerik** | **Durum** |
| --- | --- | --- |
| A — Blueprint-only | workflow, templates, review-checklists, docs/reuse guide; örnek case yok. | Kaynak yapısı buna izin veriyor; v1 için kullanıcı kararı gerekli. |
| B — Blueprint + reference design | Yeniden kullanılabilir blueprint + seçilmiş bir <case-id> tasarım örneği. | Kullanım sınıfı ve sayısal gereksinimler seçilmeden BLOCKED. |
| C — Specific board handoff | Belirli kartın tam tasarım ve manufacturing/handoff paketi. | Kart gereksinimleri, araçlar ve fiziksel doğrulama sınırı ayrıca kararlaştırılmalı. |

Öneri niteliğinde yürütme mantığı: kullanıcı v1 tercihi doğrulanana kadar blueprint şablonlarını ve kontrol yöntemlerini üretmek READY; sayısal kart tasarımı, parça seçimi ve layout ise OPEN\_DECISION’a bağlı BLOCKED tutulmalıdır.

## 6. Handoff → Çalışma Alanı İlk Yürütmesi (H0–H3)

H0–H3, ürün geliştirme aşamalarını değiştirmeden önce bağlamı güvenli ve izlenebilir bir çalışma alanına dönüştürür.

| **Paket** | **İş** | **Ana teslimatlar** | **Bitirme koşulu** | **Başlangıç durumu** |
| --- | --- | --- | --- | --- |
| H0 | Kaynak / hedef doğrula | S1/S2/görsel kaynakları ve varsa manifesti doğrula; çalışma alanı/staging kökünü belirle. | N/CONTEXT-BASELINE.md<br>N/SOURCE-INDEX.json | Kök kimliği, kaynak sürümleri, okuma kapsamı ve çelişkiler kayıtlı. |
| H1 | Ürün sınırını belgeye çevir | P1/P2 requirements altındaki boundary ve release-scope taslaklarını kaynakla doldur; açık seçimleri DECISIONS’a taşı. | P1/P2 requirements/\*<br>N/DECISIONS.json | Kaynakta olmayan v1 kararı uydurulmamış; onay durumu görünür. |
| H2 | Artefaktı standarda eşle | 16 aşamayı gerçek hedef yollar ve kanıt yöntemleriyle ARTIFACT-MAP’e bağla; referans registry ile ürün reference-map’lerini ilişkilendir. | N/WORKSPACE-PLAN.md<br>N/ARTIFACT-MAP.json<br>R/REFERENCE-REGISTRY.json<br>P1/P2 references/reference-map.json | Her aşamada tek asıl yol, kaynak bağı ve kanıt yöntemi var. |
| H3 | Görevleri ayır / kontrol et | Tek teslimatlı atomik görev paketleri üret; READY/BLOCKED ayrımı yap; belge/manifest tutarlılık kontrolleri yürüt. | N/TASK-PACKAGES.md<br>N/HANDOFF-RECEIPT.json | Yazma alanları, bağımlılıklar, karar referansları, kontrol sonuçları ve sıradaki tek görev açık. |

### 6.1 H0–H3 görev atomizasyonu

| **Paket** | **Odak** | **Durum mantığı** | **Kısıt** |
| --- | --- | --- | --- |
| H0 | Kaynak / hedef doğrula | READY; canlı Drive kimliği doğrulanmazsa yalnız staging. | Tek pakette ürün uygulaması yapılmaz; teslimat belge/manifest seviyesinde kalır. |
| H1 | Ürün sınırını belgeye çevir | READY; belirli seçimler OPEN\_DECISION olabilir. | Tek pakette ürün uygulaması yapılmaz; teslimat belge/manifest seviyesinde kalır. |
| H2 | Artefaktı standarda eşle | READY; gerçek kök belirsizse staging yolları. | Tek pakette ürün uygulaması yapılmaz; teslimat belge/manifest seviyesinde kalır. |
| H3 | Görevleri ayır / kontrol et | READY; çalıştırılmayan kontrol NOT\_RUN. | Tek pakette ürün uygulaması yapılmaz; teslimat belge/manifest seviyesinde kalır. |

## 7. READY / BLOCKED Görev Mantığı

| **READY — karar beklemeden ilerler** | **BLOCKED — ilgili karar/kanıt gerekir** |
| --- | --- |
| • Kaynak envanteri ve SOURCE-INDEX oluşturma. | • P1 için hangi profillerin v1’de tamamlanacağını seçmeden tam uygulama planını kesinleştirme. |
| • Çalışma alanı standardını Handoff yollarıyla eşleme. | • P1 runtime/dil/framework/bağımlılık sürümleri bilinmeden sahte kaynak kod dosyaları üretme. |
| • P1/P2 product-boundary ve release-scope taslaklarını kaynakta açık kısımlarla doldurma. | • P2 için kullanım sınıfı ve elektriksel gereksinimler seçilmeden parça, şema ve PCB tasarlama. |
| • 16 aşamalı ARTIFACT-MAP oluşturma. | • Gerçek ERC/DRC/simülasyon çalıştırılmadan PASS yazma. |
| • Referans kayıtlarını source\_id bazında tekilleştirme ve ürün reference-map’lerine bağlama. | • Fiziksel prototip/bring-up yapılmadan çalışan kart, EMC uygun veya saha-doğrulanmış iddiası üretme. |
| • Karar sorularını DECISIONS’a taşıma; karar verilmemiş alanları açık bırakma. | • Canlı Drive kökü doğrulanmadan uzak klasörlerde değişiklik yapma. |
| • TASK-PACKAGES içinde bağımsız belge/analiz görevlerini atomikleştirme. |  |

## 8. Açık Karar Kaydı — İlk Sürüm

| **Decision ID** | **Ürün** | **Konu** | **Soru** | **Etkilenen iş** |
| --- | --- | --- | --- | --- |
| D-P1-01 | P1 | v1 profil kapsamı | A/B/C profillerinden hangileri IMPLEMENTED hedefi alacak? | release-scope.md; P1-04/05/06/07 |
| D-P1-02 | P1 | Sözleşme | Measurement/event/diagnostic alanları ve birim/zaman semantiği nedir? | contracts; P1-03 |
| D-P1-03 | P1 | Teknoloji | Dil, runtime, ROS 2/CAN kütüphane ve bağımlılık sürümleri? | Uygulama/test görevleri |
| D-P1-04 | P1 | Kabul | Replay/golden karşılaştırma alanları ve toleransları? | P1-07/08 |
| D-P2-01 | P2 | v1 paket tipi | Blueprint-only, blueprint+case veya specific-board handoff? | release-scope; tüm design-case görevleri |
| D-P2-02 | P2 | Reference case | Kullanım sınıfı: sensor/controller/gateway/power-control vb.? | P2-01 sonrası |
| D-P2-03 | P2 | Elektriksel sınırlar | Power I/O, haberleşme, çevre/mekanik, test hedefleri? | P2-02..08 |
| D-P2-04 | P2 | Araçlar | ECAD/simülasyon/çıktı formatları ve üretici kuralları? | P2-04..07 |
| D-WS-01 | Ortak | Canlı kök | Hangi 23.09.2026 çalışma alanı kökü gerçek hedef? | H0-H3 canlı yazma |

## 9. İzlenebilirlik ve Kanıt Zinciri

Her iddianın minimum zinciri aşağıdaki kayıtları birbirine bağlamalıdır:

**source\_id → finding\_id → product\_function → requirement\_id → decision\_id → artifact\_path → test\_id → evidence\_id → claim**

| **Kayıt** | **Zorunlu alanlar / amaç** |
| --- | --- |
| SOURCE-INDEX / REFERENCE-REGISTRY | Kaynak kimliği, tür/platform, sürüm, incelenen bölüm, erişim tarihi, read\_state, discovery\_role ve evidence\_role. |
| DECISIONS | Soru, kaynak dayanağı, alternatif/öneri, onay durumu, karar sahibi ve etkilenen görevler. |
| ARTIFACT-MAP | Ürün, workflow\_stage, asıl hedef yol, bağımlılıklar, kabul kontrolü, kanıt yolu ve state. |
| TASK-PACKAGES | Tek teslimat, input/output yolları, depends\_on, decision\_refs, kabul kontrolü, izin verilen yazma alanı ve READY/BLOCKED nedeni. |
| HANDOFF-RECEIPT | Kök/staging, değişen dosyalar, çalıştırılan/çalıştırılmayan kontroller, açık kararlar ve sıradaki tek uygulanabilir görev. |

### 9.1 State modeli

Artefakt durumları PLANNED → DRAFT → IMPLEMENTED → VERIFIED şeklinde yükselir. Bir yolun varlığı veya bir test planının yazılmış olması IMPLEMENTED/VERIFIED anlamına gelmez. NOT\_RUN hiçbir durumda PASS değildir.

## 10. Kabul Kontrolleri

1. Standart üç ana klasör ve beş referans kategorisi doğru adlarla korunmuş mu?

2. P1 ve P2 ayrı kök, kapsam, karar ve kabul kayıtlarına sahip mi?

3. P1+P2 toplam 16 aşama en az bir asıl artefakt yoluna bağlandı mı?

4. P1’de LaserScan, CAN/DBC, Generic Event Detection ile replay/diagnostics sorumlulukları kaybolmadan gösterildi mi?

5. P2 ECAD/manufacturing varlıkları P1 replay/golden alanlarıyla karışıyor mu?

6. Rapor kapsamı ile v1 onayı ayrı statülerde mi?

7. Aynı mühendislik varlığının birden çok bağımsız “asıl kopyası” oluştu mu?

8. Eksik kaynak MISSING\_SOURCE, çelişki SOURCE\_CONFLICT, karar OPEN\_DECISION olarak açık mı?

9. Boş klasör/plan/test taslağı tamamlanmış uygulama diye raporlanıyor mu?

10. PASS yalnız gerçek çalıştırma kaydı ve evidence varsa mı yazılıyor?

11. Donanımsız doğrulamanın sınırı iddialarda açık mı?

12. Her BLOCKED görevin nedeni ve sıradaki unblocked iş görünür mü?

## 11. Önerilen Uygulama Sırası

Bu Handoff için en düşük riskli yürütme sırası aşağıdaki gibidir. Buradaki “önerilen” sözcüğü, kaynakta v1 onayı verildiği anlamına gelmez; işlerin bağımlılıklarına göre çalışma sırasını belirtir.

| **Sıra** | **Katman** | **İş** | **Çıktı** | **Neden** |
| --- | --- | --- | --- | --- |
| 1 | H0 | Kök/kaynak doğrulama | CONTEXT-BASELINE + SOURCE-INDEX | Canlı hedef belirsizliğini giderir; yanlış çalışma alanına yazmayı önler. |
| 2 | H1 | Sınır/release taslakları | P1/P2 boundary + release-scope + DECISIONS | Ürün kapsamı ile sürüm seçimini ayırır. |
| 3 | H2 | Artefakt haritası | WORKSPACE-PLAN + ARTIFACT-MAP + reference maps | 16 aşamayı gerçek çalışma alanı yollarına bağlar. |
| 4 | H3 | Atomik görevler | TASK-PACKAGES + HANDOFF-RECEIPT | READY/BLOCKED ayrımını ve sıradaki işi görünür kılar. |
| 5 | P1-01..03 / P2-01..02 | Spesifikasyon/mimari derinleştirme | Ürün gereksinimleri, mimari, sözleşmeler | Uygulama öncesi belirsizlikleri minimuma indirir. |
| 6 | Seçili uygulama aşamaları | P1 profilleri veya P2 reference case | Kod/ECAD artefaktları | Yalnız v1 kararları ve gereken girdiler mevcutsa başlar. |
| 7 | Doğrulama / evidence | Replay/regression veya ERC/DRC/simülasyon/DFM | Evidence + reports | Gerçek koşu sonucu olmayan iş PASS sayılmaz. |
| 8 | Handoff/Docs | Evidence matrix + package index + limitations | Teslim paketi | İddia sınırı ve kalan fiziksel testler açıkça belirtilir. |

## 12. Sonuç

Handoff, çalışma alanının hemen tam uygulamaya geçirilmesini değil, iki ürünün kaynak temelli sınırlarını koruyarak uygulanabilir bir mühendislik sistemine dönüştürülmesini tarif ediyor. Bu nedenle doğru ilk teslim “tamamlanmış iki ürün” değil; doğrulanmış kaynak/hedef kaydı, ürün başına release-scope taslağı, 16 aşamalı artefakt haritası, referans/karar kayıtları ve READY/BLOCKED görev paketleridir.

Bu rapora göre çalışma alanı üretimi başlatılabilir; ancak P1 v1 profil seçimi, P2 v1 paket tipi/reference-case seçimi ve canlı Drive kökü gibi kararlar açıkça kayıt altına alınmalı, bunlara bağımlı uygulama işleri karar verilene kadar BLOCKED tutulmalıdır. Diğer belge, kaynak eşleme ve planlama işleri ilerlemeye devam edebilir.

| **Sıradaki tek uygulanabilir görev —** H0: 23.09.2026 hedef çalışma alanı kökünü ve Handoff’un kaynak paketini doğrulayıp CONTEXT-BASELINE.md ile SOURCE-INDEX.json üretmek. Bu tamamlandıktan sonra H1–H3 aynı PLAN\_AND\_DOCUMENT sınırında yürütülür. |
| --- |

### Kaynak Notu

Bu rapor yalnızca kullanıcı tarafından sağlanan “23.09.2026\_Codex\_Handoff\_v1(1).docx” içeriğinden türetilmiştir. Handoff içinde S1–S5 olarak atıf yapılan kaynakların kendileri bu rapor için bağımsız biçimde yeniden doğrulanmamış; Handoff’un kaynak otoritesi, sınırları ve uyarıları olduğu gibi korunmuştur.
