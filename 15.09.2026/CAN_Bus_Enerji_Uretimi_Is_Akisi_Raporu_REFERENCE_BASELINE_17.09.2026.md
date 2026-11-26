# CAN Bus + Energy Generation Portfolio<br>İş Akışı Raporu

**REFERENCE BASELINE — 17.09.2026**

Kaynak: Codex Handoff — Revised 17.09.2026<br>**Handoff ID: CAN-ENERGY-PORTFOLIO-2026-09-15**<br>Operating principle: Gate-first, evidence-driven, limit-conscious execution<br>Status: GATE-DRIVEN / EVIDENCE REQUIRED / PHYSICAL VALIDATION PENDING

| **Raporun odak noktası:** Bu sürümde başlangıç noktası Phase 0 değildir. Mevcut workspace ve PROJECT-01 PR bağlamı korunur; Codex yalnız aktif kanıt boşluklarını en düşük yeterli araçla kapatır. |
| --- |

## 1. Yönetici Özeti

- Yeni Handoff, proje tanıtımından yürütme sözleşmesine geçmiştir. Codex’in görevi yeni alan keşfetmek değil, mevcut repository kanıtını yeniden kullanarak aktif gate’i kapatmaktır.

- PROJECT-01 / PR #1 / GATE-2 tek aktif yürütme odağıdır. Yakın dönem iş dört kapanış sözleşmesiyle sınırlıdır: GAP-08, GAP-03, GAP-02 ve DEC-11.

- Kanıt olgunluğu, requirement status ve hardware state birbirinden ayrılmıştır. SOURCE\_SUPPORTED veya CALCULATED bir çıktı fiziksel davranışı BENCH\_VERIFIED yapmaz.

- Simülasyon yalnız model kaynağı, korelasyonu, kapsadığı davranış ve çalışma zarfı açıkça nitelendirildiğinde tasarım kararı için kullanılabilir.

- Kod, EDA, simülasyon ve fiziksel test kullanımı “cheapest-sufficient-tool” zinciriyle yönetilir. Bir gap kabul kriterini sağladığında araştırma ve araç kullanımı durur.

## 2. 16.09 Sürümünden 17.09 Reference Baseline’a Geçiş

| **Alan** | **Önceki yaklaşım** | **Reference Baseline yaklaşımı** |
| --- | --- | --- |
| Starting point | Phase 0 / workspace creation | Mevcut workspace + mevcut PROJECT-01 PR; yeniden yaratma yok |
| Research | Unresolved-gap doğrulaması | Repository/reference/evidence ledger kontrolü önce; yalnız isimlendirilmiş boşluk için yeni kaynak |
| Simulation | Çıktı varsa destekleyici kanıt | Model provenance + qualification + scope zorunlu; kapsam dışı davranışta MODEL\_OUT\_OF\_SCOPE |
| Tooling | Reproducible araç kullanımı | T0–T6 escalation; heavy-tool trigger ve stop koşulları |
| Validation | Req → method → expected → actual → evidence | Evidence maturity + exact tool + model status + limitations alanları eklendi |
| Hardware truth | DESIGNED/SIMULATED/... | Aynı vocabulary korunur; claim/evidence maturity’den kesin ayrılır |
| PROJECT-01 | Broad topic closure | GATE-2 için dört deterministik closure contract |
| PR discipline | Genel repo ilerlemesi | Gate kapanış PR’ına gelecek iş karıştırma; MERGE\_READY ayrı karar |

## 3. Codex Yürütme Sözleşmesi

| CURRENT HANDOFF + ACTIVE GAP FILES<br>↓<br>EXISTING EVIDENCE REUSE<br>↓<br>CHEAPEST SUFFICIENT TOOL<br>↓<br>ACCEPTANCE CRITERION<br>↓<br>EVIDENCE PATH UPDATE<br>↓<br>GAP CLOSE / PHYSICAL\_VALIDATION\_REQUIRED<br>↓<br>STOP |
| --- |

