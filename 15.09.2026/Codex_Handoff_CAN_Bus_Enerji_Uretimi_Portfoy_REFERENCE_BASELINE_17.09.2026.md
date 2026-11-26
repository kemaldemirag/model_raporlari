**CODEX HANDOFF**

**CAN Bus + Energy Generation Portfolio**

**REVISED — 17.09.2026**

| **Handoff ID** | CAN-ENERGY-PORTFOLIO-2026-09-15 |
| --- | --- |
| **Workspace type** | Job-derived engineering portfolio |
| **Revision basis** | 15.09.2026 research records + reference lists + current PROJECT-01 PR/GATE-2 evidence |
| **Operating principle** | Gate-first, evidence-driven, limit-conscious execution |
| **Status** | GATE-DRIVEN / EVIDENCE REQUIRED / PHYSICAL VALIDATION PENDING |

*Purpose: reduce Codex discovery/research work and make it execute only the smallest sufficient engineering action required to close an evidence gap.*

# REVISION DELTA — WHAT CHANGED

| **This revision changes the handoff from a project-introduction document into an execution contract.**<br>The previous FIRST CODEX ACTION (“create the workspace skeleton / Phase 0”) is retired. The workspace and PROJECT-01 execution already exist.<br>Broad research is no longer the default action. Codex must first reuse existing evidence and resolve only the currently open evidence gap.<br>Tool use is now governed by cheapest-sufficient-tool escalation: calculation → scripted sweep → EDA/build checks → simulation → physical validation.<br>Simulation results may not be treated as evidence until the model is qualified for the specific behavior being claimed.<br>Physical claims remain PHYSICAL\_VALIDATION\_REQUIRED until measurements from actual hardware exist.<br>PROJECT-01 GATE-2 is the current execution focus; four blocker/decision closures define the near-term work. |
| --- |

| **Area** | **Previous handoff** | **Revised handoff** |
| --- | --- | --- |
| Starting point | Phase 0 skeleton creation | Existing workspace / existing PROJECT-01 PR; no re-creation |
| Research | Broad topic/repository research allowed | Only unresolved-gap verification; primary sources first |
| Simulation | Accepted as evidence when output exists | Requires model provenance + qualification + scope |
| Tooling | General reproducible scripts/tools | Tool trigger + escalation + stop conditions |
| Verification | Requirement / expected / actual / evidence | Adds evidence maturity, model status, tool, and physical-validation queue |
| Hardware truth | DESIGNED/SIMULATED/etc. status | Retained, plus strict separation from claim/evidence maturity |
| PROJECT-01 | Investigate broad design topics | Close GATE-2 blockers deterministically, then final review / merge-ready |
| Codex cost control | Implicit | Explicit delta-only reads, no redundant search, no unnecessary heavy-tool use |

# 1. OBJECTIVE

Create and maintain a professional engineering portfolio derived from public freelance job requirements. The portfolio must demonstrate traceable engineering work without implying that the original client projects were executed.

- PROJECT-01 — Solar-powered low-power embedded sensor board

- PROJECT-02 — USB CAN/CAN FD interface board

- PROJECT-03 — CAN-connected battery monitoring/interface system

- PROJECT-04 — PV + BESS + backup-generation electrical design package

Projects may share reusable engineering conventions, scripts, templates, and evidence schemas, but each project must remain independently understandable, testable, and releasable.

# 2. SOURCE-JOB SCOPE

| **ID** | **Portfolio line** | **Key source-job requirements retained as inspiration** |
| --- | --- | --- |
| JOB-01 | Solar low-power sensor | ESP32-WROOM-32E; solar + USB; single-cell Li-ion; low-Iq 3.3 V regulation; battery sensing; I2C sensor; deep sleep; KiCad 2-layer |
| JOB-02 | USB CAN/CAN FD interface | STM32H573RIT6; dual CAN/CAN FD; USB-C; USB/UART; SWD; protected voltage measurement; compact 2-layer target |
| JOB-03 | CAN battery monitor | Per-cell voltage; Hall current sensing; CAN/DroneCAN; AP\_Periph; health/status; parameters; firmware; calibration/test documentation |
| JOB-04 | PV + BESS design | Industrial/factory load; PV; inverter sizing/losses; AC-coupled BESS; variable load profile; single-line diagram; energy modeling |
| JOB-05 | Permit/documentation reference | PV; battery storage; generator; transfer switching; load calculations; SLDs; equipment schedules |
| JOB-06 | VESC/BMS reference | CAN; BMS; motor controller; USB-C; UART; I2C; wake/shutdown; wiring and bench-test documentation |

