**CODEX HANDOFF<br>CAN Bus + Energy Generation Portfolio**

**RESEARCH-HANDOFF REVISION — 18.09.2026**

| **Field** | **Value** |
| --- | --- |
| Handoff ID | CAN-ENERGY-PORTFOLIO-2026-09-15 |
| Revision basis | 15.09.2026 Research Records + Reference Lists + 17.09 reference baseline + PROJECT-01 GATE-2 evidence |
| Operating principle | Research in a separate project; return evidence-backed decisions; Codex executes after closure |
| Status | RESEARCH\_REQUIRED / CALCULATION\_REQUIRED / DESIGN\_INPUT\_REQUIRED / EVIDENCE REQUIRED |

Purpose: transfer unresolved engineering questions to a separate research project, where product research and calculations will close missing specifications before implementation.

# REVISION DELTA

- Broad product research is removed from the current-project/Codex execution scope.

- Product research, manufacturer-document comparison and engineering calculations are delegated to a separate research project.

- This handoff carries known facts, missing design inputs, researchable unknowns, decision dependencies, seed candidates, calculation requirements, closure criteria and return-package requirements.

- Seed products are not selected components; the research project may accept, reject or replace them.

- Internal screenshots and prior synthesis organize work but are not independent technical evidence.

- After research returns, Codex receives only frozen decisions, calculations, evidence paths, validation criteria and explicit residual blockers.

# 1. PRODUCT BOUNDARY

- PROJECT-01 — Solar-powered low-power ESP32 sensor board.

- PROJECT-02 — USB CAN/CAN FD interface board.

- PROJECT-03 — CAN-connected battery monitoring/interface system.

- PROJECT-04 — PV + BESS + backup-generation electrical design package.

Cross-project integration is optional. Do not expand a product merely because a related reference exists.

# 2. RETAINED SOURCE-JOB FRAME

| **ID** | **Product** | **Retained requirements** |
| --- | --- | --- |
| JOB-01 | Solar sensor | ESP32-WROOM-32E; solar + USB; 1S Li-ion; low-Iq 3.3 V; battery sensing; I²C sensor; deep sleep; KiCad 2-layer |
| JOB-02 | USB CAN/CAN FD | STM32H573RIT6; dual CAN/CAN FD; USB-C; USB/UART; SWD; protected voltage measurement; compact 2-layer |
| JOB-03 | CAN battery monitor | Per-cell voltage; Hall current; CAN/DroneCAN; AP\_Periph; status/parameters; firmware; calibration/test |
| JOB-04 | PV+BESS | Factory load; PV; inverter sizing/losses; AC-coupled BESS; variable load profile; SLD; energy modeling |

# 3. EVIDENCE / REFERENCE CLASSES

| **Class** | **Rule** |
| --- | --- |
| PRIMARY\_EVIDENCE | Manufacturer datasheet/manual, protocol specification or official reference documentation; supports facts only within cited conditions. |
| IMPLEMENTATION\_REFERENCE | Official/established code or reference design; supports patterns, not electrical claims by itself. |
| DISCOVERY | Search result, screenshot or repo list; follow to original source before closure. |
| INTERNAL\_SYNTHESIS | Prior analysis/assistant output/internal screenshot; cannot independently close a technical claim. |
| SEED\_CANDIDATE | Product/topology worth comparing; not selected until research + calculations justify it. |
| OUT\_OF\_SCOPE | Do not spend research or Codex budget. |

# 4. RESEARCH-HANDOFF MODEL

**KNOWN FACT → UNKNOWN → RESEARCH QUESTION → PRODUCT STUDY → CALCULATION → DECISION → EVIDENCE → FROZEN CONTRACT → CODEX**

| **Status** | **Use** |
| --- | --- |
| DESIGN\_INPUT\_REQUIRED | Actual project input must be supplied or explicitly assumed; generic research cannot discover it. |
| RESEARCH\_REQUIRED | External product/source comparison required. |
| CALCULATION\_REQUIRED | Reproducible engineering calculation required. |
| VALIDATION\_REQUIRED | Decision exists but simulation/bench/field validation remains. |
| BLOCKED | Prerequisite prevents safe progress. |
| PROVISIONAL | Working decision with bounded assumptions. |
| FROZEN | Evidence-backed contract ready for Codex implementation. |

## 4.1 Separate research project must return

- Question/decision ID and affected requirement IDs.

- Known facts and assumptions; DESIGN\_INPUT\_REQUIRED items separated from researchable unknowns.

- 2–3 relevant candidates where comparison is necessary, with primary manufacturer sources.

- Reproducible calculations: input table, formula/script, corners/sensitivity and acceptance criteria.

- Selected option or explicit NO-SELECTION/BLOCKED result, including rejected alternatives and reasons.

- Residual uncertainty and required simulation/bench validation.

- Proposed status: PROVISIONAL, FROZEN, DESIGN\_INPUT\_REQUIRED or BLOCKED.

