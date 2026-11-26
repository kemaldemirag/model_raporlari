**CAN BUS + ENERGY GENERATION PORTFOLIO**

# POZİSYON BAZLI<br>CODEX GÖREV AKIŞI RAPORU

*Revize İş Akışı Raporu temel alınarak hazırlanmıştır*

| **Kaynak rapor** | CAN\_Bus\_Enerji\_Uretimi\_Is\_Akisi\_Raporu\_REVISED\_16.09.2026.docx |
| --- | --- |
| **Handoff ID** | CAN-ENERGY-PORTFOLIO-2026-09-15 |
| **Tarih** | 16.09.2026 |
| **Kapsam** | 4 pozisyon / proje hattı için Codex görev planlaması |
| **Planlama ilkesi** | Gate-first, evidence-driven, limit-conscious execution |

## 1. Amaç ve Planlama Mantığı

Bu rapor, revize iş akışı raporundaki dört proje/pozisyon hattını ayrı Codex görev akışlarına dönüştürür. Phase 0, Phase 0A ve Phase 0B ayrı pozisyonlar değildir; her pozisyonun çalışmasını yöneten ortak yönetişim ve giriş kapılarıdır. Codex, ham araştırmadan doğrudan implementasyona geçmez; önce gereksinim, spesifikasyon ekseni, senaryo ve karar/kanıt açığı zincirini kurar.

research / source

→ reference classification

→ requirement mapping

→ specification axis

→ candidate scenario

→ decision / evidence gap

→ Codex action

→ implementation / simulation

→ verification

→ evidence

→ validation / release

### 1.1 Ortak yürütme kuralları

- Broad GitHub veya web discovery tekrarlanmayacak; yalnız unresolved-gap verification yapılacak.

- Kaynaksız sayısal değer requirement olarak sabitlenmeyecek.

- Her candidate numeric value SOURCE\_SUPPORTED, DECISION\_SUPPORTED veya OPEN/ASSUMED + verification plan durumlarından birine sahip olacak.

- VERIFIED, VALIDATED, TESTED, PASS, COMPLETE, PRODUCTION READY ve MANUFACTURING READY ifadeleri evidence path olmadan kullanılmayacak.

- Scenario matrix ile validation matrix ayrı tutulacak.

- Gate geçilmeden PCB layout, firmware implementation veya release işi açılmayacak.

- Placeholder klasör/dosya üretimi ilerleme olarak raporlanmayacak.

### 1.2 Dört pozisyonun ana kuyruğu

| **#** | **Pozisyon / Proje** | **Ana ara çıktı** | **Gate** | **Dalga** |
| --- | --- | --- | --- | --- |
| 1 | PROJECT-01 — Solar-Powered Low-Power Sensor | I2C contract + power-state model | GATE-2 | 1 |
| 2 | PROJECT-02 — USB CAN/CAN FD Interface | CAN specification matrix | GATE-3 | 1 |
| 3 | PROJECT-03 — CAN Battery Monitoring Node | Measurement-to-telemetry contract | GATE-4 | 2 |
| 4 | PROJECT-04 — PV + BESS + Backup Generation | Operating scenario matrix | GATE-5 | 3 |

## 2. Pozisyon 1 — PROJECT-01: Solar-Powered Low-Power Sensor

Bu akışın amacı mevcut I2C ve low-power araştırmasını PCB öncesi davranış sözleşmelerine dönüştürmek, ardından board-level güç bütçesi ve doğrulama zinciri üzerinden implementasyona geçmektir.

| **Görev ID** | **Görev** | **Ana çıktı** | **Bağımlılık / koşul** | **Durum** |
| --- | --- | --- | --- | --- |
| P01-01 | Requirement Inventory | 02\_requirements/requirements.md | Mevcut screened araştırma | OPEN |
| P01-02 | Reference Classification | Sınıflandırılmış kaynak kayıtları | P01-01 ile paralel | OPEN |
| P01-03 | Specification / Scenario Matrix | PROJECT-01 scenario matrix | P01-01/02 | OPEN |
| P01-04 | I2C Interface Contract | i2c\_interface\_contract.md | GATE-0/1 girişleri | OPEN |
| P01-05 | Power-State Model | power\_state\_model.md | P01-04 | OPEN |
| P01-06 | Board-Level Power Budget | Güç bütçesi / varsayım kaydı | P01-03/05 | OPEN |
| P01-07 | Decision / Evidence Gap Closure | decision\_register güncellemesi | P01-03..06 | OPEN |
| P01-08 | PCB / Firmware Implementation | Tasarım / firmware çıktıları | GATE-2 sonrası | BLOCKED |
| P01-09 | Verification | Test sonuçları + evidence paths | P01-08 | BLOCKED |
| P01-10 | Validation Summary | validation\_matrix.csv + validation\_summary.md | P01-09 | BLOCKED |