Source-job descriptions establish portfolio inspiration and requirement candidates only. Component choices, pin mappings, electrical compatibility, protocol assumptions, and safety claims must be independently verified.

# 3. NON-CLIENT REPRESENTATION RULE

| **Representation boundary**<br>Never claim client delivery, client approval, production deployment, certification, permit approval, manufacturing completion, commercial deployment, or field validation unless direct repository evidence exists.<br>A portfolio release is a documented engineering snapshot; it is not proof of production readiness. |
| --- |

# 4. WORKSPACE AND RESEARCH BOUNDARIES

The workspace keeps four different information layers separate:

| **Layer** | **Purpose** | **Codex rule** |
| --- | --- | --- |
| Research Records | Wide capture of external research, screenshots, papers, repositories, patents, product studies | Do not ingest by default; consult only when mapped to a current requirement/gap |
| Reference Lists | Curated candidates for primary/secondary evidence | Prefer primary manufacturer/specification sources; record provenance |
| Handoff | Current decisions, blockers, closure contracts, gates, tool policy, stop conditions | Treat as execution contract |
| Repository | Actual artifacts, scripts, logs, decisions, test outputs, PR history | Repository evidence outranks narrative claims |

Do not create meaningless placeholder directories. Any new directory must contain a useful artifact or a README that states its intended artifact, current status, and blocking condition.

## 4.1 REFERENCE PRODUCT BASELINE — CURATED BENCHMARK SET

The following products are the default external benchmarks for architecture, packaging, interface behavior, documentation quality, and validation planning. They are reference products, not automatic component selections. A reference product must not silently override an existing repository decision, active gate, or verified requirement; any resulting design change requires an explicit decision-register entry and primary-source evidence.

| **Project** | **Primary reference** | **Complementary reference** | **Codex use** |
| --- | --- | --- | --- |
| PROJECT-01 — Solar low-power sensor | Adafruit Universal USB/DC/Solar Charger — BQ24074 | DFRobot FireBeetle 2 ESP32-E | Power-path/solar/USB/Li-ion benchmark plus compact ESP32+battery+USB integration. Do not claim true MPPT. Existing MCP73871 work remains an active design decision until explicitly changed. |
| PROJECT-02 — USB CAN/CAN FD | Kvaser USBcan Pro 2xHS v2 CB | PEAK PCAN-USB Pro FD | Bare-board dual-CAN-FD packaging, galvanic isolation, connector/layout constraints, termination, host behavior, and professional interface expectations. |
| PROJECT-03 — CAN battery monitor | Isabellenhütte IVT-S | Orion BMS 2 | Shunt-based V/I/T-to-CAN telemetry, calibration/DBC behavior, fault/status modeling, and configurable diagnostics. Do not expand to a full BMS without explicit requirements. |
| PROJECT-04 — PV + BESS + backup generation | Sungrow PowerStack 255CS | Huawei LUNA2000-215 | C&I BESS equipment boundaries, PCS/BMS/EMS interfaces, operating modes, communications, protection annotations, equipment schedules, and SLD assumptions. |

Benchmark rule: use the primary product for the default benchmark and the complementary product only where it fills a documented gap. Manufacturer datasheets/manuals outrank reseller summaries. Stop once the named benchmark question is answered.

# 5. PROJECT QUEUE AND CURRENT FOCUS

| **Project** | **Primary near-term artifact** | **Gate** | **Current execution state** |
| --- | --- | --- | --- |
| PROJECT-01 — Solar-Powered Low-Power Sensor | I2C contract + power-state model + GATE-2 evidence closure | GATE-2 | ACTIVE — BLOCKED by explicit evidence/decision gaps |
| PROJECT-02 — USB CAN/CAN FD Interface | CAN specification matrix + interface contract | GATE-3 | QUEUED — do not overtake PROJECT-01 closure unless independently scheduled |
| PROJECT-03 — CAN Battery Monitoring Node | Measurement-to-telemetry contract | GATE-4 | QUEUED |
| PROJECT-04 — PV + BESS + Backup Generation | Operating-scenario matrix + reproducible sizing model | GATE-5 | QUEUED |

