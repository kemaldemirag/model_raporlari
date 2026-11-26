**CAN BUS + ENERGY GENERATION PORTFOLIO**

# POZİSYON BAZLI<br>CODEX GÖREV AKIŞI RAPORU

**REFERENCE BASELINE — 17.09.2026**

*17.09.2026 tarihli İş Akışı Raporu / Reference Baseline temel alınarak hazırlanmıştır*

| **Alan** | **Değer** |
| --- | --- |
| Handoff ID | CAN-ENERGY-PORTFOLIO-2026-09-15 |
| Operating principle | Gate-first, evidence-driven, limit-conscious execution |
| Aktif yürütme odağı | PROJECT-01 / PR #1 / GATE-2 |
| Aktif kapanış sözleşmeleri | GAP-08 → GAP-03 → GAP-02 → DEC-11 |
| Kuyruk | PROJECT-02 → GATE-3; PROJECT-03 → GATE-4; PROJECT-04 → GATE-5 |
| Rapor amacı | Dört pozisyon için Codex görev akışını, promptları, stop koşullarını ve fiziksel validasyon sınırlarını güncellemek |

| **Temel değişiklik:** Başlangıç artık Phase 0 değildir. Mevcut workspace ve repository gerçeği korunur; Codex yalnız aktif kanıt boşluğunu en düşük yeterli araçla kapatır ve acceptance criterion sağlanınca durur. |
| --- |

## 1. 17.09 Reference Baseline’ın Görev Planına Etkisi

Bu sürüm, 16.09 planındaki “önce governance/requirements, sonra proje akışları” yaklaşımını daha dar ve operasyonel bir yürütme sözleşmesine dönüştürür. Dört pozisyon korunur; ancak yalnız PROJECT-01 yürütülebilir aktif iş durumundadır. PROJECT-02, PROJECT-03 ve PROJECT-04 kuyruktadır.

### 1.1 Değişen yürütme ilkeleri

- Phase 0 veya workspace skeleton yeniden oluşturulmaz; mevcut repository ve PR bağlamı kaynak gerçek kabul edilir.

- Araştırma sırası repository/reference/evidence ledger kontrolüyle başlar; yeni kaynak yalnız isimlendirilmiş unresolved gap için açılır.

- Bir gap acceptance criterion’u gereken evidence maturity düzeyinde karşıladığında evidence path güncellenir ve o gap üzerinde çalışma durur.

- Simülasyon, model provenance + qualification + scope + exclusions kayıtlı değilse requirement closure için kullanılmaz.

- Kod/EDA/simülasyon/fiziksel test, T0–T6 cheapest-sufficient-tool zinciriyle seçilir.

- Requirement status, hardware state ve evidence maturity ayrı eksenlerdir; biri diğerinin yerine kullanılamaz.

- PROJECT-01 GATE-2 dört closure contract tamamlandıktan sonra yalnız bir kez yeniden değerlendirilir.

### 1.2 Proje kuyruğu

| **Pozisyon / Proje** | **Near-term artifact** | **Gate** | **Execution state** |
| --- | --- | --- | --- |
| Pozisyon 1 — PROJECT-01 | I2C contract + power-state model + GATE-2 evidence closure | GATE-2 | ACTIVE / BLOCKED by explicit evidence-decision gaps |
| Pozisyon 2 — PROJECT-02 | CAN specification matrix + interface contract | GATE-3 | QUEUED |
| Pozisyon 3 — PROJECT-03 | Measurement-to-telemetry contract | GATE-4 | QUEUED |
| Pozisyon 4 — PROJECT-04 | Operating-scenario matrix + reproducible sizing model | GATE-5 | QUEUED |

| **Aktif öncelik:** PROJECT-01 / PR #1 / GATE-2 kapanmadan sonraki implementation work’e scope genişletilmez. P02–P04 promptları hazır bekler; otomatik yürütülmez. |
| --- |

### 1.3 Curated reference ürün baseline’ı

