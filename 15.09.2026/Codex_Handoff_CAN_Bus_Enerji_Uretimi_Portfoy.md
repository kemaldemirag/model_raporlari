# CODEX HANDOFF<br>CAN Bus + Energy Generation Portfolio Workspace

Handoff ID: CAN-ENERGY-PORTFOLIO-2026-09-15<br>Workspace type: Job-derived engineering portfolio<br>Status: REVISED / EVIDENCE REQUIRED / RESEARCH DELTA INTEGRATED

## 1. OBJECTIVE

Create a professional engineering portfolio workspace derived from real freelance job requirements. Convert public job requirements into independently reviewable engineering projects without pretending that the original client projects were executed.

- Solar-powered low-power embedded sensor board

- USB CAN/CAN FD interface board

- CAN-connected battery monitoring/interface system

- PV + BESS + backup-generation electrical design package

Projects may share reusable engineering conventions, but each project must remain independently understandable, testable, and releasable.

## 2. SOURCE JOBS

### JOB-01 — Solar-Powered Low-Power Sensor PCB

Upwork — KiCad Schematic + PCB Layout — Low-Power ESP32 Solar/Li-Ion Sensor

Reference URL: https://www.upwork.com/freelance-jobs/apply/KiCad-Schematic-PCB-Layout-Low-Power-ESP32-Solar-Ion-Sensor\_~022098110512333639161/

- ESP32-WROOM-32E

- solar + USB input

- single-cell Li-ion charging

- 3.3 V low-Iq regulation

- battery-voltage sensing

- I2C sensor

- deep-sleep optimization

- KiCad 2-layer PCB

### JOB-02 — USB CAN/CAN FD Interface

Freelancer — USB CAN ADAPTER KICAD

Reference URL: https://www.freelancer.com/projects/stm32/usb-can-adapter-kicad

- STM32H573RIT6

- dual independent CAN/CAN FD

- USB-C

- USB/UART

- SWD

- protected voltage measurement

- KiCad

- ~80 × 50 mm, 2-layer target

### JOB-03 — CAN Battery Monitor

Freelancer — AP\_Periph CAN Battery Monitor

Reference URL: https://www.freelancer.com/projects/embedded-systems/periph-can-battery-monitor

- per-cell voltage sensing

- Hall-effect current measurement

- CAN / DroneCAN

- AP\_Periph

- health/status reporting

- parameters

- firmware

- calibration and test documentation

### JOB-04 — PV + BESS Electrical Design

Upwork — Solar PV + BESS Electrical Design Consultant

Reference URL: https://www.upwork.com/freelance-jobs/apply/Solar-BESS-Electrical-Design-Consultant\_~022097611188411235823/

- ~750 kWp reference scale

- factory load

- ground-mounted PV

- inverter sizing

- losses

- AC-coupled BESS

- variable load profile

- single-line diagram

- energy modeling

### JOB-05 — Permit / Electrical Documentation Reference

Upwork — Permit Plan Set Drafter — Solar, EV Charger, Battery Storage, Generator & HVAC

Reference URL: https://www.upwork.com/freelance-jobs/apply/Permit-Plan-Set-Drafter-Solar-Charger-Battery-Storage-Generator-HVAC-Ongoing-Work\_~022097158426066722716/

- PV documentation

- battery storage

- backup generator

- transfer switching

- load calculations

- single-line diagrams

- equipment schedules

### JOB-06 — VESC/BMS Interface Reference

Upwork — PCB Design

Reference URL: https://www.upwork.com/freelance-jobs/apply/PCB-Design\_~022096947195546776628/

- CAN

- BMS

- motor controller

- USB-C

- UART

- I2C

- wake/shutdown behavior

- wiring and bench-test documentation

Source-job details are requirements inspiration, not evidence of client execution. Component choices, pin mappings, electrical compatibility, protocol assumptions, and safety requirements must be independently verified.

## 3. NON-CLIENT REPRESENTATION RULE

These are portfolio engineering studies inspired by publicly described freelance requirements.

Never claim client delivery, client approval, production deployment, certification, permit approval, field validation, manufacturing completion, or commercial deployment unless direct evidence exists in the repository.

## 4. WORKSPACE STRUCTURE

can\_energy\_portfolio/<br>├── README.md<br>├── LICENSE<br>├── .gitignore<br>├── docs/{job\_sources,portfolio\_map,engineering\_rules,evidence\_policy,terminology}.md<br>├── 01\_solar\_low\_power\_sensor/{01\_job\_context,02\_requirements,03\_architecture,04\_hardware,05\_firmware,06\_simulation,07\_validation,08\_outputs}/<br>├── 02\_usb\_can\_fd\_interface/{01\_job\_context,02\_requirements,03\_architecture,04\_hardware,05\_firmware,06\_host\_tools,07\_validation,08\_outputs}/<br>├── 03\_can\_battery\_monitor/{01\_job\_context,02\_requirements,03\_architecture,04\_hardware,05\_firmware,06\_protocol,07\_validation,08\_outputs}/<br>├── 04\_pv\_bess\_system/{01\_job\_context,02\_requirements,03\_load\_model,04\_pv\_model,05\_bess\_model,06\_electrical\_design,07\_simulation,08\_validation,09\_outputs}/<br>├── shared/{templates,scripts,references,validation}/<br>└── releases/