- Handoff yürütme sözleşmesidir; Research Records geniş bilgi havuzu, Reference Lists aday kanıt seti, Repository ise gerçek artifact ve log kaynağıdır.

- Repository kanıtı anlatısal iddiadan üstündür. Handoff, repository gerçeğini sessizce geçersiz kılamaz.

- Her yeni ağır araç çalıştırması hangi unresolved failure mode veya decision riskini azaltacağını önceden isimlendirmelidir.

- Kabul kriteri karşılandığında aynı gap üzerinde “daha iyi kaynak” araması yapılmaz; kanıt kaydedilir ve çalışma durur.

## 4. Curated Reference Product Baseline

| **Proje** | **Primary reference** | **Complementary reference** | **Codex kullanım sınırı** |
| --- | --- | --- | --- |
| PROJECT-01 | Adafruit Universal USB/DC/Solar Charger — BQ24074 | DFRobot FireBeetle 2 ESP32-E | Power-path/solar/USB/Li-ion benchmark + kompakt ESP32/batarya/USB entegrasyonu; true MPPT iddiası yok; MCP73871 kararı otomatik değişmez. |
| PROJECT-02 | Kvaser USBcan Pro 2xHS v2 CB | PEAK PCAN-USB Pro FD | Dual-CAN-FD paketleme, izolasyon, connector/layout, termination ve host davranışı benchmarkı. |
| PROJECT-03 | Isabellenhütte IVT-S | Orion BMS 2 | V/I/T-to-CAN telemetry, calibration/DBC, fault/status ve diagnostics benchmarkı; full BMS scope’a genişleme yok. |
| PROJECT-04 | Sungrow PowerStack 255CS | Huawei LUNA2000-215 | C&I BESS equipment boundary, PCS/BMS/EMS interface, operating mode, protection, equipment schedule ve SLD benchmarkı. |

| **Benchmark kuralı:** Primary ürün varsayılan benchmarktır; complementary ürün yalnız kayıtlı boşluğu tamamlamak için kullanılır. Benchmark hiçbir existing repository decision veya active gate’i sessizce değiştirmez. |
| --- |

## 5. Proje Kuyruğu ve Aktif Kapı

| **Proje** | **Near-term artifact** | **Gate** | **Execution state** |
| --- | --- | --- | --- |
| PROJECT-01 — Solar low-power sensor | I2C contract + power-state model + GATE-2 evidence closure | GATE-2 | ACTIVE — explicit evidence/decision gaps ile BLOCKED |
| PROJECT-02 — USB CAN/CAN FD | CAN specification matrix + interface contract | GATE-3 | QUEUED |
| PROJECT-03 — CAN battery monitor | Measurement-to-telemetry contract | GATE-4 | QUEUED |
| PROJECT-04 — PV+BESS+backup generation | Operating-scenario matrix + reproducible sizing model | GATE-5 | QUEUED |

| **Aktif öncelik:** PROJECT-01 PR #1 GATE-2 kapanmadan daha sonraki implementation work’e scope genişletilmez. Dört closure contract kapandıktan sonra GATE-2 bir kez yeniden değerlendirilir. |
| --- |

## 6. Requirement, Hardware ve Evidence Maturity Ayrımı

- Requirement status: OPEN / IN\_PROGRESS / IMPLEMENTED / VERIFIED / BLOCKED / NOT\_APPLICABLE. VERIFIED için evidence path zorunlu.

- Hardware state: DESIGNED / SIMULATED / FABRICATED / ASSEMBLED / BENCH\_TESTED / FIELD\_TESTED. DRC pass yalnız DESIGNED düzeyini destekler.

- Evidence maturity bağımsızdır; tasarımın ne kadar olgun olduğunu değil, belirli claim’in hangi tür kanıtla desteklendiğini belirtir.

