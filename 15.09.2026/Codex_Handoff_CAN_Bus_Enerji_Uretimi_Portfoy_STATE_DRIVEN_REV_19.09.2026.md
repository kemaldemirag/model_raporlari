**CODEX HANDOFF<br>CAN Bus + Energy Generation Portfolio**

**STATE-DRIVEN / SCENARIO × ECAD × DECISION REVISION — 19.09.2026**

| **Field** | **Value** |
| --- | --- |
| Handoff ID | CAN-ENERGY-PORTFOLIO-2026-09-15 |
| Purpose | Close the next authorized engineering cell without reopening settled research or drifting architecture. |
| Canonical research roots | Araştırma Kayıtları/15.09.2026 + Referans Listeler/15.09.2026 |
| State authority | Current repo/decision register/active gate > current closure evidence > synthesis docs > raw research records |
| Core model | Product × Scenario × ECAD/Engineering Module × Decision × Evidence Dependency |
| Control principle | Preserve baseline unless evidence/calculation invalidates it; invalidate only affected downstream cells. |

# 0. EXECUTION CONTRACT — READ FIRST

- Do not redesign the products. Close the first authorized OPEN/BLOCKED cell whose prerequisites are satisfied.

- DEVAM means: execute the next queue item automatically. Ask only for a true DESIGN\_INPUT\_REQUIRED value, an equivalent user-choice trade-off, or repository write/merge authorization.

- Do not reopen RESEARCH\_CLOSED / DECIDED(candidate) work because related material is found. Reopen only on authoritative contradiction, component-limit violation, failed calculation, or implementation evidence that invalidates the assumption.

- A component replacement invalidates only its dependent evidence, calculations, interfaces and ECAD modules.

- New findings that do not block the active closure criterion are NEW-GAP/DEFERRED and must not derail the active queue item.

- After closure: propagate dependency invalidation, update gate/registers, identify the next queue item, then STOP.

# 1. CONTROL PLANE vs PRODUCT PLANE

| **Plane** | **Contains** | **Rule** |
| --- | --- | --- |
| CONTROL PLANE | Handoff, workflow/task flow, requirements, decisions, evidence gaps, research register, queue/gates | Controls work; compact and state-authoritative. |
| PRODUCT PLANE | KiCad/ECAD, firmware, calculations/scripts, models, test vectors, drawings, fabrication outputs, real evidence | Engineering product; governance artifacts do not substitute for it. |

# 2. STATE AUTHORITY

| **Priority** | **Source** | **Authority** |
| --- | --- | --- |
| 1 | Current repository state + decision\_register + active gate + current branch/PR evidence | Authoritative current project state. |
| 2 | Current research closure packages + primary evidence | May close/reopen the named decision only. |
| 3 | Synthesis documents | Context; cannot overwrite newer repo state. |
| 4 | Raw records/screenshots/discovery | Discovery only until promoted and verified. |

Older synthesis that says OPEN must not reopen a decision subsequently closed in the current repo.

# 3. STATES AND AUTHORITY TYPES

| **State** | **Meaning** |
| --- | --- |
| SOURCE\_SUPPORTED | Source supports claim within cited conditions. |
| CALCULATED | Reproducible calculation from recorded inputs. |
| RESEARCH\_CLOSED | Required research sufficient; do not repeat discovery. |
| DECIDED(candidate) | Candidate selected; downstream contract/evidence may remain. |
| PROVISIONAL | Working decision with named unresolved validation/input. |
| FROZEN | Implementation contract ready for consumption without rediscovery. |
| VALIDATION\_REQUIRED | Research/decision may be closed while validation remains. |
| DESIGN\_INPUT\_REQUIRED | Real input must come from user/site/product; do not invent. |
| BLOCKED | Prerequisite prevents next authorized step. |
| HISTORICAL\_INVALID\_FOR\_CURRENT\_BOM | Historically correct evidence no longer applies to current BOM/topology. |

| **Authority** | **Allowed** |
| --- | --- |
| TYPE-A VERIFY | Verify existing baseline only. |
| TYPE-B SELECT | Select among bounded candidates using stated criteria. |
| TYPE-C REOPEN | Baseline is invalidated; research minimum replacement class only. |

# 4. INVALIDATION RULES

- Never transfer a numerical property from a replaced component to its replacement.

- Part change: old part-specific evidence becomes HISTORICAL\_INVALID\_FOR\_CURRENT\_BOM; open only missing replacement evidence; dependent calculations become STALE.

- Topology change: reopen only directly affected ECAD modules and downstream interfaces/calculations.

- Physical validation pending does not mean research is open.

- Missing deployment input does not justify more generic web research.

# 5. PRODUCT BASELINES