Do not create meaningless placeholder directories. Every created directory must contain useful content or a README explaining intended artifacts and current status.

## 5. PROJECT-01 — Solar-Powered Low-Power Sensor

Goal: Design a credible solar-powered low-power embedded sensor node.

Candidate architecture

SOLAR PANEL → INPUT PROTECTION → SOLAR/Li-Ion CHARGER ↔ BATTERY → 3.3 V REGULATOR → ESP32 ↔ I2C SENSOR + MEASUREMENT CIRCUITS

Investigate / verify

- solar charger architecture

- battery protection assumptions

- USB/solar source interaction

- regulator quiescent current and board-level sleep-current budget

- ESP32 sleep/wake modes and wake sources

- voltage-divider leakage and ADC accuracy

- I2C voltage domain, pull-up strategy, address inventory, discovery, initialization, ACK/NACK and missing-device behavior

- duplicate-address handling where applicable

- sensor power gating, stabilization delay, wake/re-initialization behavior

- reverse-current paths

- battery-temperature protection

- PCB grounding and analog/digital separation

- test points

Required behavior scenarios

- sleep → wake → sensor power-on → stabilization → I2C init/discovery → measurement → telemetry → peripheral shutdown → sleep

- missing I2C device / NACK

- sensor power-cycle followed by bus re-initialization

- duplicate I2C address if the selected device topology can create one

- energy-surplus, energy-neutral, and energy-deficit operating assumptions; numeric thresholds remain OPEN until sourced

Required artifacts

- requirements.md

- block\_diagram.md

- power\_budget.md

- component\_selection.md

- schematic\_review.md

- pcb\_design\_notes.md

- sleep\_current\_budget.md

- i2c\_interface\_contract.md

- power\_state\_model.md

- test\_plan.md

- risk\_register.md

## 6. PROJECT-02 — USB CAN/CAN FD Interface

Goal: Create a compact STM32-based dual-channel CAN/CAN FD engineering interface.

Candidate architecture

USB-C + SWD → STM32 → FDCAN A/B → TRANSCEIVER A/B → CAN-A/CAN-B

Investigate / verify

- STM32H573 peripheral capabilities and FDCAN pin mappings

- USB device implementation and USB-C CC implementation

- transceiver voltage compatibility and physical-layer data-rate limit

- nominal bitrate × data bitrate candidate profiles; numeric profiles remain candidates until supported by selected transceiver/topology

- selectable 120 Ω termination and termination-state test cases

- ESD protection and common-mode behavior

- connector strategy

- galvanic isolation tradeoff

- protected voltage measurement

- disconnected-channel, no/one/two-termination, bus-off and recovery behavior

Scenario rule

- Do not promote example CAN FD rates from research to requirements without MCU/transceiver/topology evidence.

- Keep FPGA-CAN and other controller implementations as SUPPORTING references unless the selected STM32 architecture changes through the decision register.

Required artifacts

- requirements.md

- architecture.md

- interface\_control\_document.md

- can\_message\_model.md

- KiCad source or schematic.md

- pcb\_notes.md

- firmware\_architecture.md

- bringup\_plan.md

- can\_specification\_matrix.md

- test\_vectors.md

- validation\_report.md

## 7. PROJECT-03 — CAN Battery Monitoring Node

Goal: Connect battery measurements to a CAN-based telemetry architecture.

Candidate architecture

CELL VOLTAGES + CURRENT SENSOR → ADC/AFE → MCU → filtering/calibration/plausibility → CAN/DroneCAN → TELEMETRY NODE

Investigate / verify

- cell measurement AFE/ADC architecture

- isolation requirements

- Hall current-sensor architecture and current polarity

- ADC resolution, scaling, error budget and filtering

- calibration points, offset/gain handling and reproducibility

- plausibility and stale/impossible-measurement checks

- over/under-voltage, current and temperature fault/status reporting

- CAN message structure and measurement-to-telemetry field mapping

- DroneCAN feasibility and BatteryInfo/cell-voltage semantics where applicable

- AP\_Periph architecture and parameter storage

- fault/status model

Research narrowing

- Do not use generic “battery monitor” search results as evidence. Search by cell AFE, Hall current sensor, ADC/calibration, fault/status, DroneCAN and AP\_Periph concepts.