| **Evidence maturity** | **Anlam** | **İzin verilen claim** |
| --- | --- | --- |
| ASSUMED | Yeterli dış/measured proof olmadan çalışma değeri/davranışı | Yalnız explicit verification plan ile |
| SOURCE\_SUPPORTED | Primary/credible source davranış/değeri destekler | Tasarımı sınırlar; bu tasarımın fiziksel kanıtı değildir |
| CALCULATED | Kayıtlı inputlardan reproducible analytical result | Belirtilen assumptions içinde predicted behavior |
| DESIGN\_VERIFIED | Schematic/layout/config requirement ve source constraint’e karşı kontrol edilmiş | Implementation consistency; fiziksel performans değil |
| BENCH\_VERIFIED | Actual hardware measurement, defined setup/condition | Ölçülen operating condition |
| FIELD\_VERIFIED | Defined real-world operating condition | Test edilmiş field envelope |

## 7. Simulation Model Qualification

| **Temel kural:** “SPICE çalıştı” bir requirement’ı kapatmaz. Simülasyon kanıtı, yalnız belirli behavior için model qualification kayıtlıysa kullanılabilir. |
| --- |

| **Model state** | **Entry condition** | **Codex kullanım sınırı** |
| --- | --- | --- |
| UNVERIFIED\_MODEL | Model dosyası var; origin/coverage kanıtlı değil | Exploratory only; PASS/VERIFIED yok |
| SOURCE\_VERIFIED | Origin ve exact component/revision traceable | Qualification testleri yapılabilir; henüz unsupported behavior claim’i yok |
| DATASHEET\_CORRELATED | Relevant datasheet point/curve declared criterion içinde yeniden üretiliyor | Correlated envelope içinde kullanılabilir |
| BENCH\_CORRELATED | Rev-A ölçümü ile model karşılaştırılmış | Correlated behavior için daha yüksek güven |
| QUALIFIED\_FOR\_\[SCOPE\] | Source + correlation + envelope + exclusions kayıtlı | Yalnız isimlendirilmiş analysis scope için |

- Model kaydı: MPN/manufacturer, model source/version/date, datasheet revision, model type, supported/unsupported ranges, testbench, acceptance criterion, correlation outputs, qualification scope ve exclusions.

- Startup, shutdown, reverse leakage, thermal shutdown, OVP logic, protection disconnect veya temperature behavior modelde kanıtlı değilse MODEL\_OUT\_OF\_SCOPE kaydı açılır; simulation requirement closure için kullanılmaz.

## 8. Cheapest-Sufficient-Tool Politikası

| **Level** | **Tool/method** | **Use when** | **Escalate when** |
| --- | --- | --- | --- |
| T0 | Primary-source read + exact condition extraction | Semantic/specification sorusu | Operating condition veya dependency çözülemiyor |
| T1 | Analytical calculation / Python | RC, budget, tolerance, scaling, energy balance, sizing | Parametre sayısı / nonlinear/transient sensitivity artıyor |
| T2 | Python sweep / corner / Monte Carlo | Tolerance ve operating envelope robustness | Dynamic/nonlinear device behavior baskın |
| T3 | KiCad ERC/DRC / build / static / unit tests | Implementation consistency mechanically checkable | Analog/transient/physical behavior belirleyici |
| T4 | Qualified-model SPICE / EDA | Dynamic/nonlinear behavior decision-critical | Model unqualified veya physical parasitics baskın |
| T5 | Physical bench validation | Leakage, timing, startup, thermal, RF/EMI, component spread | Field environment kalan uncertainty |
| T6 | Field validation | Real mission profile / operating environment | Son seviye; tested envelope raporlanır |

| **Stop rule:** Acceptance criterion gerekli evidence maturity ile sağlandığında gap kapatılır, evidence path kaydedilir ve o gap üzerinde çalışma durur. |
| --- |

## 9. PROJECT-01 — GATE-2 Kapanış İş Akışı

| GAP-08 → GAP-03 → GAP-02 → DEC-11<br>↓<br>GATE-2 RE-EVALUATION (once)<br>↓<br>AFFECTED FINAL CHECKS<br>↓<br>PASS ? FINAL REVIEW → MERGE\_READY<br>BLOCKED ? EXACT REMAINING BLOCKER<br>↓<br>STOP |
| --- |