# 5. PROJECT-01 — SOLAR LOW-POWER ESP32 SENSOR NODE

Research objective: freeze the power-tree, energy and I²C/sensor contracts before implementation. Do not implement the PCB inside the research project.

## 5.1 Known / bounded

- Target module remains ESP32-WROOM-32E. ESP32-C3 sources are comparative only unless an explicit architecture decision changes the module.

- BME280 is the current sensor direction; electrical claims require Bosch/NXP/Espressif primary evidence as applicable.

- Prior I²C work used a 400 pF worst-case assumption and 2.2 kΩ working pull-up recommendation; re-evaluate if actual Cbus or switched-rail topology changes.

- MCP73871 is an existing design direction, not a complete two-source arbitration solution.

- TPS7A02 must not be assumed as the sole ESP32 main regulator without peak/transient evidence.

## 5.2 Decision / research chain

| **ID** | **Package** | **Closure target** |
| --- | --- | --- |
| P01-R01 / DEC-01 | USB + solar source arbitration | Compare USB-priority, ideal-diode OR/power-mux or better topology; freeze source-state table and reverse-current behavior. |
| P01-R02 / DEC-02 | Charger / power-path | Retain MCP73871 + upstream arbitration or justify replacement; freeze CHARGER\_IN/BAT/SYS domains, input/charge limits and thermal assumptions. |
| P01-R03 / DEC-04 | Main 3.3 V regulator | Select >=500 mA-class main-rail approach using input range, Iq, dropout/region, transient margin and capacitor/stability evidence. |
| P01-R04 / DEC-13 | Battery protection | Freeze protected-1S-cell boundary or external protection architecture; include cutoff/leakage/failure assumptions. |
| P01-R05 / DEC-12 | BME280 + I²C power gating | Freeze VDD/VDDIO, pull-up rail, GPIO safe state, startup/reinit and off-state test contract. |
| P01-R06 / DEC-11 | Always-on leakage | Authoritative leakage/Iq budget for protection/arbitration parts; no guessed values. |
| P01-R07 | Battery sensing / ADC | Divider leakage, source impedance, filter, ADC accuracy and calibration calculation. |
| P01-R08 | Board-level energy model | Deep sleep, measurement, Wi-Fi TX, USB/solar-present, constrained and critical-battery modes. |

## 5.3 DESIGN\_INPUT\_REQUIRED

- Solar panel V\_OC, V\_MP, I\_MP, P\_MAX and cable/connector constraints.

- Battery chemistry, capacity, protection type and allowed charge current.

- Measurement/reporting interval and Wi-Fi transaction duration/retry policy.

- Deployment temperature/environment/enclosure constraints.

- Mandatory size, cost, connector or sourcing constraints not already recorded.

## 5.4 Seed candidates — not selections

| **Seed** | **Restriction** |
| --- | --- |
| MCP73871 | Existing charger/power-path direction; requires explicit upstream source policy and battery-protection boundary. |
| BQ24074-class | Alternative charger/power-path benchmark; evaluate, do not auto-replace. |
| TPS2113A-class | Power-mux/source-arbitration candidate; verify range, priority, current, leakage and solar interaction. |
| LTC4412 + P-MOSFET | Ideal-diode/source-OR candidate; verify FET orientation/Rds(on), leakage and both-source behavior. |
| TPS7A02 | Ultra-low-Iq auxiliary rail candidate; not automatically the ESP32 main rail. |
| Adafruit BQ24074 / FireBeetle-class boards | Architecture/documentation benchmarks only; not automatic BOM. |

# 6. PROJECT-02 — USB CAN/CAN FD

| **ID** | **Research package** | **Return condition** |
| --- | --- | --- |
| P02-R01 | STM32H573 FDCAN/USB/pin/clock capability | Source-supported peripheral and pin matrix. |
| P02-R02 | CAN/CAN FD physical layer | Transceiver candidates, voltage domains, common-mode, ESD, termination and isolation trade-off frozen. |
| P02-R03 | USB-C device interface | CC implementation, power assumptions and host/device behavior frozen. |
| P02-R04 | Protected voltage measurement | Protection, divider/filter/ADC scaling and error calculation. |
| P02-R05 | Host/message tooling | DBC/message model, SocketCAN/monitoring/test-vector strategy; transport separated from payload semantics. |

# 7. PROJECT-03 — CAN BATTERY MONITOR

| **ID** | **Research package** | **Return condition** |
| --- | --- | --- |
| P03-R01 | Measurement chain | Cell voltage, Hall current, temperature, ADC/AFE and isolation architecture with error budget. |
| P03-R02 | Calibration/filtering | Offset/gain, storage, filter dynamics, plausibility and fault thresholds quantified. |
| P03-R03 | CAN/DroneCAN telemetry | Units/scaling/update rates/status/fault messages; DroneCAN/AP\_Periph feasibility evidenced. |
| P03-R04 | Scope boundary | Monitor vs full-BMS boundary explicit; no silent scope expansion. |
| P03-R05 | Validation | Known-input, calibration, noise and fault-injection matrix with bench evidence requirements. |