| **Pozisyon** | **Primary reference** | **Complementary reference** | **Codex kullanım sınırı** |
| --- | --- | --- | --- |
| P01 | Adafruit Universal USB/DC/Solar Charger — BQ24074 | DFRobot FireBeetle 2 ESP32-E | Power-path/solar/USB/Li-ion benchmarkı; true MPPT iddiası yok; MCP73871 kararını otomatik değiştirmez. |
| P02 | Kvaser USBcan Pro 2xHS v2 CB | PEAK PCAN-USB Pro FD | Dual-CAN-FD paketleme, izolasyon, connector/layout, termination ve host davranışı benchmarkı. |
| P03 | Isabellenhütte IVT-S | Orion BMS 2 | V/I/T-to-CAN telemetry, calibration/DBC, fault/status ve diagnostics benchmarkı; full BMS scope’a genişleme yok. |
| P04 | Sungrow PowerStack 255CS | Huawei LUNA2000-215 | C&I BESS equipment boundary, PCS/BMS/EMS interface, operating mode, protection, equipment schedule ve SLD benchmarkı. |

| **Benchmark kuralı:** Primary ürün varsayılan benchmarktır; complementary ürün yalnız kayıtlı boşluğu tamamlamak için kullanılır. Benchmark, existing repository decision veya active gate’i sessizce değiştiremez. |
| --- |

## 2. Ortak Codex Yürütme Sözleşmesi

CURRENT HANDOFF + ACTIVE GAP FILES

↓

EXISTING EVIDENCE REUSE

↓

CHEAPEST SUFFICIENT TOOL

↓

ACCEPTANCE CRITERION

↓

EVIDENCE PATH UPDATE

↓

GAP CLOSE / PHYSICAL\_VALIDATION\_REQUIRED

↓

STOP

### 2.1 Ortak wrapper prompt

ROLE: Evidence-driven portfolio execution agent

OPERATING MODE: Gate-first / delta-only / cheapest-sufficient-tool

Before changing anything:

1. Read only the current handoff, active gap/decision files and directly affected repository evidence.

2. Reuse existing source/calculation/script/passing output unless the current change invalidates it.

3. Name the exact unresolved failure mode or decision risk before running any new heavy tool.

4. Select the lowest tool level that can satisfy the acceptance criterion.

5. Write evidence to the repository immediately after each closure contract.

6. Stop work on that gap when the acceptance criterion is satisfied.

Never:

- recreate Phase 0 or the workspace,

- restart broad web/GitHub discovery for a narrowed datasheet condition,

- use an unqualified simulation model to close a requirement,

- claim BENCH\_VERIFIED/FIELD\_VERIFIED without actual operator measurement,

- mix unrelated future implementation into a gate-closure PR,

- infer PASS/MERGE\_READY from a clean diff alone.

If remaining uncertainty is board/assembly dependent:

mark PHYSICAL\_VALIDATION\_REQUIRED, prepare the lab procedure, and STOP.

### 2.2 Cheapest-sufficient-tool escalation

| **Level** | **Tool / method** | **Kullanım amacı** | **Yükseltme tetikleyicisi** |
| --- | --- | --- | --- |
| T0 | Primary-source read + exact condition extraction | Semantic/specification sorusu | Operating condition veya dependency çözülemiyor |
| T1 | Analytical calculation / Python | RC, budget, tolerance, scaling, energy balance, sizing | Parametre / nonlinear / transient sensitivity artıyor |
| T2 | Python sweep / corner / Monte Carlo | Tolerance ve operating envelope robustness | Dynamic/nonlinear device behavior baskın |
| T3 | KiCad ERC/DRC / build / static / unit tests | Implementation consistency | Analog/transient/physical behavior belirleyici |
| T4 | Qualified-model SPICE / EDA | Decision-critical dynamic/nonlinear behavior | Model unqualified veya physical parasitics baskın |
| T5 | Physical bench validation | Leakage, timing, startup, thermal, RF/EMI, component spread | Field environment uncertainty kalıyor |
| T6 | Field validation | Real mission profile / operating environment | Son seviye; tested envelope raporlanır |

| **Stop rule:** Acceptance criterion gereken evidence maturity ile sağlandığında gap kapanır, evidence path kaydedilir ve aynı gap üzerinde ek araştırma/araç çalıştırılmaz. |
| --- |

### 2.3 Evidence ve model state sınırları