| **Sıra:** Cheap-first default sıra GAP-08 → GAP-03 → GAP-02 → DEC-11’dir. Yalnız kayıtlı dependency bu sırayı değiştirebilir. |
| --- |

### 9.1 GAP-08 — No-input / shutdown current semantics

**Current issue:** Kaydedilmiş current değerlerinin aynı system state / measurement condition’a ait olduğu henüz kanıtlı değil.

**Cheapest sufficient action:** Datasheet’te exact parameter name, condition ve current path eşleştirilir. Yalnız like-for-like state’ler aynı budget’a alınır; gerekirse reproducible current budget script’i çalıştırılır.

**Closure criterion:** Budget’a giren her current term primary-source condition taşır ve seçilen system state kendi içinde tutarlıdır.

**Stop/output:** Power-state/current budget güncellenir; semantic conflict kapanınca araştırma durur.

### 9.2 GAP-03 — MCP73871 battery-side protection

**Current issue:** Protection ifadeleri, tasarım varsayımındaki exact independent battery disconnect semantics’i henüz kanıtlamıyor.

**Cheapest sufficient action:** Yalnız official MCP73871 documentation kullanılır; exact mode/condition çıkarılır. Generic charger model ile protection behavior çıkarımı yapılmaz.

**Closure criterion:** Primary source gereken davranışı açıkça kanıtlar veya assumption reddedilir ve external protection decision açılır.

**Stop/output:** Decision register ve requirement status güncellenir; semantics unambiguous olduğunda durulur.

### 9.3 GAP-02 — I2C electrical closure

**Current issue:** Pull-up hesabı Cbus provenance / board model eksikliği nedeniyle final değil.

**Cheapest sufficient action:** MCU/sensor/input capacitance + trace/connector estimate ile documented Cbus budget oluşturulur; Rpull-up/rise-time worst-case bound reproducible hesaplanır. SPICE yalnız non-ideal dynamic gerekirse.

**Closure criterion:** Source-supported inputlar + worst-case calculation compliant margin gösterir; actual rise-time Rev-A physical queue’ya aktarılır.

**Stop/output:** I2C contract/calculation evidence güncellenir; analytical gap kapanır; scope actual rise-time bench validation olarak ayrılır.

### 9.4 DEC-11 — Input OVP selection

**Current issue:** Functional need biliniyor; threshold/disconnect/Iq/leakage/hot-plug kanıtı component selection için yeterli değil.

**Cheapest sufficient action:** Yalnız primary-source candidates explicit criteria’ya karşı karşılaştırılır. İlk yöntem calculation’dır. Dynamic response decision-critical ve qualified manufacturer model varsa transient simulation düşünülebilir.

**Closure criterion:** Seçenek threshold, true disconnect, current/leakage budget, abs-max/hot-plug requirement’larını karşılar veya exact missing evidence ile BLOCKED kalır.

**Stop/output:** Decision register + component selection güncellenir; speculative seçim yapılmaz.

## 10. PROJECT-01 Fiziksel Validasyon Kuyruğu

- Assembled PCB üzerinde I2C SDA/SCL rise time ve logic levels.

- Defined firmware state altında deep-sleep / shutdown / battery-only current.

- Power-rail startup, shutdown ve source-transition behavior.

- Battery ve solar/USB koşullarında charger/power-path behavior.

- Final architecture gerektiriyorsa input OVP/fault-disconnect behavior.

- Low-power state transition sırasında sensor communication.

- Variable-input / weak-solar behavior ve brownout/recovery.

- Dissipation veya charge-current koşulları gerektiriyorsa thermal behavior.

| **Sınır:** Bu maddeler actual Rev-A measurement olmadan BENCH\_VERIFIED seviyesine yükseltilemez. Codex procedure hazırlayabilir; measurement üretemez. |
| --- |

## 11. PROJECT-02 / 03 / 04 Kuyruk Sözleşmeleri