### 2.1 Zorunlu kapsam

- I2C voltage domain ve pull-up stratejisi

- Address inventory, discovery ve initialization davranışı

- ACK/NACK ve missing-device davranışı

- Uygunsa duplicate-address handling kararı

- Sensor power-gating sonrası stabilization ve bus re-initialization

- sleep → wake → sensor power-on → stabilization → I2C init/discovery → measurement → telemetry → peripheral shutdown → sleep

- Board-level sleep-current budget: ESP32 state current + regulator Iq + divider leakage + sensor standby/power-gating etkileri

- Energy-surplus, energy-neutral ve energy-deficit senaryoları

| **GATE-2** | I2C interface contract + power-state model + source/decision-supported numeric assumptions hazır olmadan PCB implementation başlamaz. |
| --- | --- |

### 2.2 Pozisyon 1 — Codex ana promptu

ROLE: PROJECT-01 Solar-Powered Low-Power Sensor Engineer

SCOPE: 01\_solar\_low\_power\_sensor/

TASK RANGE: P01-01..P01-10

Use the current workflow governance. Do not perform broad discovery.

Use existing screened research first and perform only targeted unresolved-gap verification.

Execution order:

1. Build/update canonical requirements.

2. Classify references as RELEVANT, SUPPORTING, NEEDS\_VERIFICATION or OUT\_OF\_SCOPE.

3. Build the PROJECT-01 specification/scenario matrix.

4. Create i2c\_interface\_contract.md covering voltage domain, pull-up strategy,

address inventory, discovery, initialization, ACK/NACK, missing-device behavior,

duplicate-address handling where applicable, power-gating, stabilization and re-init.

5. Create power\_state\_model.md for:

sleep → wake → sensor power-on → stabilization → I2C init/discovery

→ measurement → telemetry → peripheral shutdown → sleep.

6. Build a board-level power budget including MCU state current, regulator Iq,

divider leakage, sensor standby and power-gating effects where evidence exists.

7. Record energy-surplus, energy-neutral and energy-deficit scenarios.

8. Keep unsupported numeric values OPEN/ASSUMED with verification plans.

9. Evaluate GATE-2 before any PCB or firmware implementation.

10. After GATE-2 only, open implementation, verification and validation tasks.

Never use VERIFIED/PASS/COMPLETE without an evidence path.

At the end report:

- changed files

- decisions

- OPEN assumptions

- evidence gaps

- GATE-2 status

- next executable task

## 3. Pozisyon 2 — PROJECT-02: USB CAN/CAN FD Interface

Bu akışın amacı STM32H573, FDCAN, USB/USB-C ve CAN transceiver yeteneklerini primary evidence ile bağlamak; numeric CAN FD profillerini seçmeden önce physical-layer, data-rate ve termination senaryolarını açık hale getirmektir.

| **Görev ID** | **Görev** | **Ana çıktı** | **Bağımlılık / koşul** | **Durum** |
| --- | --- | --- | --- | --- |
| P02-01 | Requirement Inventory | 02\_requirements/requirements.md | Mevcut screened araştırma | OPEN |
| P02-02 | Reference Classification | Sınıflandırılmış kaynak kayıtları | P02-01 ile paralel | OPEN |
| P02-03 | Primary Evidence Verification | MCU/USB/transceiver evidence | P02-01/02 | OPEN |
| P02-04 | CAN Specification Matrix | can\_specification\_matrix.md | P02-03 | OPEN |
| P02-05 | Termination Scenario Pack | No/one/two termination cases | P02-04 | OPEN |
| P02-06 | Fault / Recovery Contract | Disconnected, bus-off, recovery senaryoları | P02-04 | OPEN |
| P02-07 | Candidate CAN FD Profiles | Candidate nominal/data bitrate seti | P02-03..06 | OPEN |
| P02-08 | Hardware Architecture | Tasarım çıktıları | GATE-3 sonrası | BLOCKED |
| P02-09 | Firmware Implementation | USB/FDCAN firmware | P02-08 | BLOCKED |
| P02-10 | Verification / Evidence | validation matrix + evidence | P02-08/09 | BLOCKED |