| **Current priority**<br>PROJECT-01 PR #1 remains the current closure context.<br>Do not broaden scope into later implementation work while GATE-2 is blocked.<br>When all GATE-2 closure contracts are satisfied, perform final review and mark MERGE\_READY. Merge only when explicitly authorized by the operator/workflow. |
| --- |

# 6. REQUIREMENTS, DECISIONS, RISKS AND EVIDENCE

Each project must preserve separate but linked records:

- requirements inventory — canonical IDs such as SOL-001…, CAN-001…, BAT-001…, PVB-001…

- decision register — DECISION-ID, problem, options, selected option, reason, evidence, consequences, status

- risk register — failure mode / impact / evidence / mitigation / verification / owner-status

- validation matrix — requirement → method → expected → actual → evidence path → status

- evidence ledger — source/model/tool/measurement provenance and scope

## 6.1 Requirement status vocabulary

Allowed requirement statuses: OPEN, IN\_PROGRESS, IMPLEMENTED, VERIFIED, BLOCKED, NOT\_APPLICABLE. VERIFIED requires an evidence path.

## 6.2 Hardware state vocabulary

Allowed hardware states: DESIGNED, SIMULATED, FABRICATED, ASSEMBLED, BENCH\_TESTED, FIELD\_TESTED. Never infer a higher state. DRC pass alone means DESIGNED, not BENCH\_TESTED.

# 7. CLAIM / EVIDENCE MATURITY MODEL

Claim maturity is separate from requirement status and hardware state. Codex must record the strongest evidence actually available, not the strongest status desired.

| **Evidence maturity** | **Meaning** | **Permitted claim** |
| --- | --- | --- |
| ASSUMED | Working value/behavior without sufficient external or measured proof | May be used only with explicit verification plan |
| SOURCE\_SUPPORTED | Primary or credible source supports the behavior/value | May constrain design; not yet proof of this design |
| CALCULATED | Reproducible analytical calculation from recorded inputs | Supports predicted design behavior within stated assumptions |
| DESIGN\_VERIFIED | Schematic/layout/configuration has been checked against requirements and source constraints | Supports implementation consistency, not physical performance |
| BENCH\_VERIFIED | Actual hardware measurement under defined setup/conditions | Supports that measured operating condition |
| FIELD\_VERIFIED | Observed under defined real-world operating conditions | Supports the tested field envelope only |

Simulation is not inserted as a universal rung in this ladder. Simulation evidence is tagged separately because its validity depends on model qualification and analysis scope.

# 8. SIMULATION MODEL QUALIFICATION

A simulation may support a design decision only when the model is qualified for the specific behavior being claimed. 'SPICE ran successfully' is not sufficient evidence.

| **Model state** | **Entry condition** | **What Codex may do** |
| --- | --- | --- |
| UNVERIFIED\_MODEL | Model file exists but origin/coverage is not proven | Exploratory use only; no PASS/VERIFIED claims |
| SOURCE\_VERIFIED | Model origin and exact component/revision are traceable | Run qualification tests; still no design claim from unsupported behavior |
| DATASHEET\_CORRELATED | Model reproduces relevant datasheet points/curves within declared acceptance criterion | Use inside the correlated operating envelope |
| BENCH\_CORRELATED | Model behavior has been compared with Rev-A measurements | Use for the correlated behavior with higher confidence |
| QUALIFIED\_FOR\_\[SCOPE\] | Source + correlation + operating envelope + exclusions are explicitly recorded | Use only for the named analysis scope |

## 8.1 Required model record

- component MPN and manufacturer

- model source URL/file and version/date

- datasheet revision used for correlation

- model type: SPICE/PSpice/IBIS/behavioral/other

- supported operating ranges and unsupported behaviors

- qualification testbench and acceptance criterion

- correlation outputs and evidence paths

- final qualification scope and exclusions

## 8.2 Model-coverage rule

If a behavior such as startup, shutdown, reverse leakage, thermal shutdown, OVP logic, protection disconnect, or temperature dependence is not demonstrated by the model, Codex must record MODEL\_OUT\_OF\_SCOPE for that behavior and must not use the simulation to close the requirement.

# 9. CHEAPEST-SUFFICIENT-TOOL POLICY

Codex must use the lightest reproducible method that can answer the engineering question. A heavier tool is an escalation, not a default.