# 8. PROJECT-04 — PV + BESS + BACKUP GENERATION

Terminology: PROJECT-04 'load model' means factory demand/load-profile model, not regulator load-line, probe loading, CAN/I²C bus loading, electronic-load hardware or AI model loading.

| **ID** | **Research / calculation package** | **Return condition** |
| --- | --- | --- |
| P04-R01 | Factory load model | Load-profile assumptions, peak demand, daily energy and scenarios reproducible. |
| P04-R02 | PV sizing | Production assumptions, capacity, inverter sizing, DC/AC ratio and losses calculated. |
| P04-R03 | BESS sizing | Energy/power rating, SOC window, efficiencies and operating objective calculated. |
| P04-R04 | Generator + ATS | Generator role, start/transfer logic, abnormal scenarios and operating-state matrix defined. |
| P04-R05 | Electrical design boundary | SLD assumptions, buses, switching, protection annotations and equipment-schedule basis explicit. |
| P04-R06 | Scenario validation | Normal, low-PV, high-load, BESS-depleted, grid-outage and generator modes reproduced. |

# 9. PRODUCT-RESEARCH AND CALCULATION RULES

- Start from a named decision/question, not a generic keyword.

- Prefer manufacturer datasheets/manuals, protocol specifications and official reference designs.

- Use GitHub/open-source implementations for patterns, not as substitutes for electrical specifications.

- Stop when the closure criterion is satisfied; do not collect similar products merely for confidence.

- Report CONFLICT when authoritative sources disagree and the conflict changes the design.

- Report DESIGN\_INPUT\_REQUIRED when the missing value belongs to the actual deployment context.

- Every numerical result must label input provenance as SOURCE\_SUPPORTED, DECISION\_SUPPORTED or ASSUMED.

- Use parameter sweeps when an input is not frozen; return safe operating regions instead of one invented nominal value.

- Simulation is an escalation after analytical work; model provenance and scope must be recorded.

- Physical claims remain VALIDATION\_REQUIRED until measured on actual hardware.

# 10. RESEARCH RETURN → CODEX EXECUTION GATE

| **Codex input** | **Required content** |
| --- | --- |
| Frozen decision | Selected topology/component/interface and rationale. |
| Evidence | Primary-source paths and exact conditions. |
| Calculation | Reproducible inputs, script/formula, result and acceptance criterion. |
| Implementation contract | Net/interface/state/parameter values Codex may implement without rediscovery. |
| Validation contract | What Codex can check mechanically/simulate and what remains physical. |
| Stop condition | Exact point where Codex must stop rather than reopen research. |

- Codex must not repeat product research already returned by the research project.

- Codex must not silently replace a FROZEN decision; implementation conflicts reopen the named decision only.

- Use cheapest sufficient tooling and rerun only checks invalidated by the current change.

- After acceptance: record evidence, close the task, STOP.

# 11. EVIDENCE MATURITY

| **Maturity** | **Meaning** |
| --- | --- |
| ASSUMED | Working value; explicit verification plan required. |
| SOURCE\_SUPPORTED | Primary/credible source supports value/behavior, not yet this design. |
| CALCULATED | Reproducible analytical result from recorded inputs. |
| DESIGN\_VERIFIED | Implementation checked against source-supported requirements. |
| BENCH\_VERIFIED | Actual hardware measured under defined setup. |
| FIELD\_VERIFIED | Observed under defined real-world conditions. |

Never infer BENCH\_VERIFIED or FIELD\_VERIFIED from documentation, simulation, DRC, build logs or narrative claims.

# 12. IMMEDIATE NEXT ACTION

- Transfer this handoff to the separate product-research project.

- Start with PROJECT-01 in dependency order: P01-R01 → P01-R02 → P01-R03 → P01-R04 → P01-R05/P01-R06 → P01-R07/P01-R08.

- Before researching, split each item into DESIGN\_INPUT\_REQUIRED versus RESEARCH\_REQUIRED versus CALCULATION\_REQUIRED.

- Return one compact closure package per decision; do not implement repository changes as part of product research.

- After the returned packages are accepted, produce a Codex execution handoff containing only FROZEN contracts and explicit remaining validation items.

# 13. DEFINITION OF RESEARCH-HANDOFF DONE

- Every unresolved engineering item has an owner status and closure criterion.

- Actual deployment inputs are not disguised as research questions.

- Every selected product/topology has primary evidence and reproducible calculations where required.

- Rejected alternatives and residual uncertainty are recorded.

- No seed candidate is silently promoted to selected component.

- Codex can implement each FROZEN decision without repeating broad discovery.

- Physical-validation items remain explicitly queued rather than claimed as complete.