| **Product** | **Engineering center** | **First gate** |
| --- | --- | --- |
| P01 Solar Low-Power ESP32 Sensor | Energy + sensor + low-power duty cycle; current ESP32-WROOM-32E/BME280 baseline | Finish current GATE-2 blockers and BQ24074 replacement-evidence propagation. |
| P02 Dual USB CAN/CAN-FD | USB ↔ STM32H573 ↔ dual FDCAN channels | STM32H573RIT6 simultaneous package pin/peripheral feasibility. |
| P03 CAN/DroneCAN Battery Monitor | Measurement → calibration → engineering state → DroneCAN health/telemetry | Measurement envelope + calibration + telemetry/fault semantics. |
| P04 PV + AC-Coupled BESS | Factory load ↔ PV ↔ BESS ↔ grid/generator | Real load profile + existing SLD + dispatch objective. |

ESP32-C3 + LiFePO4 + VL53L1X and similar studies are NON-CANONICAL alternative scenarios unless the authoritative repo explicitly reopens P01 architecture.

# 6. P01 — CURRENT STATE

| **Item** | **Current state** | **Required propagation** |
| --- | --- | --- |
| DEC-03 / P01-R02 charger | DECIDED(candidate): TI BQ24074 | Do not reopen broad charger research. Add BQ24074-specific current/leakage/thermal/input evidence. |
| I\_charger\_quiescent | OPEN for BQ24074 | Do not reuse MCP73871 ~30 µA. Source the applicable BQ24074 current terms and conditions; update power budget afterward. |
| DEC-11 / P01-R06 protection | DECIDED(candidate), rationale narrowed | BQ24074 OVP handles charger abs-max envelope; TVS/ESD remains surge/ESD protection. Recalculate leakage role only. |
| P01-R03 main 3V3 | PROVISIONAL / BLOCKED | Close regulator contract; retain PV-01 brownout/dropout validation. |
| P01-R04 battery protection | PROVISIONAL / BLOCKED | Close protection boundary, procurement assumption and PV-06 cutoff coordination. |
| P01-R05 BME280/I2C gating | DECIDED(candidate) / VALIDATION\_REQUIRED | Do not re-research; carry switched-domain/high-Z contract into ECAD. |
| GATE-2 | BLOCKED | Open only after R03/R04 closure plus required BQ24074 evidence propagation. |

## 6.1 P01 Scenario × ECAD

| **Scenario** | **ECAD module** | **Focus** |
| --- | --- | --- |
| Solar only / cold Voc | Solar input + charger | Panel input envelope, BQ24074 limits/OVP, surge role. |
| USB only | USB-C + charger | VBUS/CC/protection and charge/system behavior. |
| USB + solar / handover | Source arbitration + charger | No backfeed, source-state and transient behavior. |
| Wi-Fi TX peak | Main 3V3 + ESP32 core | Peak/transient, dropout/brownout, decoupling. |
| Low battery | Battery + 3V3 + firmware policy | Cutoff coordination and operating region. |
| Sensor off | Load switch + I2C | VDD/VDDIO/pull-up/high-Z/back-power. |
| Deep sleep | All always-on paths | Current/leakage budget with current-BOM evidence. |
| Battery measurement | ADC frontend | Divider leakage, settling, calibration/error. |
| USB debug | USB-UART + rail interlock | No backfeed/injection between debug VBUS and system rail. |

- ECAD-P01-01 USB-C Input/Protection

- ECAD-P01-02 Solar Input/Protection

- ECAD-P01-03 Source Arbitration

- ECAD-P01-04 Charger/Power Path — BQ24074 candidate

- ECAD-P01-05 Battery Protection

- ECAD-P01-06 Main 3V3 + decoupling

- ECAD-P01-07 Sensor Load Switch

- ECAD-P01-08 I²C Electrical Interface

- ECAD-P01-09 Battery ADC Front End

- ECAD-P01-10 ESP32 Core/boot/antenna/debug

- ECAD-P01-11 Test points/current-measure links

- ECAD-P01-12 USB-UART/debug power interlock

## 6.2 P01 EXECUTION QUEUE

| **Queue** | **Action** | **STOP** |
| --- | --- | --- |
| P01-Q1 | Propagate BQ24074 replacement evidence; open only replacement-specific missing values and stale calculations. | BQ24074 provenance explicit; stale power-budget terms identified. |
| P01-Q2 | Close P01-R03 main 3V3 decision/contract. | Decision/contract updated; PV-01 validation plan named. |
| P01-Q3 | Close P01-R04 battery protection boundary. | Protection boundary and PV-06 plan explicit. |
| P01-Q4 | Recalculate power/energy budget using current BOM evidence only. | No historical component current remains active. |
| P01-Q5 | Re-evaluate GATE-2. | Authorize ECAD contract implementation or stop on exact remaining blocker. |
| P01-Q6 | Implement/refresh affected ECAD modules only. | ERC/DRC/interface checks for changed modules; unrelated modules untouched. |

# 7. P02 — DETERMINISTIC QUEUE

