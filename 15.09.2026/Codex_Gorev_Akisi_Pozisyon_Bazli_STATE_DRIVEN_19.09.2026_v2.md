**CAN Bus + Energy Generation Portfolio**

**Codex Görev Akışı<br>Pozisyon Bazlı Rapor**

**STATE-DRIVEN / SCENARIO × ECAD × DECISION**<br>19.09.2026

| **Alan** | **Değer** |
| --- | --- |
| Handoff ID | CAN-ENERGY-PORTFOLIO-2026-09-15 |
| Kaynak baseline | CAN\_Bus\_Enerji\_Uretimi\_Is\_Akisi\_Raporu\_STATE\_DRIVEN\_FINAL\_19.09.2026 |
| Çekirdek model | Product × Scenario × ECAD/Engineering Module × Decision × Evidence Dependency |
| Yürütme amacı | Yetkilendirilmiş bir sonraki mühendislik hücresini kapatmak; kapanmış araştırmayı yeniden açmamak; mimari sürüklenmeyi önlemek. |
| State authority | Current repo/decision register/active gate > current closure evidence > synthesis docs > raw research records |
| Pozisyon sayısı | 4 |

| **Bu rapor dört pozisyonu korur. Phase/rol çoğaltımı yapılmaz; ortak state-driven protokol pozisyonların üstünde çalışan yürütme kuralıdır.** |
| --- |

# 1. State-Driven Codex Yürütme Sözleşmesi

Bu sürümde Codex bütün portföyü yeniden planlamaz. Önkoşulları sağlanmış ilk OPEN/BLOCKED hücreyi seçer, yalnız o hücrenin closure criterion'ını karşılayacak minimum mühendislik eylemini uygular, etkilenen bağımlılıkları günceller ve durur.

## 1.1 Ortak yürütme akışı

| LOAD current repo + decision register + active gate + current branch/PR evidence<br>↓<br>SELECT first prerequisite-satisfied OPEN/BLOCKED cell<br>↓<br>AUTHORITY = TYPE-A VERIFY \| TYPE-B SELECT \| TYPE-C REOPEN<br>↓<br>REUSE current evidence / respect RESEARCH\_CLOSED<br>↓<br>EXECUTE minimum sufficient engineering action<br>↓<br>CHECK named closure criterion<br>↓<br>UPDATE state + decision/register/gate + evidence path<br>↓<br>INVALIDATE only affected dependencies<br>↓<br>MARK non-blocking findings NEW-GAP/DEFERRED<br>↓<br>IDENTIFY next queue item<br>↓<br>STOP |
| --- |

| **State** | **Operasyonel anlam** |
| --- | --- |
| SOURCE\_SUPPORTED | Kaynak, iddiayı belirtilen koşullarda destekler. |
| CALCULATED | Kayıtlı girdilerden yeniden üretilebilir hesap. |
| RESEARCH\_CLOSED | Gerekli araştırma yeterli; discovery tekrarlanmaz. |
| DECIDED(candidate) | Aday seçildi; downstream sözleşme/kanıt eksik olabilir. |
| PROVISIONAL | Adlandırılmış validasyon/girdi ile çalışan karar. |
| FROZEN | Yeniden discovery olmadan implementation tarafından tüketilebilir sözleşme. |
| VALIDATION\_REQUIRED | Araştırma/karar kapalı olabilir; doğrulama beklenir. |
| DESIGN\_INPUT\_REQUIRED | Gerçek kullanıcı/site/product girdisi gerekir; uydurulamaz. |
| BLOCKED | Önkoşul sonraki yetkili adımı engeller. |
| HISTORICAL\_INVALID\_FOR\_CURRENT\_BOM | Tarihsel olarak doğru fakat güncel BOM/topoloji için geçersiz kanıt. |

| **Authority** | **İzin verilen eylem** |
| --- | --- |
| TYPE-A VERIFY | Mevcut baseline'ı doğrula. |
| TYPE-B SELECT | Belirtilmiş kriterlerle sınırlı adaylar arasından seç. |
| TYPE-C REOPEN | Baseline invalidate olmuştur; yalnız minimum replacement class araştırmasını aç. |

# 2. Ortak Invalidation ve Araştırma Kontrolü

- Değiştirilen bir komponentin sayısal özelliği yeni komponente taşınamaz.

- Part change sonrası eski part-specific evidence HISTORICAL\_INVALID\_FOR\_CURRENT\_BOM olur; yalnız replacement evidence açılır; bağımlı hesaplar STALE olur.