| **Eksen** | **İzin verilen durumlar** | **Kritik yorum** |
| --- | --- | --- |
| Requirement status | OPEN / IN\_PROGRESS / IMPLEMENTED / VERIFIED / BLOCKED / NOT\_APPLICABLE | VERIFIED için evidence path zorunlu. |
| Hardware state | DESIGNED / SIMULATED / FABRICATED / ASSEMBLED / BENCH\_TESTED / FIELD\_TESTED | DRC pass yalnız DESIGNED düzeyini destekler. |
| Evidence maturity | ASSUMED / SOURCE\_SUPPORTED / CALCULATED / DESIGN\_VERIFIED / BENCH\_VERIFIED / FIELD\_VERIFIED | SOURCE\_SUPPORTED/CALCULATED fiziksel davranışı BENCH\_VERIFIED yapmaz. |
| Model state | UNVERIFIED\_MODEL / SOURCE\_VERIFIED / DATASHEET\_CORRELATED / BENCH\_CORRELATED / QUALIFIED\_FOR\_\[SCOPE\] | Model yalnız kayıtlı scope içinde decision evidence olabilir. |

## 3. Pozisyon 1 — PROJECT-01: Solar Low-Power Sensor

| **Execution state:** ACTIVE. PROJECT-01 / PR #1 / GATE-2 tek aktif yürütme odağıdır. Cheap-first default sıra GAP-08 → GAP-03 → GAP-02 → DEC-11’dir; yalnız kayıtlı dependency sıralamayı değiştirebilir. |
| --- |

### 3.1 Güncel görev akışı

P01-G2-00  Current repository + PR #1 + GATE-2 evidence load

↓

P01-G2-01  GAP-08 — No-input / shutdown current semantics

↓

P01-G2-02  GAP-03 — MCP73871 battery-side protection

↓

P01-G2-03  GAP-02 — I2C electrical closure

↓

P01-G2-04  DEC-11 — Input OVP selection

↓

P01-G2-05  GATE-2 RE-EVALUATION — ONCE

↓

P01-G2-06  Affected final checks

↓

PASS → FINAL REVIEW → MERGE\_READY

BLOCKED → exact remaining blocker

↓

STOP

### 3.2 Atomik Codex görev tablosu

| **ID** | **Amaç** | **En düşük araç** | **Closure / stop output** |
| --- | --- | --- | --- |
| P01-G2-00 | Requirement inventory, decision register, PR #1 diff/context, GATE-2 ve dört gap evidence dosyasını yükle; yeni source açmadan mevcut kanıtı eşle. | T0 / repo read | Active evidence map; gereksiz reread yok. |
| P01-G2-01 | Like-for-like system state/current terms ile no-input/shutdown current semantics’i kapat. | T0 → gerekirse T1 | Power-state/current budget update; semantic conflict kapanır. |
| P01-G2-02 | MCP73871 exact battery-side protection semantics’ini official docs ile doğrula veya assumption’ı reddet. | T0 | Decision register + requirement status; unambiguous olunca stop. |
| P01-G2-03 | Documented Cbus budget + worst-case Rpull-up/rise-time bound üret. | T0 → T1; T4 yalnız gerekirse | I2C contract/calculation evidence; actual rise-time T5 kuyruğuna ayrılır. |
| P01-G2-04 | Input OVP candidate’larını threshold, true disconnect, Iq/leakage, abs-max/hot-plug kriterlerine göre karşılaştır. | T0 → T1; T4 yalnız qualified model varsa | Decision register + component selection veya exact missing evidence ile BLOCKED. |
| P01-G2-05 | Dört closure contract sonrası GATE-2’yi yalnız bir kez yeniden değerlendir. | Repo evidence / affected checks | PASS veya exact remaining blocker. |
| P01-G2-06 | Yalnız change’den etkilenen final checks’i çalıştır. | T3 ve gerekiyorsa daha alt seviye | Final review için machine-readable evidence. |
| P01-G2-07 | PASS ise MERGE\_READY; değilse exact blocker raporu. Merge işlemi ayrı operator/workflow authorization ister. | No new discovery | Completion report + STOP. |

### 3.3 P01-G2-01 — GAP-08 promptu

TASK: P01-G2-01 — Close GAP-08: no-input / shutdown current semantics

Use the current PROJECT-01 repository evidence only unless an exact source condition is missing.

Current issue:

Recorded current values are not yet proven to describe the same system state / measurement condition.

Action:

- Extract the exact parameter name, condition and current path from the relevant primary datasheet(s).

- Include only like-for-like states in the same budget.

- Reuse existing current-budget evidence if still valid.

- Run a reproducible analytical/Python current budget only if needed to resolve the semantic mismatch.

Closure criterion:

Every current term entering the budget has a primary-source condition and the selected system state is internally consistent.

Output:

- update power-state/current-budget evidence,

- record provenance for every term,