### 3.1 Zorunlu kapsam

- STM32H573 peripheral capabilities ve FDCAN pin mappings primary evidence ile doğrulanır.

- USB device implementation ve USB-C CC davranışı kanıta bağlanır.

- Transceiver voltage compatibility ve physical-layer data-rate limiti incelenir.

- Nominal bitrate × data bitrate profilleri candidate olarak tutulur; kanıtsız requirement olmaz.

- Selectable 120 Ω termination için no/one/two-termination test cases oluşturulur.

- Disconnected-channel, bus-off ve recovery davranışları test senaryolarına eklenir.

- FPGA-CAN ve farklı controller örnekleri yalnız SUPPORTING kalır; approved decision olmadan ana mimari girdisi olmaz.

| **GATE-3** | CAN specification matrix ve termination/data-rate evidence hazır olmadan numeric CAN FD profili sabitlenmez. |
| --- | --- |

### 3.2 Pozisyon 2 — Codex ana promptu

ROLE: PROJECT-02 USB CAN/CAN FD Interface Engineer

SCOPE: 02\_usb\_can\_fd\_interface/

TASK RANGE: P02-01..P02-10

Use existing screened evidence first. Do not repeat broad GitHub discovery.

Perform only targeted verification for unresolved engineering gaps.

Execution order:

1. Build/update canonical requirements.

2. Classify references.

3. Verify with primary evidence:

- STM32H573 FDCAN capability and pin mapping

- USB device constraints

- USB-C CC behavior

- transceiver voltage compatibility

- transceiver physical-layer data-rate capability

4. Create can\_specification\_matrix.md containing requirement ID, specification axis,

candidate scenario/value, source, evidence strength, decision status,

verification method and evidence path.

5. Create termination scenarios for no termination, one termination and two terminations.

6. Define disconnected-channel, bus-off and recovery behavior.

7. Keep nominal/data bitrate combinations as CANDIDATES until MCU + transceiver +

topology evidence or an approved decision supports them.

8. Treat FPGA-CAN or unrelated controller implementations only as SUPPORTING evidence.

9. Evaluate GATE-3 before hardware/firmware implementation.

10. After GATE-3 only, open architecture, firmware and verification work.

Never promote an unsupported numeric profile into a requirement.

At the end report:

- changed files

- candidate profiles

- supporting evidence

- OPEN gaps

- GATE-3 status

- next executable task

## 4. Pozisyon 3 — PROJECT-03: CAN Battery Monitoring Node

Bu akış generic “battery monitor” araştırmasını genişletmek yerine ölçüm zincirini telemetry semantiğine bağlar. Başlangıç noktası measurement-to-telemetry evidence ve calibration/fault semantics olmalıdır.

| **Görev ID** | **Görev** | **Ana çıktı** | **Bağımlılık / koşul** | **Durum** |
| --- | --- | --- | --- | --- |
| P03-01 | Requirement Inventory | PROJECT-03 requirements | Measurement/protocol interface yeterince açık | OPEN |
| P03-02 | Measurement Architecture | Measurement chain tanımı | P03-01 | OPEN |
| P03-03 | AFE / ADC Evidence | Cell AFE, Hall, ADC evidence | P03-02 | OPEN |
| P03-04 | Calibration & Error Budget | Calibration/error modeli | P03-03 | OPEN |
| P03-05 | Measurement-to-Telemetry Contract | measurement\_to\_telemetry\_contract.md | P03-02..04 | OPEN |
| P03-06 | Plausibility / Fault Semantics | Stale/impossible/fault davranışları | P03-05 | OPEN |
| P03-07 | DroneCAN Applicability | Protocol applicability kararı | P03-05/06 | OPEN |
| P03-08 | Firmware / Protocol Implementation | Firmware/protocol çıktısı | GATE-4 sonrası | BLOCKED |
| P03-09 | Verification | Test + evidence | P03-08 | BLOCKED |
| P03-10 | Evidence Package | validation matrix + summary | P03-09 | BLOCKED |

### 4.1 Measurement chain

physical quantity

→ sensing element / AFE

→ ADC / raw value

→ calibration

→ engineering unit

→ plausibility / status

→ telemetry field

- Cell AFE/ADC architecture, Hall current sensor polarity, ADC error budget ve calibration points birlikte ele alınır.