- Topology change yalnız doğrudan etkilenen ECAD modüllerini ve downstream arayüz/hesapları yeniden açar.

- Missing deployment input, daha fazla generic web research gerekçesi değildir.

- Yeni araştırma yalnız named requirement, decision, evidence gap veya failed calculation kapatıyorsa açılır.

- Gerçek seçim açık değilse alternatif listesi büyütülmez; 2–4 aday sınırı korunur.

- Rejected alternatives kaydedilir; sonraki ajanlar aynı araştırmayı tekrar etmez.

| **RESEARCH\_CLOSED veya DECIDED(candidate) bir konu yalnız authoritative contradiction, component-limit violation, failed calculation veya implementation evidence ile invalidate olursa yeniden açılır.** |
| --- |

## 2.1 “DEVAM” komutunun operasyonel anlamı

1. Önkoşulları sağlanmış ilk kuyruk maddesini otomatik seç.

2. Yalnız DESIGN\_INPUT\_REQUIRED, gerçek kullanıcı tercih trade-off'u veya write/merge/push yetkisi gerekiyorsa soru sor.

3. Görevi kapat; dependency invalidation uygula; gate/register durumunu güncelle.

4. Bir sonraki queue item'ı belirle ve STOP.

# 3. Dört Pozisyonun State-Driven Kuyruğu

| **Pozisyon** | **Mühendislik merkezi** | **İlk gate / başlangıç** |
| --- | --- | --- |
| P01 — Solar Low-Power ESP32 Sensor | Enerji + sensör + low-power duty cycle; ESP32-WROOM-32E/BME280 baseline | Mevcut GATE-2 blocker'ları + BQ24074 replacement-evidence propagation |
| P02 — Dual USB CAN/CAN-FD | USB <-> STM32H573 <-> dual FDCAN channels | STM32H573RIT6 simultaneous package pin/peripheral feasibility |
| P03 — CAN/DroneCAN Battery Monitor | Measurement → calibration → engineering state → DroneCAN telemetry/health | Measurement envelope + calibration + telemetry/fault semantics |
| P04 — PV + AC-Coupled BESS | Factory load <-> PV <-> BESS <-> grid/generator | Real load profile + existing SLD + dispatch objective |

| **Canonicality boundary: ESP32-C3 + LiFePO4 + VL53L1X ve benzeri çalışmalar, authoritative repo P01 mimarisini açıkça yeniden açmadıkça NON-CANONICAL alternatif senaryolardır.** |
| --- |

# 4. Pozisyon 1 — P01 Solar Low-Power ESP32 Sensor

Bu baseline'da aktif yürütme P01 üzerindedir. Mevcut state, BQ24074 candidate charger seçimi, P01-R03 main 3V3, P01-R04 battery protection ve current-BOM power/energy budget propagation üzerinden GATE-2 kapanışına bağlanır.

| **Item** | **Current state** | **Required propagation** |
| --- | --- | --- |
| DEC-03 / P01-R02 charger | DECIDED(candidate): TI BQ24074 | Broad charger research açma; yalnız BQ24074-specific current/leakage/thermal/input evidence ekle. |
| I\_charger\_quiescent | OPEN for BQ24074 | MCP73871 ~30 µA kullanma; BQ24074 applicable current terms/conditions kaynakla, sonra power budget güncelle. |
| DEC-11 / P01-R06 protection | DECIDED(candidate), rationale narrowed | BQ24074 OVP charger abs-max envelope; TVS/ESD surge/ESD rolünde; leakage yeniden hesaplanır. |
| P01-R03 main 3V3 | PROVISIONAL / BLOCKED | Regulator contract kapat; PV-01 brownout/dropout validation'ı koru. |
| P01-R04 battery protection | PROVISIONAL / BLOCKED | Protection boundary, procurement assumption ve PV-06 cutoff coordination kapat. |
| P01-R05 BME280/I2C gating | DECIDED(candidate) / VALIDATION\_REQUIRED | Yeniden araştırma yapma; switched-domain/high-Z contract'ı ECAD'e taşı. |
| GATE-2 | BLOCKED | R03/R04 closure + gerekli BQ24074 evidence propagation sonrası yeniden değerlendir. |

## 4.1 P01 Scenario × ECAD matrisi