- close GAP-08 when the criterion is satisfied,

- otherwise report the exact missing condition.

STOP after closure. Do not continue to unrelated research.

### 3.4 P01-G2-02 — GAP-03 promptu

TASK: P01-G2-02 — Close GAP-03: MCP73871 battery-side protection

Use only official MCP73871 documentation for the semantic decision.

Do not infer protection behavior from a generic charger model or unrelated implementation.

Determine whether the documented modes/conditions prove the exact independent battery-disconnect behavior required by the current design assumption.

Closure criterion:

Either:

A) primary-source documentation explicitly supports the required behavior, or

B) the assumption is rejected and an external-protection decision is opened.

Update:

- decision register,

- requirement status,

- evidence path.

STOP when the semantics are unambiguous.

### 3.5 P01-G2-03 — GAP-02 promptu

TASK: P01-G2-03 — Close GAP-02: I2C electrical closure

Build a documented Cbus budget using source-supported MCU/sensor input capacitance and recorded trace/connector estimate(s).

Compute a reproducible worst-case pull-up / rise-time bound.

Tool order:

T0 source extraction → T1 calculation.

Use SPICE only if a non-ideal dynamic behavior is decision-critical and the model is qualified for that scope.

Closure criterion:

Source-supported inputs plus worst-case calculation demonstrate compliant analytical margin.

Output:

- update I2C contract/calculation evidence,

- mark actual Rev-A rise-time measurement as PHYSICAL\_VALIDATION\_REQUIRED,

- keep the analytical gap separate from physical bench validation.

STOP after the analytical closure is recorded.

### 3.6 P01-G2-04 — DEC-11 promptu

TASK: P01-G2-04 — Resolve DEC-11: input OVP selection

Compare only primary-source component candidates against the explicit current criteria:

- threshold behavior,

- true disconnect behavior,

- current / leakage budget,

- abs-max requirement,

- hot-plug requirement.

Start with T0/T1 source extraction and calculation.

Use transient simulation only if dynamic response is decision-critical and an exact qualified manufacturer model is available.

Closure criterion:

Select a candidate only if it satisfies the recorded criteria with traceable evidence.

Otherwise keep DEC-11 BLOCKED and record the exact missing evidence.

Update the decision register and component-selection evidence.

Do not make a speculative selection.

### 3.7 GATE-2 yeniden değerlendirme promptu

TASK: P01-G2-05..07 — Re-evaluate GATE-2 once

Precondition:

GAP-08, GAP-03, GAP-02 and DEC-11 have each reached a documented closure state.

1. Re-evaluate GATE-2 exactly once against current repository evidence.

2. Run only checks affected by the closure changes.

3. If all acceptance criteria pass, report FINAL REVIEW → MERGE\_READY.

4. If blocked, report the exact remaining blocker and the smallest sufficient next action.

5. Do not start the next implementation PR.

6. Do not merge automatically; merge requires separate operator/workflow authorization.

7. STOP.

Use the standard completion report format.

### 3.8 Fiziksel validasyon kuyruğu — Codex sınırı

- Assembled PCB üzerinde I2C SDA/SCL rise time ve logic levels.

- Defined firmware state altında deep-sleep / shutdown / battery-only current.

- Power-rail startup, shutdown ve source-transition behavior.

- Battery ve solar/USB koşullarında charger/power-path behavior.

- Final architecture gerektiriyorsa input OVP/fault-disconnect behavior.

- Low-power state transition sırasında sensor communication.

- Variable-input / weak-solar behavior ve brownout/recovery.

- Gerekiyorsa thermal behavior.

| **Sınır:** Codex bu testler için procedure ve acceptance criterion hazırlayabilir; actual Rev-A operator measurement olmadan BENCH\_VERIFIED claim üretemez. |
| --- |

## 4. Pozisyon 2 — PROJECT-02: USB CAN / CAN FD Interface

| **Execution state:** QUEUED / GATE-3. PROJECT-01 GATE-2 kapanışı ve sonraki açık talimat olmadan implementation başlatılmaz. |
| --- |

### 4.1 Pre-implementation contract

Reference Baseline bu pozisyon için önce CAN/FDCAN/USB/power/interface specification matrix ve pin/electrical compatibility sözleşmesinin kurulmasını ister. Benchmark ürünler Kvaser USBcan Pro 2xHS v2 CB (primary) ve PEAK PCAN-USB Pro FD (complementary) olarak kullanılır; benchmark existing design decision’ı otomatik değiştirmez.