| **Level** | **Tool/method** | **Use when** | **Escalate when** |
| --- | --- | --- | --- |
| T0 | Primary-source read + exact condition extraction | Question is semantic/specification-based | Source does not resolve operating condition or design dependency |
| T1 | Analytical calculation / Python script | RC, power budget, tolerance, scaling, energy balance, sizing | Result is sensitive to multiple parameters or nonlinear/transient behavior |
| T2 | Python sweep / corner / Monte Carlo | Tolerance and operating-envelope robustness matter | Dynamic/nonlinear device behavior dominates |
| T3 | KiCad ERC/DRC / build / static analysis / unit tests | Implementation consistency can be checked mechanically | Question depends on analog/transient/physical behavior |
| T4 | SPICE / EDA simulation with qualified model | Dynamic/nonlinear behavior is decision-critical and model coverage is adequate | Model is unqualified or physical parasitics dominate |
| T5 | Physical bench validation | Claim depends on real board, leakage, timing, startup, thermal, RF, EMI, component spread, or unmodeled parasitics | Field environment is the remaining uncertainty |
| T6 | Field validation | Real operating environment/mission profile must be proven | Not applicable; report tested envelope and limits |

## 9.1 Stop rule

Once the acceptance criterion for a gap is satisfied with the required evidence maturity, Codex must close the gap, record the evidence path, and STOP work on that gap. Do not continue researching for 'better' sources unless a conflict remains.

## 9.2 Heavy-tool trigger rule

Do not run SPICE, SI/PI, large sweeps, full repository scans, or repeated builds merely because they are available. Each heavy-tool run must name the unresolved failure mode or decision it is intended to reduce.

# 10. CODEX TOOL RESPONSIBILITY BOUNDARIES

Codex may directly own scriptable/CLI engineering work when the toolchain is available:

- KiCad CLI ERC/DRC, exports, BOM/manufacturing checks where applicable

- ngspice/SPICE testbenches and parameter sweeps using qualified models

- Python calculations, corner studies, tolerance sweeps, energy models and report generation

- firmware builds, unit tests, static analysis, mocks, CI and reproducible host-side tests

- Git diffs, evidence-path updates, decision-register changes and PR preparation

Codex may prepare but may not fabricate physical evidence for:

- oscilloscope/logic-analyzer captures

- actual deep-sleep or shutdown-current measurements

- bench power-path / charger / OVP behavior

- temperature/stress/fault-injection results

- EMI/EMC/RF behavior

- field operation

| **Unavailable toolchain rule**<br>If a required tool cannot be executed, create the exact command/testbench/procedure needed, mark execution BLOCKED or PHYSICAL\_VALIDATION\_REQUIRED, and stop.<br>Never fabricate logs, screenshots, measurements, or PASS states. |
| --- |

# 11. VALIDATION MATRIX — REVISED SCHEMA

| **Field** | **Required content** |
| --- | --- |
| Requirement ID | Canonical SOL/CAN/BAT/PVB/INT identifier |
| Claim / condition | Exact behavior/value being verified |
| Requirement status | OPEN / IN\_PROGRESS / IMPLEMENTED / VERIFIED / BLOCKED / NOT\_APPLICABLE |
| Evidence maturity | ASSUMED / SOURCE\_SUPPORTED / CALCULATED / DESIGN\_VERIFIED / BENCH\_VERIFIED / FIELD\_VERIFIED |
| Verification method | Inspection / calculation / ERC/DRC / build / unit test / simulation / bench / field |
| Tool | Exact script/tool/command/version where practical |
| Model status | N/A or model qualification state + scope |
| Expected result | Acceptance criterion |
| Actual result | Measured/calculated/tested result; blank if not executed |
| Evidence path | Repository-relative artifact/log/report |
| Limitations | Operating envelope, assumptions, exclusions |

# 12. PROJECT-01 — GATE-2 CLOSURE CONTRACT

The current PROJECT-01 task is not broad architecture discovery. It is the deterministic closure of the remaining GATE-2 evidence/decision gaps in the existing PR context.