| **Scenario** | **ECAD module** | **Focus** |
| --- | --- | --- |
| Solar only / cold Voc | Solar input + charger | Panel input envelope, BQ24074 limits/OVP, surge role |
| USB only | USB-C + charger | VBUS/CC/protection ve charge/system behavior |
| USB + solar / handover | Source arbitration + charger | No backfeed, source-state ve transient behavior |
| Wi-Fi TX peak | Main 3V3 + ESP32 core | Peak/transient, dropout/brownout, decoupling |
| Low battery | Battery + 3V3 + firmware policy | Cutoff coordination ve operating region |
| Sensor off | Load switch + I2C | VDD/VDDIO/pull-up/high-Z/back-power |
| Deep sleep | All always-on paths | Current/leakage budget with current-BOM evidence |
| Battery measurement | ADC frontend | Divider leakage, settling, calibration/error |
| USB debug | USB-UART + rail interlock | Debug VBUS ile system rail arasında backfeed/injection olmaması |

## 4.2 P01 ECAD modül haritası

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

## 4.3 P01 deterministic execution queue

| **Queue** | **Authority** | **Action** | **STOP / closure criterion** |
| --- | --- | --- | --- |
| P01-Q1 | TYPE-A VERIFY | BQ24074 replacement evidence propagate et; yalnız replacement-specific missing values ve stale calculations aç. | BQ24074 provenance explicit; stale power-budget terms identified. |
| P01-Q2 | TYPE-B SELECT / VERIFY | P01-R03 main 3V3 decision/contract kapat. | Decision/contract updated; PV-01 validation plan named. |
| P01-Q3 | TYPE-B SELECT / VERIFY | P01-R04 battery protection boundary kapat. | Protection boundary and PV-06 plan explicit. |
| P01-Q4 | CALCULATED | Current BOM evidence ile power/energy budget yeniden hesapla. | No historical component current remains active. |
| P01-Q5 | Gate re-evaluation | GATE-2'yi yeniden değerlendir. | ECAD contract implementation authorize edilir veya exact blocker'da durulur. |
| P01-Q6 | Product-plane implementation | Yalnız etkilenen ECAD modüllerini implement/refresh et. | Changed modules için ERC/DRC/interface checks; unrelated modules untouched. |

## 4.4 P01 — Codex pozisyon promptu

| ROLE: P01 State-Driven Closure Executor<br>PROJECT: Solar Low-Power ESP32 Sensor<br>BASELINE: ESP32-WROOM-32E + BME280; current charger candidate = TI BQ24074.<br><br>1. Load authoritative current state from repo + decision\_register + active GATE-2 + current branch/PR evidence.<br>2. Do NOT rebuild Phase 0 and do NOT reopen closed charger/I2C discovery.<br>3. Select only the first prerequisite-satisfied queue item:<br>P01-Q1 → P01-Q2 → P01-Q3 → P01-Q4 → P01-Q5 → conditional P01-Q6.<br>4. For part replacement, mark old part-specific evidence HISTORICAL\_INVALID\_FOR\_CURRENT\_BOM; invalidate only dependent calculations/contracts.<br>5. Never reuse MCP73871 numeric current values for BQ24074.<br>6. Ask the user only for DESIGN\_INPUT\_REQUIRED, genuine trade-off, or write/merge/push authorization.<br>7. Non-blocking new findings become NEW-GAP/DEFERRED.<br>8. When closure criterion is met: update state/register/gate/evidence path; identify next queue item; STOP.<br>9. P01-Q6 is authorized only if GATE-2 permits implementation.<br>10. If actual hardware evidence is required, mark VALIDATION\_REQUIRED; prepare the validation contract; do not reopen research merely because bench validation is pending. |
| --- |

# 5. Pozisyon 2 — P02 Dual USB CAN/CAN-FD

| **Queue** | **Package** | **Output / closure** |
| --- | --- | --- |
| P02-Q1 | STM32H573RIT6 pin/peripheral feasibility | Two FDCAN + USB + SWD + UART + ADC + clock/control/power pin map. Simultaneous mapping fails ise STOP. |
| P02-Q2 | CAN PHY + termination + protection | CAN-A/B ECAD contracts: transceiver/VIO, independent termination, TVS/CMC/ground. |
| P02-Q3 | Isolation decision | Non-isolated vs isolated architecture; power/area/creepage/EMI implications. |
| P02-Q4 | USB + debug UART | USB-device/debug ECAD contract; ayrı USB-UART gerekli mi karar. |
| P02-Q5 | Protected voltage ADC | Frontend + calibration/error budget. |
| P02-Q6 | Power architecture | Rails freeze; sonra KiCad implementation authorize. |

Target ECAD sheets: usb\_power, stm32h5, can\_channel\_a, can\_channel\_b, uart\_debug, voltage\_sense.

## 5.1 P02 — Codex pozisyon promptu