- Candidate components and numeric ranges remain OPEN until source-job constraints and primary technical references support selection.

Required artifacts

- measurement\_requirements.md

- measurement\_architecture.md

- calibration\_strategy.md

- measurement\_to\_telemetry\_contract.md

- can\_protocol\_map.md

- fault\_model.md

- firmware\_architecture.md

- simulation\_plan.md

- bench\_test\_plan.md

- validation\_matrix.md

## 8. PROJECT-04 — PV + BESS + Backup Generation Design

Goal: Produce a documented reference electrical design for an industrial load supplied by PV, battery storage, utility grid, and optional backup generation.

Candidate architecture

PV ARRAY → PV INVERTER ┐<br>GRID ──────────────────┼→ AC BUS → FACTORY LOAD<br>BESS ↔ BESS PCS ───────┤<br>GENERATOR → ATS ───────┘

Investigate / verify

- load profile, peak demand and daily energy

- PV production, inverter sizing, DC/AC ratio and losses

- BESS power \[kW\], energy \[kWh\], duration \[h\], SOC operating window and efficiency

- dispatch strategy: peak shaving, self-consumption and reserve behavior

- generator role and start/stop assumptions

- ATS operating logic and transfer-mode assumptions

- normal and abnormal operating modes

- basic protection architecture

- single-line diagram

Minimum operating scenarios

- normal day

- PV surplus

- evening/production peak

- grid outage

- grid outage with low BESS SOC

- BESS unavailable

- generator unavailable or failed start

- ATS transfer and grid-return event

- Treat 1 h / 2 h / 4 h or other duration values only as sensitivity candidates until the load model and design objective justify selection.

Required artifacts

- assumptions.md

- load\_profile.csv

- load\_analysis.md

- pv\_sizing.md

- bess\_sizing.md

- generator\_strategy.md

- operating\_modes.md

- operating\_scenario\_matrix.md

- single\_line\_diagram.md

- equipment\_schedule.md

- calculation\_methodology.md

- simulation\_results.md

- limitations.md

## 9. CROSS-PROJECT INTEGRATION

PV / ENERGY SYSTEM → BATTERY / STORAGE → BATTERY MONITOR → CAN → CAN INTERFACE → USB → PC / LOGGER

This is an optional integration target, not an initial requirement. Do not force integration before individual subsystems have testable interfaces.

## 10. REQUIREMENTS SYSTEM

Create canonical requirement IDs: SOL-001…, CAN-001…, BAT-001…, PVB-001…, INT-001…. Each requirement must contain ID, title, description, source, rationale, verification method, status, and evidence.

Allowed status values: OPEN, IN\_PROGRESS, IMPLEMENTED, VERIFIED, BLOCKED, NOT\_APPLICABLE. VERIFIED requires evidence.

Specification Scenario Matrix

Each project shall maintain a compact scenario matrix with: Requirement ID | Specification axis | Candidate scenario | Source/reference | Decision status | Verification method | Evidence path.

Candidate scenario values are not requirements. Promote a value only after a decision is supported by source constraints and technical evidence; otherwise keep it OPEN or explicitly ASSUMED.

## 11. EVIDENCE POLICY

No unsupported claims. VERIFIED, VALIDATED, TESTED, PASS, COMPLETE, PRODUCTION READY, and MANUFACTURING READY require evidence.

- automated test logs

- simulation output

- KiCad ERC/DRC reports

- compiler output

- unit tests

- CAN traffic logs

- oscilloscope captures

- current measurements

- generated manufacturing files

- calculation scripts and outputs

A document merely stating that something passed is not evidence by itself.

## 12. HARDWARE STATUS CLASSIFICATION

Use only: DESIGNED, SIMULATED, FABRICATED, ASSEMBLED, BENCH\_TESTED, FIELD\_TESTED. Never infer a higher state. A PCB that only passes DRC is DESIGNED, not BENCH\_TESTED.

## 13. VALIDATION MATRIX

Every project must contain: requirement, test, expected result, actual result, evidence path, status.

- validation\_matrix.csv — machine-readable where practical

- validation\_summary.md — human-readable summary

## 14. RESEARCH POLICY

Prefer manufacturer datasheets, reference manuals, application notes, protocol specifications, official project documentation, established open-source implementations, engineering papers, then community material as supporting evidence.

For every external reference record title, organization/author, URL, access date, use, affected requirement, evidence strength, and action.

Classify each research/reference item as RELEVANT, SUPPORTING, NEEDS\_VERIFICATION, or OUT\_OF\_SCOPE. Presence in Araştırma Kayıtları or Referans Listeler does not by itself make an item a Handoff input.

A reference may enter Codex execution context only if it changes or supports a requirement, decision, verification method, test scenario, or evidence expectation. Do not silently copy reference designs.