| **Gap** | **Current issue** | **Cheapest sufficient next action** | **Closure criterion** | **Stop/output** |
| --- | --- | --- | --- | --- |
| GAP-02 — I2C electrical closure | Existing pull-up calculation is not final because bus-capacitance provenance/board model is incomplete. | Build a documented Cbus budget from MCU/sensor/input capacitance + trace/connector estimate; run reproducible Rpull-up/rise-time bounds. Escalate to SPICE only if non-ideal dynamics matter; Rev-A oscilloscope measurement remains the physical closure for actual rise time. | Source-supported inputs + reproducible worst-case calculation demonstrate compliant design margin, with physical rise-time validation queued for Rev-A. | Update I2C contract / calculation evidence; mark analytical gap closed; queue actual rise-time as a Rev-A bench validation item. |
| GAP-03 — MCP73871 battery-side protection | Protection-related datasheet statements do not yet prove the exact independent battery disconnect behavior required by the design assumption. | Use official MCP73871 documentation only; extract exact mode/condition and determine whether required disconnect semantics are actually provided. Do not use a generic charger model to infer protection behavior. | Primary source explicitly proves the required behavior, or the design assumption is rejected and an external protection decision is opened. | Update decision register and requirement status; stop once semantics are unambiguous. |
| GAP-08 — no-input / shutdown current semantics | Recorded current figures are not yet proven to refer to the same operating state/measurement condition. | Reconcile exact datasheet parameter names, conditions and paths; create a leakage/current budget using only like-for-like states. | Every included current term has a primary-source condition and the selected system state is internally consistent. | Update power-state/current budget; stop after semantic conflict is resolved. |
| DEC-11 — input OVP selection | Functional need is known, but component selection is not yet justified by complete threshold/disconnect/Iq/leakage/hot-plug evidence. | Compare only primary-source candidates against explicit criteria; use calculation first. Run transient simulation only if decision depends on dynamic response and a qualified manufacturer model exists. | Selected option satisfies threshold, true disconnect behavior, current/leakage budget, abs-max/hot-plug requirements, or remains BLOCKED with exact missing evidence. | Update decision register + component selection; no speculative selection. |

## 12.1 PROJECT-01 GATE-2 gate logic

**GAP-02 analytical closure<br>+ GAP-03 protection-semantics closure<br>+ GAP-08 no-input/shutdown-current closure<br>+ DEC-11 OVP decision closure<br>→ GATE-2 RE-EVALUATION<br>→ if PASS: FINAL REVIEW<br>→ MERGE\_READY<br>→ merge only when authorized<br>→ next branch/PR for post-GATE-2 implementation work**

## 12.2 PROJECT-01 physical validation queue

The following items must not be silently upgraded to BENCH\_VERIFIED before actual Rev-A measurements:

- I2C SDA/SCL rise time and logic levels on the assembled PCB

- deep-sleep / shutdown / battery-only current under defined firmware state

- power-rail startup, shutdown and source-transition behavior

- charger/power-path behavior across battery and solar/USB conditions

- input OVP/fault-disconnect behavior if required by the final architecture

- sensor communication under low-power state transitions

- variable-input / weak-solar behavior and brownout/recovery

- thermal behavior if power dissipation or charge-current conditions make it relevant

# 13. PROJECT-02 / 03 / 04 EXECUTION RULES

Reference baseline for queued work: PROJECT-02 → Kvaser USBcan Pro 2xHS v2 CB + PEAK PCAN-USB Pro FD; PROJECT-03 → Isabellenhütte IVT-S + Orion BMS 2; PROJECT-04 → Sungrow PowerStack 255CS + Huawei LUNA2000-215. These references constrain benchmark questions only; they do not constitute design approval or verified implementation evidence.

The same evidence/tool policy applies to the remaining projects. However, do not spend Codex limit on broad implementation while their gate prerequisites are not the active queue.

| **Project** | **Pre-implementation contract** | **Likely tool path** | **Physical/field boundary** |
| --- | --- | --- | --- |
| PROJECT-02 — USB CAN/CAN FD | CAN/FDCAN/USB/power/interface specification matrix and pin/electrical compatibility | Primary docs → scripted checks → KiCad ERC/DRC → firmware build/tests → qualified simulation only for specific transients | CAN physical-layer captures, USB/CAN timing, protection behavior require bench evidence |
| PROJECT-03 — CAN battery monitor | Measurement chain, calibration, filtering, plausibility and CAN telemetry contract | Source extraction → error budget / Python → firmware tests → ADC/AFE simulation if justified | Calibration, sensor error, noise, current/voltage measurement require bench correlation |
| PROJECT-04 — PV+BESS | Operating scenarios, load model, assumptions, sizing constraints and protection boundaries | Reproducible Python/energy model → scenario/corner analysis → electrical design checks | Site-specific/permit/utility/field claims remain outside evidence unless actual project data exists |