| **Project** | **Pre-implementation contract** | **Likely tool path** | **Physical/field boundary** |
| --- | --- | --- | --- |
| PROJECT-02 | CAN/FDCAN/USB/power/interface specification matrix + pin/electrical compatibility | Primary docs → scripted checks → KiCad ERC/DRC → firmware build/tests → qualified simulation only for specific transients | CAN physical-layer captures, USB/CAN timing, protection behavior bench evidence ister |
| PROJECT-03 | Measurement chain, calibration, filtering, plausibility + CAN telemetry contract | Source extraction → error budget/Python → firmware tests → ADC/AFE simulation if justified | Calibration, sensor error/noise, current/voltage measurement bench correlation ister |
| PROJECT-04 | Operating scenarios, load model, assumptions, sizing constraints, protection boundaries | Reproducible Python/energy model → scenario/corner analysis → electrical design checks | Site-specific/permit/utility/field claims actual project data olmadan evidence değildir |

## 12. Araştırma Politikası — Delta Only

- Yeni product search öncesi §4 reference baseline kontrol edilir. Yeni benchmark ancak primary/complementary pair isimlendirilmiş requirement/decision gap’i cevaplayamıyorsa eklenir.

- Web/GitHub araştırmasından önce repository, reference list ve evidence ledger’da mevcut kanıt aranır.

- Primary sıralama: manufacturer datasheet → reference manual → application note → protocol specification → official project documentation.

- Open-source implementation, electrical specification yerine geçmez; implementation pattern ve compatibility clue olarak kullanılır.

- Conflict authoritative sources arasında tasarımı etkiliyorsa CONFLICT raporlanır. Remaining uncertainty board/assembly-dependent ise PHYSICAL\_VALIDATION\_REQUIRED ile durulur.

- Narrowed datasheet condition için broad repository discovery yeniden başlatılmaz.

## 13. Limit-Bilinçli Codex Uygulaması

| **Kaçınılacak** | **Yerine kullanılacak** |
| --- | --- |
| Entire research archive reread | Requirement/gap-linked evidence lookup |
| General web/GitHub exploration | Primary-source unresolved-gap verification |
| All simulations “for confidence” | Failure-mode-specific qualified-model simulation |
| Repeated full builds after doc-only edits | Affected-check selection |
| Placeholder files as progress | Only active closure-contract artifacts |
| PASS from narrative | Machine-readable evidence path + acceptance criterion |

- Yalnız current handoff ve active gap tarafından adı verilen dosyalar okunur.

- Mevcut calculation/source/script/passing output change tarafından invalidated değilse yeniden kullanılır.

- Bir closure contract tek seferde işlenir ve evidence hemen repository’ye yazılır.

- Birçok interactive micro-step yerine consolidated scripted run tercih edilir.

- Gate acceptance sonrası record → close → stop.

## 14. Reproducibility ve Physical Validation Handoff Loop

| DESIGN / REQUIREMENT<br>↓<br>ANALYTICAL CHECK<br>↓<br>OPTIONAL QUALIFIED SIMULATION<br>↓<br>REV-A FABRICATION / ASSEMBLY<br>↓<br>CODEX LAB PROCEDURE<br>↓<br>OPERATOR MEASUREMENT<br>↓<br>EVIDENCE IMPORT<br>↓<br>EXPECTED vs ACTUAL<br>↓<br>ISSUE / DECISION UPDATE → REV-B if needed |
| --- |

- Inputs machine-readable veya explicit recorded olmalı; numeric constants SOURCE\_SUPPORTED / DECISION\_SUPPORTED / ASSUMED provenance taşımalı.

- Script output, tool/version (uygunsa), input set, command, result ve acceptance criterion üretebilmeli.

- Tek başına plot yeterli kanıt değildir; underlying inputs/results gerekir.

- Manufacturer model sessizce düzenlenmez; wrapper veya parameter override kaydedilir.

- Physical-test record en az hardware revision/serial, firmware commit/config, equipment/settings, wiring/setup, environment/state, expected criterion, actual measurement, raw evidence path ve follow-up içermeli.