### 4.2 Codex görev akışı

| **ID** | **Görev** | **Tool path / stop** |
| --- | --- | --- |
| P02-Q-00 | Activation guard: current handoff + GATE-3 files + existing repo evidence’i yükle; P01 gate durumu ve explicit authorization olmadan implementation açma. | T0; scope guard |
| P02-01 | Existing evidence ve requirement-linked gap lookup. | Repository/reference/evidence ledger first |
| P02-02 | Curated benchmark baseline’ın yalnız kayıtlı gap’i cevaplayıp cevaplamadığını kontrol et. | Primary Kvaser; PEAK yalnız complementary gap closure |
| P02-03 | CAN/FDCAN/USB/power/interface specification matrix + interface contract oluştur/güncelle. | T0; source-supported conditions |
| P02-04 | Pin/electrical compatibility kontrollerini primary docs ile bağla. | T0 → gerekirse scripted checks |
| P02-05 | Reproducible scripted checks çalıştır; yalnız affected outputs üret. | T1/T3 |
| P02-06 | Implementation consistency gerekiyorsa KiCad ERC/DRC ve firmware build/tests. | T3 |
| P02-07 | Yalnız belirli decision-critical transient için qualified model simülasyonu. | T4 conditional |
| P02-08 | GATE-3 evidence re-evaluation; physical gaps’i bench queue’ya ayır. | PASS/BLOCKED + evidence paths |

### 4.3 Pozisyon 2 — Activation promptu

ROLE: PROJECT-02 USB CAN/CAN FD Interface Engineer

STATE: QUEUED / GATE-3

Do not execute implementation unless PROJECT-02 is explicitly activated after the current higher-priority gate work.

When activated:

1. Load only the current PROJECT-02 handoff/gate files and existing repository evidence.

2. Reuse existing evidence before requesting any new source.

3. Use Kvaser USBcan Pro 2xHS v2 CB as the primary product benchmark.

4. Use PEAK PCAN-USB Pro FD only if a named requirement/decision gap remains unanswered.

5. Build/update the CAN/FDCAN/USB/power/interface specification matrix and pin/electrical compatibility evidence.

6. Use primary documentation first; open-source implementations are compatibility clues, not electrical-specification proof.

7. Run scripted checks, KiCad ERC/DRC or firmware build/tests only when they close a named gap.

8. Use simulation only for a specific decision-critical transient with a model qualified for that exact scope.

9. Separate unresolved CAN physical-layer captures, USB/CAN timing and protection behavior into PHYSICAL\_VALIDATION\_REQUIRED bench work.

10. Re-evaluate GATE-3 using machine-readable evidence paths and STOP.

Return the standard completion report.

### 4.4 Fiziksel sınır

CAN physical-layer captures, USB/CAN timing ve protection behavior, gerçek bench evidence gerektirir. Codex bu alanlarda test planı/procedure hazırlayabilir; actual capture veya measurement üretmeden BENCH\_VERIFIED claim kullanamaz.

## 5. Pozisyon 3 — PROJECT-03: CAN Battery Monitoring Node

| **Execution state:** QUEUED / GATE-4. Scope full BMS tasarımına genişletilmez; iş measurement-to-telemetry contract çevresinde tutulur. |
| --- |

### 5.1 Pre-implementation contract ve benchmark

Pre-implementation contract: measurement chain, calibration, filtering, plausibility + CAN telemetry contract. Primary benchmark Isabellenhütte IVT-S; complementary benchmark Orion BMS 2’dir. Kullanım ekseni V/I/T-to-CAN telemetry, calibration/DBC, fault/status ve diagnostics ile sınırlıdır.

### 5.2 Codex görev akışı

| **ID** | **Görev** | **Tool path / stop** |
| --- | --- | --- |
| P03-Q-00 | Activation guard + mevcut requirement/evidence load. | T0; queued state korunur |
| P03-01 | Measurement chain ve telemetry semantics için mevcut evidence/gap lookup. | Repository/reference first |
| P03-02 | Primary/complementary benchmark yalnız isimlendirilmiş gap için kullan. | IVT-S primary; Orion BMS 2 complementary |
| P03-03 | Measurement chain, calibration, filtering, plausibility ve CAN telemetry contract oluştur/güncelle. | T0 |
| P03-04 | Source-supported error budget ve calibration assumptions çıkar. | T0 → T1/Python |
| P03-05 | Firmware-level mapping/plausibility tests gerekiyorsa çalıştır. | T3 / unit tests |
| P03-06 | ADC/AFE simülasyonu yalnız justified failure mode + qualified model olduğunda. | T4 conditional |
| P03-07 | GATE-4 re-evaluation; calibration/noise/measurement uncertainty physical queue’ya ayrılır. | PASS/BLOCKED + evidence paths |