# 14. RESEARCH POLICY — DELTA ONLY

Product-benchmark rule: consult the curated baseline in §4.1 before searching for additional products. Add a new benchmark product only when the primary/complementary pair cannot answer a named requirement or decision gap.

Existing research records are a knowledge base, not an automatic Codex input. Before any web/GitHub research, Codex must check whether the needed evidence already exists in the repository, reference list, or recorded decision/evidence ledger.

- Prefer manufacturer datasheets, reference manuals, application notes, protocol specifications and official project documentation.

- Use established open-source implementations for implementation patterns and compatibility clues, not as substitutes for primary electrical specifications.

- Use papers/patents/product studies for architecture, terminology and benchmark context unless they directly satisfy a requirement.

- Record title, organization/author, URL/source, access date, exact use, affected requirement, and evidence class.

- Do not silently copy reference designs.

## 14.1 Research stop conditions

- Stop when the exact closure criterion is satisfied.

- Stop and report CONFLICT if two authoritative sources disagree and the conflict changes the design.

- Stop and report PHYSICAL\_VALIDATION\_REQUIRED when the remaining uncertainty is board-/assembly-/measurement-dependent.

- Do not restart broad repository discovery after a gap has already been narrowed to a specific datasheet condition.

# 15. LIMIT-CONSCIOUS CODEX EXECUTION

| **Default execution pattern**<br>Read only the current handoff + files directly named by the active gap.<br>Reuse existing calculations, sources, scripts, and prior passing outputs unless affected by a change.<br>Do one closure contract at a time; update evidence immediately.<br>Prefer one consolidated scripted run over many interactive micro-steps.<br>Run heavyweight tools only when their trigger condition is met.<br>Re-run only checks invalidated by the current change.<br>After acceptance: record, close, stop. |
| --- |

| **Avoid** | **Use instead** |
| --- | --- |
| Re-reading the entire research archive | Requirement/gap-linked evidence lookup |
| General web/GitHub exploration | Primary-source unresolved-gap verification |
| Running all simulations 'for confidence' | Failure-mode-specific simulation with qualified model |
| Repeated full builds/tests after unrelated doc edits | Affected-check selection |
| Producing placeholder files as progress | Only artifacts required by an active closure contract |
| Declaring PASS from narrative | Machine-readable evidence path + acceptance criterion |

# 16. EVIDENCE FILE / OUTPUT CONVENTION

Where practical, use repository-relative evidence artifacts similar to:

- shared/scripts/ — reusable calculations/checkers

- shared/references/ — source inventory and provenance

- shared/validation/ — shared schemas and validation conventions

- <project>/02\_requirements/ — requirement inventory

- <project>/03\_architecture/ — interface/power/state contracts

- <project>/06\_simulation/ — model provenance, qualification testbenches, outputs

- <project>/07\_validation/ — validation\_matrix.csv, validation\_summary.md, test plans and evidence links

- <project>/08\_outputs/ — release-facing outputs only after gate/review

Exact repository paths may differ from the current implementation. Do not move working files merely to match this handoff; preserve existing valid structure and map equivalent locations.

# 17. MODEL / CALCULATION REPRODUCIBILITY RULE

- Inputs must be machine-readable or explicitly recorded.

- Numerical constants must identify SOURCE\_SUPPORTED / DECISION\_SUPPORTED / ASSUMED provenance.

- Scripts must emit enough context to reproduce the result: tool version where relevant, input set, command, output, acceptance criterion.

- A plot without underlying inputs/results is supporting visualization, not sufficient evidence.

- A manufacturer model must not be edited silently. Any wrapper/parameter override must be recorded.

- Model correlation criteria are requirement-specific; do not apply an arbitrary universal percentage.

# 18. PHYSICAL VALIDATION HANDOFF LOOP

Codex cannot perform physical measurement itself, but it can prepare a deterministic lab procedure and ingest the resulting measurement as evidence.

**Design/requirement<br>→ analytical check<br>→ optional qualified simulation<br>→ Rev-A fabrication/assembly<br>→ lab procedure generated by Codex<br>→ operator measurement<br>→ evidence file imported<br>→ Codex compares expected vs actual<br>→ issue/decision update<br>→ Rev-B if required**

## 18.1 Minimum physical-test record

- hardware revision / serial identifier

- firmware version/commit and configuration

- test equipment and relevant settings

- wiring/setup description

- environment/operating state

- expected result and acceptance criterion