| ROLE: P02 State-Driven CAN Interface Executor<br>PROJECT: Dual USB CAN/CAN-FD<br><br>1. Start at P02-Q1; do not begin with generic CAN component discovery.<br>2. Prove simultaneous STM32H573RIT6 mapping for: 2×FDCAN + USB + SWD + UART + ADC + clock/control/power pins.<br>3. If simultaneous mapping fails, STOP and report the exact collision/blocker.<br>4. Only after Q1 closes, execute Q2→Q6 one closure at a time.<br>5. Respect RESEARCH\_CLOSED unless a valid TYPE-C REOPEN trigger exists.<br>6. Record rejected alternatives; propagate only dependency-local invalidation.<br>7. Every control-plane decision must map to an ECAD/interface artifact.<br>8. At each closure: update evidence/state, identify next queue item, STOP. |
| --- |

# 6. Pozisyon 3 — P03 CAN/DroneCAN Battery Monitor

| **Queue** | **Package** | **Output** |
| --- | --- | --- |
| P03-Q1 | Measurement envelope | Cell/pack voltage, current range, accuracy, common-mode, rate, isolation, fault limits. |
| P03-Q2 | Measurement architecture | MCU ADC+divider vs monitor/AFE; comparison + error budget. |
| P03-Q3 | Hall sensing + calibration | Range, offset/gain/drift, bandwidth, ADC compatibility, calibration model. |
| P03-Q4 | Measurement → DroneCAN | Raw → engineering unit → validity/health → DroneCAN field/update rate. |
| P03-Q5 | Fault/health + parameters | Fault semantics and AP\_Periph parameter contract. |
| P03-Q6 | Physical CAN + power/protection | Measurement/semantic contract sonrası ECAD physical layer. |
| P03-Q7 | Validation | Known-input/simulated-load/calibration/telemetry tests; unavailable 400 A bench iddiası yok. |

## 6.1 P03 — Codex pozisyon promptu

| ROLE: P03 State-Driven Measurement/Telemetry Executor<br>PROJECT: CAN/DroneCAN Battery Monitor<br><br>1. Begin with P03-Q1; do not start with generic battery-monitor product search.<br>2. Close in order: Q1 measurement envelope → Q2 architecture/error budget → Q3 Hall sensing/calibration → Q4 measurement-to-DroneCAN → Q5 fault/health/parameters → Q6 physical CAN/power/protection → Q7 validation.<br>3. Preserve the semantic chain:<br>physical quantity → raw measurement → calibration → engineering unit → validity/health → telemetry field/update rate.<br>4. Do not claim unavailable 400 A bench validation.<br>5. Where actual hardware evidence is missing, use VALIDATION\_REQUIRED rather than reopening research.<br>6. Every queue item ends with named closure criterion, evidence path, next queue item, STOP. |
| --- |

# 7. Pozisyon 4 — P04 PV + AC-Coupled BESS

| **Queue** | **Package** | **Output** |
| --- | --- | --- |
| P04-Q1 | Site/load baseline | Factory load profile, existing SLD, grid point/voltage, tariff/schedule, missing inputs. |
| P04-Q2 | Dispatch objective | Peak shaving/self-consumption/backup/clipping-recovery priority. |
| P04-Q3 | PV architecture | Module/inverter envelope, temperature string voltage, MPPT/string topology, DC:AC, loss/yield. |
| P04-Q4 | AC-coupled BESS | PCS/battery/AC bus/EMS/meter architecture + commercial-reference comparison. |
| P04-Q5 | BESS sizing | Reproducible kW/kWh dispatch/energy calculations with SOC/efficiency/degradation assumptions. |
| P04-Q6 | Tie-in/protection/siting | Power flow, transformer/switchgear, cable/protection ve siting constraints. |
| P04-Q7 | SLD/equipment schedule | Buildable design package with assumptions/evidence links. |
| P04-Q8 | Scenario validation | Normal, low-PV, high-load, BESS full/empty, grid outage, generator/ATS/recovery as applicable. |

## 7.1 P04 — Codex pozisyon promptu

| ROLE: P04 State-Driven PV/BESS Executor<br>PROJECT: PV + AC-Coupled BESS<br><br>1. Begin at P04-Q1.<br>2. Real site/load/SLD/tariff data are DESIGN\_INPUT\_REQUIRED when missing; do not replace them with generic internet research.<br>3. Execute Q1→Q8 sequentially.<br>4. Freeze only evidence-backed architecture/sizing assumptions.<br>5. Use reproducible calculations for kW/kWh/dispatch and preserve SOC/efficiency/degradation assumptions.<br>6. Site-specific, permit, utility or field claims require actual project data.<br>7. Every queue item must end with closure criterion → evidence path → next queue item → STOP. |
| --- |