- Plausibility yalnız threshold değildir; stale ve impossible measurement davranışlarını da kapsar.

- DroneCAN feasibility ve BatteryInfo/cell-voltage semantics yalnız applicable ise kullanılır.

- Generic battery monitor araması evidence sayılmaz; araştırma AFE, Hall, ADC/calibration, fault/status ve protocol eksenlerine daraltılır.

- Candidate component ve numeric ranges primary reference + job constraints olmadan OPEN kalır.

| **GATE-4** | measurement\_to\_telemetry\_contract.md yoksa protocol/firmware “tamamlandı” sayılamaz. |
| --- | --- |

### 4.2 Pozisyon 3 — Codex ana promptu

ROLE: PROJECT-03 CAN Battery Monitoring Node Engineer

SCOPE: PROJECT-03 only

TASK RANGE: P03-01..P03-10

Start only when the relevant measurement and protocol interfaces are sufficiently defined.

Do not use generic battery-monitor search results as evidence.

Execution order:

1. Build/update canonical requirements.

2. Define the measurement architecture:

physical quantity → sensing/AFE → ADC/raw → calibration → engineering unit

→ plausibility/status → telemetry field.

3. Collect/verify evidence for cell AFE/ADC architecture, Hall sensor polarity,

ADC error budget, calibration points and reproducibility.

4. Create measurement\_to\_telemetry\_contract.md.

5. For every measurement define sensing method, raw representation, calibration,

engineering unit, valid behavior, stale/impossible behavior, status/fault semantics

and telemetry field mapping.

6. Evaluate DroneCAN / BatteryInfo / cell-voltage semantics only when applicable.

7. Keep unsupported components and numeric ranges OPEN.

8. Evaluate GATE-4 before protocol/firmware completion claims.

9. After GATE-4, open implementation, verification and evidence tasks.

Never call the protocol COMPLETE without the measurement-to-telemetry contract and evidence.

At the end report:

- field mappings

- calibration gaps

- protocol applicability

- OPEN items

- GATE-4 status

- next executable task

## 5. Pozisyon 4 — PROJECT-04: PV + BESS + Backup Generation

Bu akış normal işletmeye göre tek seferlik boyutlandırma yapmak yerine operating-scenario matrix tarafından sürülen, varsayımları açık ve tekrar üretilebilir bir hesap/simülasyon paketi üretir.

| **Görev ID** | **Görev** | **Ana çıktı** | **Bağımlılık / koşul** | **Durum** |
| --- | --- | --- | --- | --- |
| P04-01 | Requirement / Assumption Inventory | Varsayım ve değişken envanteri | Mevcut job/research girdisi | OPEN |
| P04-02 | Operating Scenario Matrix | operating\_scenario\_matrix.md | P04-01 | OPEN |
| P04-03 | Load / PV / BESS Variable Model | Parametrik model | P04-02 | OPEN |
| P04-04 | Dispatch Strategy Model | Peak shaving / self-consumption / reserve | P04-03 | OPEN |
| P04-05 | Generator / ATS Model | Start/stop/transfer varsayımları | P04-02..04 | OPEN |
| P04-06 | Failure Scenarios | Arıza/erişilemezlik senaryoları | P04-02..05 | OPEN |
| P04-07 | Sensitivity Analysis | 1h/2h/4h vb. candidate analizleri | P04-03..06 | OPEN |
| P04-08 | Reproducible Simulation Package | Hesap/simülasyon paketi | GATE-5 sonrası | BLOCKED |
| P04-09 | Validation / Evidence | validation matrix + summary | P04-08 | BLOCKED |

### 5.1 Minimum operating scenarios

1. SCN-01 — Normal day

2. SCN-02 — PV surplus

3. SCN-03 — Evening/production peak

4. SCN-04 — Grid outage

5. SCN-05 — Outage + low BESS SOC

6. SCN-06 — BESS unavailable

7. SCN-07 — Generator unavailable / failed start

8. SCN-08 — ATS transfer

9. SCN-09 — Grid return

### 5.2 Modelleme eksenleri

- BESS power \[kW\], energy \[kWh\], duration \[h\], SOC window ve efficiency ayrı değişkenlerdir.

- Dispatch strategy: peak shaving, self-consumption ve reserve behavior birbirinden ayrılır.

- Generator start/stop assumptions ve ATS transfer-mode assumptions açıkça belgelenir.