### 5.3 Pozisyon 3 — Activation promptu

ROLE: PROJECT-03 CAN Battery Monitoring Engineer

STATE: QUEUED / GATE-4

Do not broaden scope into a full BMS.

When explicitly activated:

1. Load only current PROJECT-03 requirement, decision and evidence files.

2. Reuse repository/reference evidence before any new search.

3. Use Isabellenhütte IVT-S as the primary benchmark.

4. Use Orion BMS 2 only to fill a named telemetry/calibration/fault-status gap.

5. Build/update the measurement chain + calibration + filtering + plausibility + CAN telemetry contract.

6. Make numeric inputs provenance-explicit: SOURCE\_SUPPORTED / DECISION\_SUPPORTED / ASSUMED as applicable.

7. Build a reproducible analytical/Python error budget before considering ADC/AFE simulation.

8. Use firmware/unit tests only for implementation behavior that is mechanically testable.

9. Use ADC/AFE simulation only when a named failure mode requires dynamic behavior and the model is qualified for that scope.

10. Mark calibration, sensor error/noise and current/voltage measurement correlation as PHYSICAL\_VALIDATION\_REQUIRED when actual hardware is needed.

11. Re-evaluate GATE-4 and STOP.

Return the standard completion report.

### 5.4 Fiziksel sınır

Calibration, sensor error/noise ve current/voltage measurement bench correlation gerçek operator measurement ister. Codex yalnız test procedure, logging formatı, expected criterion ve evidence import yapısını hazırlayabilir.

## 6. Pozisyon 4 — PROJECT-04: PV + BESS + Backup Generation

| **Execution state:** QUEUED / GATE-5. Kodun ana işi site gerçekliğini uydurmak değil; explicit assumptions ile reproducible scenario/sizing model üretmektir. |
| --- |

### 6.1 Pre-implementation contract ve benchmark

Pre-implementation contract: operating scenarios, load model, assumptions, sizing constraints ve protection boundaries. Primary benchmark Sungrow PowerStack 255CS; complementary benchmark Huawei LUNA2000-215’tir. Kullanım sınırı C&I BESS equipment boundary, PCS/BMS/EMS interface, operating mode, protection, equipment schedule ve SLD benchmarkıdır.

### 6.2 Codex görev akışı

| **ID** | **Görev** | **Tool path / stop** |
| --- | --- | --- |
| P04-Q-00 | Activation guard + current requirement/scenario/evidence load. | T0; queued state korunur |
| P04-01 | Existing load/energy assumptions ve evidence provenance audit. | Repository/reference first |
| P04-02 | Primary/complementary benchmark yalnız isimlendirilmiş equipment/interface gap için kullan. | Sungrow primary; Huawei complementary |
| P04-03 | Operating-scenario matrix + load model + sizing constraints + protection boundaries oluştur/güncelle. | T0 / structured data |
| P04-04 | Reproducible Python/energy sizing model üret. | T1 |
| P04-05 | Scenario/corner/sensitivity analysis ile model envelope’ını değerlendir. | T2 |
| P04-06 | Gerekli electrical design checks’i çalıştır. | T3 as applicable |
| P04-07 | GATE-5 re-evaluation; site/permit/utility/field claims’i physical/project-data boundary’ye ayır. | PASS/BLOCKED + evidence paths |

### 6.3 Pozisyon 4 — Activation promptu

ROLE: PROJECT-04 PV+BESS+Backup Generation Modeling Engineer

STATE: QUEUED / GATE-5

When explicitly activated:

1. Load only the current PROJECT-04 requirement, scenario and evidence files.

2. Audit every numeric input for recorded provenance before using it in sizing.

3. Use Sungrow PowerStack 255CS as the primary product benchmark.

4. Use Huawei LUNA2000-215 only when a named equipment/interface gap remains unanswered.

5. Build/update the operating-scenario matrix, load model, assumptions, sizing constraints and protection boundaries.