# 8. Control Plane → Product Plane Dönüşüm Mantığı

| **Control-plane girdisi** | **Product-plane karşılığı** | **Kapanış kanıtı** |
| --- | --- | --- |
| Requirement / OPEN cell | ECAD, firmware, script, model veya hesap artefaktı | Named acceptance/closure criterion |
| Decision | Frozen interface/component/topology contract | Decision record + provenance |
| Scenario | İlgili ECAD/engineering module üzerinde davranış | Calculation / check / implementation evidence |
| Evidence gap | Minimum sufficient engineering action | Repository evidence path / updated state |
| Gate | Implementation authorization boundary | All prerequisite cells closed or exact blocker named |

| **Governance dokümanları ürün artefaktının yerine geçmez. İş akışı ancak control-plane kararı product-plane artefaktına ve closure evidence'ına bağlandığında ilerlemiş sayılır.** |
| --- |

# 9. Standart Codex Completion Report

## 9.1 Her görev sonunda zorunlu çıktı

| ACTIVE\_PROJECT:<br>ACTIVE\_QUEUE\_ITEM:<br>AUTHORITY\_TYPE:<br>STATE\_BEFORE:<br>ACTION\_TAKEN:<br>FILES\_CHANGED:<br>EVIDENCE\_ADDED:<br>DECISIONS\_UPDATED:<br>INVALIDATED\_DEPENDENCIES:<br>NEW\_GAPS\_DEFERRED:<br>VALIDATION\_REQUIRED:<br>STATE\_AFTER:<br>GATE\_STATUS:<br>NEXT\_QUEUE\_ITEM:<br>USER\_INPUT\_REQUIRED:<br>STOP\_REASON: |
| --- |

# 10. Yapılmaması Gerekenler

- Settled charger, I2C veya başka bir başlığı sırf yeni kaynak bulundu diye yeniden araştırmak.

- Eski MCP73871 sayısal akım değerlerini BQ24074 için kullanmak.

- P01 için non-canonical ESP32-C3/LiFePO4/VL53L1X senaryosunu mevcut baseline yerine geçirmek.

- P01 GATE-2 kapanmadan ilgisiz sonraki implementation kapsamını genişletmek.

- Bir parça değişimi sonrası bütün schematic/firmware/araştırma alanını invalid saymak.

- Fiziksel validation beklediği için research state'ini OPEN'a çevirmek.

- Plan, gap listesi veya governance dokümanını evidence klasörüne üretilmiş kanıt gibi koymak.

- Kullanıcı/site girdisi eksikken generic internet araştırmasıyla bu girdiyi tahmin etmek.

# 11. Doğrudan Uygulanabilir İlk Akış

| **P01-Q1 → P01-Q2 → P01-Q3 → P01-Q4 → P01-Q5 → (gate izin verirse) P01-Q6** |
| --- |

| **Adım** | **Yetki tipi** | **Beklenen sonuç** |
| --- | --- | --- |
| P01-Q1 | TYPE-A VERIFY | BQ24074 replacement-specific provenance ve stale terms listesi. |
| P01-Q2 | TYPE-B SELECT / VERIFY | Main 3V3 decision/contract closure + PV-01 plan. |
| P01-Q3 | TYPE-B SELECT / VERIFY | Battery protection boundary + PV-06 plan. |
| P01-Q4 | CALCULATED | Current-BOM power/energy budget; historical current terms sıfırlanmış. |
| P01-Q5 | Gate re-evaluation | ECAD implementation authorization veya exact remaining blocker. |
| P01-Q6 | Product-plane implementation | Yalnız affected ECAD modules; ERC/DRC/interface checks. |

# 12. Workflow Success / Definition of Success

- Claude/Codex sonraki görevi proje yeniden planlaması yapmadan tanıyabiliyor.

- Closed research tekrar edilmiyor.

- Component replacement yalnız bağımlı evidence/calculations alanını invalidate ediyor.

- Her senaryo bir engineering/ECAD module ve closure evidence ile eşleşiyor.

- Control-plane dokümanları gerçek product-plane artefaktlara yol açıyor.

- P01 current state stale synthesis veya non-canonical senaryolarla üzerine yazılmıyor.

- P02/P03/P04 generic component search yerine gerçek ilk engineering gate'lerinden başlıyor.

- Her task bir closure criterion ve STOP ile bitiyor.