- 1 h / 2 h / 4 h duration değerleri sensitivity candidate olabilir; load model + design objective gerekçesi olmadan requirement değildir.

| **GATE-5** | operating\_scenario\_matrix.md yoksa BESS/generator/ATS modeli release adayı değildir. |
| --- | --- |

### 5.3 Pozisyon 4 — Codex ana promptu

ROLE: PROJECT-04 PV + BESS + Backup Generation Modeling Engineer

SCOPE: PROJECT-04 only

TASK RANGE: P04-01..P04-09

Create a scenario-driven, reproducible calculation/simulation workflow.

Expose every assumption and never convert unsupported durations into requirements.

Execution order:

1. Build requirement/assumption inventory.

2. Model separately:

- BESS power \[kW\]

- BESS energy \[kWh\]

- duration \[h\]

- SOC window

- efficiency

3. Create operating\_scenario\_matrix.md.

4. Include at minimum:

normal day, PV surplus, evening/production peak, grid outage,

outage + low BESS SOC, BESS unavailable, generator unavailable/failed start,

ATS transfer and grid return.

5. Distinguish dispatch strategies:

peak shaving, self-consumption and reserve behavior.

6. Document generator start/stop assumptions, ATS transfer assumptions and grid-return behavior.

7. Treat 1 h / 2 h / 4 h as sensitivity candidates unless justified by load model

and design objective.

8. Evaluate GATE-5 before release-candidate modeling claims.

9. After GATE-5, package the reproducible simulation/calculation workflow and validate it.

At the end report:

- scenario coverage

- assumptions

- sensitivity variables

- missing inputs

- GATE-5 status

- next executable task

## 6. Birleşik Codex Görev Kuyruğu

Codex limitini korumak için dört pozisyon aynı anda tam implementasyona açılmaz. PROJECT-01 ve PROJECT-02 pilot hatlardır; PROJECT-03 interface/protocol olgunluğuna, PROJECT-04 ise operating-scenario modeline göre ilerler.

DALGA 0 — Ortak governance

Phase 0 → Phase 0A → Phase 0B

DALGA 1 — Pilot pozisyonlar

PROJECT-01: P01-01 → P01-07 → GATE-2

PROJECT-02: P02-01 → P02-07 → GATE-3

DALGA 2

PROJECT-03: P03-01 → P03-07 → GATE-4

DALGA 3

PROJECT-04: P04-01 → P04-07 → GATE-5

DALGA 4 — Gate sonrası

Implementation / Simulation

→ Verification

→ Evidence

→ Cross-project consistency

→ Release

### 6.1 Codex oturumu kapanış formatı

CHANGED FILES

- ...

DECISIONS

- ...

OPEN / ASSUMED

- ...

EVIDENCE GAPS

- ...

GATE STATUS

- GATE-X: PASS / BLOCKED

- Evidence: ...

NEXT EXECUTABLE TASK

- TASK-ID: ...

- Reason: ...

### 6.2 Sprint-fail koşulları

- Broad GitHub discovery tekrar edilirse.

- Unsupported numeric requirement üretilirse.

- Evidence path olmadan VERIFIED/PASS/COMPLETE kullanılırsa.

- Placeholder üretimi ilerleme gibi gösterilirse.

- Gate geçmeden PCB/firmware/release işine geçilirse.

- Client-delivery, production-ready veya manufacturing-ready izlenimi kanıtsız oluşturulursa.

## 7. Sonuç

Görev planı dört pozisyonu korur ve rapordaki Phase 0/0A/0B mantığını pozisyon içi giriş kapıları olarak uygular. İlk gerçek Codex çalışma paketi PROJECT-01 ve PROJECT-02 için requirement/reference/specification işlerini tamamlayıp GATE-2 ve GATE-3’e ulaşmaktır. PROJECT-03 ve PROJECT-04 promptları hazır tutulur ancak kendi bağımlılıkları sağlanmadan implementasyona açılmaz. Bu yapı araştırma tekrarını azaltır, sayısal varsayımların erken sabitlenmesini engeller ve her pozisyonda çıktı → verification → evidence → validation zincirini izlenebilir tutar.

**Kaynak:** CAN Bus + Energy Generation Portfolio — Revize İş Akışı Raporu, 16.09.2026. Bu belge, kaynak rapordaki dört PROJECT hattı, gate’ler, artifact zorunlulukları, araştırma politikası ve validation kuralları esas alınarak hazırlanmıştır.