- actual measurement

- raw evidence path (capture/log/photo/data)

- result status and follow-up

# 19. GITHUB / PR RULES

- Do not mix unrelated future implementation into a PR whose purpose is evidence/gate closure.

- Each PR must state affected requirements/decisions/gates.

- Do not merge merely because the diff is clean; gate status and evidence must support MERGE\_READY.

- After a gate-closing PR is merged, continue subsequent implementation in a new branch/PR unless the current PR scope explicitly includes it.

- Do not create repeated 'continue' commits that only restate unresolved blockers.

# 20. RISK POLICY

At minimum, preserve explicit risks for:

- electrical and battery safety

- CAN termination/ground/reference errors

- component availability and model availability

- PCB manufacturability and unmodeled parasitics

- unvalidated high-current/transient behavior

- ADC scaling/calibration and measurement plausibility

- thermal behavior

- incorrect energy/load assumptions

- BESS/PV sizing assumptions

- regulatory/permit limitations

- unsupported performance claims

- simulation-model misuse outside qualified scope

# 21. RELEASE OUTPUTS

Each releasable project snapshot should converge toward:

- README.md — demonstrated competencies, reproduction steps, evidence and limits

- release\_notes.md

- architecture\_summary.md

- validation\_summary.md

- demo\_manifest.md

- checksums.txt where appropriate

A release is a portfolio snapshot, not proof of production readiness.

# 22. DEFINITION OF DONE

A project may be called portfolio-ready only when:

- requirements are traceable

- architecture/interfaces are documented

- calculations/build steps are reproducible

- decision and risk registers reflect current design state

- validation status is explicit

- every VERIFIED claim has an evidence path

- simulation claims identify qualified model scope

- physical claims are limited to actual BENCH\_VERIFIED/FIELD\_VERIFIED evidence

- limitations and unresolved assumptions are disclosed

- README can be understood independently

If any required condition is missing, keep the project OPEN, IN\_PROGRESS, or BLOCKED as appropriate.

# 23. FIRST CODEX ACTION — REVISED

PROJECT-01 benchmark context: Adafruit BQ24074 is the selected external power-path reference and DFRobot FireBeetle 2 ESP32-E is the selected integration reference. They do not automatically replace MCP73871 or any existing repository component. If benchmark evidence suggests a change, open or update a decision record; do not silently alter the active GATE-2 closure contract.

| **Do not recreate Phase 0. Do not restart broad research.**<br>1) Load the current PROJECT-01 requirement inventory, decision register, PR #1 diff/context, GATE-2 status, and evidence files linked to GAP-02, GAP-03, GAP-08 and DEC-11.<br>2) Reuse existing sources/calculations before requesting any new source.<br>3) Resolve primary-source semantic closures first (default cheap-first order: GAP-08 → GAP-03), then analytical/design closures (GAP-02 → DEC-11). Reorder only if a documented dependency requires it.<br>4) For each gap, use the cheapest-sufficient-tool policy and write evidence immediately to the repository.<br>5) Never use an unqualified simulation model to close a requirement.<br>6) If the remaining uncertainty requires real hardware, mark PHYSICAL\_VALIDATION\_REQUIRED and prepare the bench procedure instead of fabricating a PASS.<br>7) When all four GATE-2 closure contracts are satisfied, re-evaluate GATE-2 once, run the affected final checks, and report MERGE\_READY or the exact remaining blocker.<br>8) STOP. Do not automatically begin the next implementation PR unless separately instructed. |
| --- |

# 24. CODEX COMPLETION REPORT FORMAT

At the end of an execution batch, return a compact status block:

ACTIVE\_GATE: GATE-2<br>GAPS\_CLOSED: \[IDs\]<br>GAPS\_OPEN: \[IDs + exact missing evidence\]<br>TOOLS\_RUN: \[tool / command / purpose\]<br>MODELS\_USED: \[status + qualification scope\]<br>FILES\_CHANGED: \[repo-relative paths\]<br>EVIDENCE\_ADDED: \[repo-relative paths\]<br>PHYSICAL\_VALIDATION\_QUEUE: \[tests\]<br>GATE\_RESULT: PASS | BLOCKED | NOT\_REEVALUATED<br>NEXT\_ACTION: one smallest sufficient action only

Core rules retained: no unsupported claims; no invented measurements/approvals; preserve traceability and strict design/simulation/fabrication/bench/field boundaries.