## 15. GitHub / PR Kuralları

- Evidence/gate closure PR’ına ilgisiz future implementation karıştırılmaz.

- Her PR affected requirements / decisions / gates’i açıkça yazar.

- Clean diff tek başına merge nedeni değildir; gate status ve evidence MERGE\_READY durumunu desteklemelidir.

- Gate-closing PR merge sonrası sonraki implementation yeni branch/PR’da yürütülür; mevcut PR scope açıkça kapsamadıkça aynı PR’a devam edilmez.

- Yalnız “continue” niteliğinde ve unresolved blocker’ları tekrar eden commit’ler üretilmez.

- GATE-2 dört closure contract sonrası bir kez re-evaluate edilir; PASS ise final review → MERGE\_READY; merge yalnız operator/workflow authorization ile.

## 16. Revize İlk Codex Aksiyonu

1. Phase 0’ı yeniden oluşturma ve broad research başlatma.

2. PROJECT-01 requirement inventory, decision register, PR #1 diff/context, GATE-2 status ve GAP-02/GAP-03/GAP-08/DEC-11 evidence dosyalarını yükle.

3. Yeni source istemeden önce mevcut source/calculation’ları tekrar kullan.

4. Default sıra ile GAP-08 → GAP-03 → GAP-02 → DEC-11 closure contract’larını çöz; yalnız documented dependency varsa sırayı değiştir.

5. Her gap için cheapest-sufficient-tool kullan ve evidence’i hemen repository’ye yaz.

6. Unqualified simulation model ile requirement kapatma.

7. Remaining uncertainty actual hardware gerektiriyorsa PHYSICAL\_VALIDATION\_REQUIRED işaretle ve bench procedure hazırla.

8. Dört closure contract sonrası GATE-2’yi bir kez re-evaluate et; affected final checks’i çalıştır; MERGE\_READY veya exact remaining blocker raporla.

9. STOP. Ayrı talimat olmadan sonraki implementation PR’a geçme.

## 17. Codex Completion Report Format

| ACTIVE\_GATE: GATE-2<br>GAPS\_CLOSED: \[IDs\]<br>GAPS\_OPEN: \[IDs + exact missing evidence\]<br>TOOLS\_RUN: \[tool / command / purpose\]<br>MODELS\_USED: \[status + qualification scope\]<br>FILES\_CHANGED: \[repo-relative paths\]<br>EVIDENCE\_ADDED: \[repo-relative paths\]<br>PHYSICAL\_VALIDATION\_QUEUE: \[tests\]<br>GATE\_RESULT: PASS \| BLOCKED \| NOT\_REEVALUATED<br>NEXT\_ACTION: one smallest sufficient action only |
| --- |

## 18. Portfolio-Ready / Definition of Done

- Requirements traceable.

- Architecture/interfaces documented.

- Calculations/build steps reproducible.

- Decision and risk registers current design state’i yansıtıyor.

- Validation status explicit.

- Every VERIFIED claim has an evidence path.

- Simulation claims qualified model scope’u gösteriyor.

- Physical claims yalnız actual BENCH\_VERIFIED/FIELD\_VERIFIED evidence ile sınırlı.

- Limitations ve unresolved assumptions açık.

- README bağımsız olarak anlaşılabilir.

| **Sonuç:** Bu şartlardan biri eksikse proje OPEN, IN\_PROGRESS veya BLOCKED kalır. Release bir portfolio snapshot’tır; production readiness kanıtı değildir. |
| --- |

## 19. Asgari Risk Politikası

- Electrical and battery safety.

- CAN termination / ground / reference errors.

- Component availability and model availability.

- PCB manufacturability and unmodeled parasitics.

- Unvalidated high-current / transient behavior.

- ADC scaling/calibration and measurement plausibility.

- Thermal behavior.

- Incorrect energy/load assumptions and BESS/PV sizing assumptions.

- Regulatory/permit limitations.

- Unsupported performance claims.

- Simulation-model misuse outside qualified scope.