| **Queue** | **Package** | **Output** |
| --- | --- | --- |
| P02-Q1 | STM32H573RIT6 pin/peripheral feasibility | Two FDCAN + USB + SWD + UART + ADC + clock/control/power pin map. STOP if simultaneous mapping fails. |
| P02-Q2 | CAN PHY + termination + protection | CAN-A/B ECAD contracts: transceiver/VIO, independent termination, TVS/CMC/ground. |
| P02-Q3 | Isolation decision | Non-isolated vs isolated architecture; power/area/creepage/EMI implications. |
| P02-Q4 | USB + debug UART | USB-device/debug ECAD contract; decide if separate USB-UART is necessary. |
| P02-Q5 | Protected voltage ADC | Frontend + calibration/error budget. |
| P02-Q6 | Power architecture | Freeze rails; then authorize KiCad implementation. |

Target ECAD sheets: usb\_power, stm32h5, can\_channel\_a, can\_channel\_b, uart\_debug, voltage\_sense.

# 8. P03 — DETERMINISTIC QUEUE

| **Queue** | **Package** | **Output** |
| --- | --- | --- |
| P03-Q1 | Measurement envelope | Cell/pack voltage, current range, accuracy, common-mode, rate, isolation, fault limits. |
| P03-Q2 | Measurement architecture | MCU ADC+divider vs monitor/AFE; comparison + error budget. |
| P03-Q3 | Hall sensing + calibration | Range, offset/gain/drift, bandwidth, ADC compatibility, calibration model. |
| P03-Q4 | Measurement → DroneCAN | Raw → engineering unit → validity/health → DroneCAN field/update rate. |
| P03-Q5 | Fault/health + parameters | Fault semantics and AP\_Periph parameter contract. |
| P03-Q6 | Physical CAN + power/protection | Only after measurement/semantic contract; then ECAD physical layer. |
| P03-Q7 | Validation | Known-input/simulated-load/calibration/telemetry tests; do not claim unavailable 400 A bench. |

# 9. P04 — DETERMINISTIC QUEUE

| **Queue** | **Package** | **Output** |
| --- | --- | --- |
| P04-Q1 | Site/load baseline | Factory load profile, existing SLD, grid point/voltage, tariff/schedule, missing inputs. |
| P04-Q2 | Dispatch objective | Peak shaving/self-consumption/backup/clipping-recovery priority. |
| P04-Q3 | PV architecture | Module/inverter envelope, temperature string voltage, MPPT/string topology, DC:AC, loss/yield. |
| P04-Q4 | AC-coupled BESS | PCS/battery/AC bus/EMS/meter architecture + commercial-reference comparison. |
| P04-Q5 | BESS sizing | Reproducible kW/kWh dispatch/energy calculations with SOC/efficiency/degradation assumptions. |
| P04-Q6 | Tie-in/protection/siting | Power flow, transformer/switchgear, cable/protection and siting constraints. |
| P04-Q7 | SLD/equipment schedule | Buildable design package with assumptions/evidence links. |
| P04-Q8 | Scenario validation | Normal, low-PV, high-load, BESS full/empty, grid outage, generator/ATS/recovery as applicable. |

# 10. RESEARCH OPEN/CLOSE RULES

- Open new research only when it closes a named requirement, decision, evidence gap or failed calculation.

- Use 2–4 alternatives only when selection is genuinely open; do not expand candidate count after closure criteria are met.

- Primary source numerical limits outrank community repositories and marketing content.

- Reference designs/EVMs/working products bridge implementation context but are not automatic selection evidence.

- Record rejected alternatives so future agents do not repeat the same research.

- FROZEN decisions authorize downstream implementation; Rev-A → analysis/simulation/test → finding → Rev-B iteration remains allowed.

- Evidence folders contain produced evidence, not plans or gap lists.

# 11. 'DEVAM' SEMANTICS

When the user says DEVAM, execute the first queue item with satisfied prerequisites. Do not ask 'which task?' and do not switch to PR/merge management unless the queue explicitly reaches a repository-state action requiring user authorization.

- If technical prerequisite missing and it is DESIGN\_INPUT\_REQUIRED: ask only for that exact input.

- If two technically valid alternatives require a user preference not inferable from requirements: present the bounded choice.

- If a write/merge/push action is needed: stop and request exact authorization.

- Otherwise execute the queue item, update state/dependencies, report closure, then STOP.

# 12. DEFINITION OF HANDOFF SUCCESS

- Claude/Codex can identify the next task without re-planning the project.

- Closed research is not repeated.

- Component replacement invalidates only dependent evidence/calculations.

- Each scenario maps to an engineering/ECAD module and closure evidence.

- Control-plane documents lead to real product-plane artifacts.

- P01 current state is not overwritten by stale synthesis or non-canonical scenario studies.

- P02/P03/P04 begin at their actual first engineering gates rather than generic component search.

- Every task ends with a closure criterion and STOP.