6. Implement a reproducible Python/energy model with explicit inputs, assumptions, outputs and acceptance criteria.

7. Run scenario/corner analysis before escalating to further design tools.

8. Preserve equipment/product benchmark boundaries; do not convert benchmark values into site requirements without project data.

9. Treat site-specific, permit, utility and field claims as unsupported unless actual project data/evidence exists.

10. Re-evaluate GATE-5 with traceable evidence paths and STOP.

Return the standard completion report.

### 6.4 Fiziksel / saha sınırı

Actual site data olmadan permit/utility/field claim veya saha performansı kanıtlanmış sayılamaz. Model çıktısı yalnız recorded assumptions ve tested analytical envelope içinde CALCULATED evidence olarak kullanılmalıdır.

## 7. Reproducibility ve Physical Validation Handoff Loop

DESIGN / REQUIREMENT

↓

ANALYTICAL CHECK

↓

OPTIONAL QUALIFIED SIMULATION

↓

REV-A FABRICATION / ASSEMBLY

↓

CODEX LAB PROCEDURE

↓

OPERATOR MEASUREMENT

↓

EVIDENCE IMPORT

↓

EXPECTED vs ACTUAL

↓

ISSUE / DECISION UPDATE → REV-B if needed

- Numeric constants explicit provenance taşır; inputs machine-readable veya açıkça kaydedilmiş olmalıdır.

- Script output tool/version (uygunsa), input set, command, result ve acceptance criterion içermelidir.

- Tek başına plot kanıt değildir; underlying inputs/results de kaydedilir.

- Manufacturer model sessizce düzenlenmez; wrapper/parameter override kayıt altına alınır.

- Physical-test record: hardware revision/serial, firmware commit/config, equipment/settings, wiring/setup, environment/state, expected criterion, actual measurement, raw evidence path ve follow-up içermelidir.

## 8. GitHub / PR Uygulama Kuralları

- Evidence/gate closure PR’ına ilgisiz future implementation karıştırılmaz.

- Her PR affected requirements / decisions / gates’i açıkça yazar.

- Clean diff tek başına merge nedeni değildir; gate status + evidence MERGE\_READY durumunu desteklemelidir.

- Gate-closing PR merge sonrası sonraki implementation yeni branch/PR’da yürütülür; mevcut PR scope açıkça kapsamıyorsa aynı PR’a devam edilmez.

- Yalnız “continue” niteliğinde, unresolved blocker’ları tekrar eden commit’ler üretilmez.

- GATE-2 dört closure contract sonrası bir kez re-evaluate edilir; PASS ise FINAL REVIEW → MERGE\_READY. Merge işlemi ayrıca yetkilendirilir.

## 9. Standart Codex Completion Report

ACTIVE\_GATE: <GATE-ID>

GAPS\_CLOSED: \[IDs\]

GAPS\_OPEN: \[IDs + exact missing evidence\]

TOOLS\_RUN: \[tool / command / purpose\]

MODELS\_USED: \[status + qualification scope\]

FILES\_CHANGED: \[repo-relative paths\]

EVIDENCE\_ADDED: \[repo-relative paths\]

PHYSICAL\_VALIDATION\_QUEUE: \[tests\]

GATE\_RESULT: PASS | BLOCKED | NOT\_REEVALUATED

NEXT\_ACTION: one smallest sufficient action only

## 10. Güncel Codex Çalıştırma Sırası

ŞİMDİ:

PROJECT-01 / PR #1 / GATE-2

GAP-08 → GAP-03 → GAP-02 → DEC-11

→ GATE-2 RE-EVALUATION (once)

→ affected final checks

→ FINAL REVIEW → MERGE\_READY | exact blocker

→ STOP

KUYRUKTA:

PROJECT-02 → GATE-3

PROJECT-03 → GATE-4

PROJECT-04 → GATE-5

| **En küçük yeterli sonraki iş:** Mevcut baseline’a göre ilk Codex icrası P01-G2-00 ardından GAP-08’dir. P02–P04 için bu rapordaki promptlar hazırlık amaçlıdır; GATE-2 kapanmadan otomatik icra edilmemelidir. |
| --- |

## 11. Definition of Done

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

| **Sonuç:** Bu koşullardan biri eksikse proje OPEN, IN\_PROGRESS veya BLOCKED kalır. Release yalnız portfolio snapshot’tır; production readiness kanıtı değildir. |
| --- |