Research compression rule: raw records → requirement mapping → specification axis → candidate scenario → decision/evidence gap → Codex action.

## 15. GITHUB RESEARCH

Do not repeat broad repository discovery already captured in Araştırma Kayıtları / Referans Listeler. Codex GitHub work is limited to unresolved-gap verification.

Priority unresolved searches: PROJECT-01 primary ESP32/I2C/low-power implementation evidence; PROJECT-02 STM32H573 FDCAN/transceiver/USB-C/termination evidence; PROJECT-03 cell AFE/Hall sensor/calibration/DroneCAN/AP\_Periph evidence; PROJECT-04 reproducible PV/BESS/ATS modeling references.

Store only screened findings in shared/references/github\_research.md. Record URL, relevant paths/concepts, license, reuse limits, affected requirement, classification, and exact unresolved question answered.

Do not vendor entire repositories merely because they are relevant. FPGA, robotics/navigation, Firebase/cloud identity, and unrelated PWM results remain OUT\_OF\_SCOPE unless a canonical requirement or approved decision explicitly brings them into scope.

## 16. DECISION REGISTER

Each project must contain decision\_register.md with DECISION-ID, problem, options, selected option, reason, evidence, consequences, and status. Do not silently make major architectural decisions.

## 17. RISK REGISTER

- electrical and battery safety

- CAN termination and grounding/reference errors

- component availability

- PCB manufacturability

- unvalidated high-current behavior

- incorrect ADC scaling

- thermal behavior

- incorrect energy assumptions

- BESS sizing assumptions

- regulatory/permit limitations

- unsupported performance claims

## 18. CODEX EXECUTION ORDER

Phase 0 — Create workspace skeleton, root README, evidence policy, job-source map, requirement templates, reference-classification template, and specification-scenario-matrix template.

Phase 0A — For PROJECT-01 and PROJECT-02, map known research to requirements and create OPEN decision/scenario matrices. Do not perform broad web/GitHub discovery.

Phase 0B — Specification/scenario gate: before implementation, every candidate numeric value must be either source-supported, decision-supported, or explicitly OPEN/ASSUMED with a verification plan.

Phase 1 — Baseline PROJECT-01 and PROJECT-02 first; they are the most bounded portfolio builds. PROJECT-01 should convert the current I2C/low-power research into interface and power-state contracts.

Phase 2 — Create PROJECT-03 after measurement/protocol interfaces are explicit; first narrow research to measurement-to-telemetry evidence.

Phase 3 — Create PROJECT-04 as a reproducible calculation/simulation package driven by the operating-scenario matrix with all assumptions exposed.

Phase 4 — Run cross-project consistency checks and only then evaluate optional integration.

Phase 5 — Generate release packages only for projects with traceable evidence.

## 19. CODEX OPERATING RULES

- Do not invent client files, measurements, test results, certifications, approvals, or hardware access.

- Do not mark requirements VERIFIED without evidence paths.

- Do not replace engineering uncertainty with guessed values; record assumptions and OPEN items.

- Prefer scripts and reproducible calculations over manually typed final numbers.

- Keep source-job requirements traceable to derived portfolio requirements.

- Preserve clear boundaries between design, simulation, fabrication, bench validation, and field validation.

- When a toolchain is unavailable, create the design/test plan and mark execution BLOCKED rather than fabricating output.

- Document major architecture changes in decision registers.

## 20. RELEASE OUTPUTS

Each project output/release directory should converge toward:

- README.md — summary, demonstrated competencies, how to reproduce, evidence, limits

- release\_notes.md

- architecture\_summary.md

- validation\_summary.md

- demo\_manifest.md

- checksums.txt

A release is a portfolio snapshot, not proof of production readiness.

## 21. DEFINITION OF DONE

A project may be called portfolio-ready only when requirements are traceable, architecture is documented, calculations/build steps are reproducible, validation status is explicit, evidence paths exist for every VERIFIED claim, limitations are disclosed, and the README can be understood independently.

If any of those conditions are missing, keep the project status OPEN, IN\_PROGRESS, or BLOCKED as appropriate.

## 22. FIRST CODEX ACTION

Start by creating the workspace skeleton and Phase-0 governance files. Then create requirement inventories, OPEN decision registers, and specification-scenario matrices for PROJECT-01 and PROJECT-02. For PROJECT-01, convert the existing I2C/low-power research into an I2C interface contract and power-state model before PCB implementation. For PROJECT-02, create the CAN physical-layer/data-rate/termination scenario matrix before selecting numeric profiles. Do not repeat broad GitHub research; verify only unresolved gaps. Do not jump directly to PCB layout or implementation before requirements, assumptions, interfaces, scenarios, and evidence expectations are explicit.
