**CAN Bus + Energy Generation Portfolio**

**State-Driven İş Akışı Raporu**

19.09.2026

| **Alan** | **Değer** |
| --- | --- |
| Handoff ID | CAN-ENERGY-PORTFOLIO-2026-09-15 |
| Revision | STATE-DRIVEN / SCENARIO × ECAD × DECISION |
| Çekirdek model | Product × Scenario × ECAD/Engineering Module × Decision × Evidence Dependency |
| Yürütme amacı | Yetkilendirilmiş bir sonraki mühendislik hücresini kapatmak; kapanmış araştırmayı yeniden açmamak; mimari sürüklenmeyi önlemek. |
| State authority | Current repo/decision register/active gate > current closure evidence > synthesis docs > raw research records |

| **Raporun ana yorumu**<br>Bu revizyonda iş akışı artık proje safhaları üzerinden değil, durum otoritesi ve bağımlılık grafiği üzerinden yürütülür. Bir görev yalnız önkoşulları sağlanmış ilk OPEN/BLOCKED hücreyi kapatır; kapanıştan sonra yalnız etkilenen bağımlılıklar güncellenir ve süreç durur. |
| --- |

# 1. Revizyonun İş Akışına Etkisi

- Önceki geniş araştırma / genel planlama yaklaşımı terk edilmiştir; yürütme bir 'state-driven execution contract' haline gelmiştir.

- Kontrol düzlemi (handoff, requirements, decisions, gates, queue) ile ürün düzlemi (ECAD, firmware, scripts, models, evidence) açıkça ayrılmıştır.

- Yeni bilgi aktif kapanış kriterini engellemiyorsa NEW-GAP/DEFERRED olarak ayrılır ve aktif işi rayından çıkarmaz.

- Bir parça değişimi tüm projeyi yeniden açmaz; yalnız o parçaya bağlı kanıtlar, hesaplar, arayüzler ve ECAD modülleri invalidate edilir.

- Fiziksel doğrulama beklenmesi, araştırmanın açık olduğu anlamına gelmez.

## 1.1 'DEVAM' komutunun operasyonel anlamı

1. Önkoşulları sağlanmış ilk kuyruk maddesini otomatik seç.

2. Yalnız DESIGN\_INPUT\_REQUIRED, eşdeğer kullanıcı tercih trade-off'u veya write/merge/push yetkisi gerekiyorsa soru sor.

3. Görevi kapat, bağımlılık invalidation'ını uygula, gate/register durumunu güncelle.

4. Bir sonraki kuyruk maddesini belirle ve STOP.

# 2. Durum Otoritesi ve Çalışma Sınırı

| **Öncelik** | **Kaynak** | **İş Akışındaki Otoritesi** |
| --- | --- | --- |
| 1 | Current repository + decision\_register + active gate + current branch/PR evidence | Mevcut proje durumunun otoritatif kaynağı. |
| 2 | Current research closure packages + primary evidence | Yalnız adlandırılmış kararı/gap'i kapatabilir veya yeniden açabilir. |
| 3 | Synthesis documents | Bağlam sağlar; yeni repo durumunu geçersiz kılamaz. |
| 4 | Raw records/screenshots/discovery | Promote edilip doğrulanana kadar discovery malzemesidir. |

## 2.1 State vocabulary

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

## 2.2 Yetki tipleri

| **Authority** | **İzin verilen eylem** |
| --- | --- |
| TYPE-A VERIFY | Mevcut baseline'ı doğrula. |
| TYPE-B SELECT | Belirtilmiş kriterlerle sınırlı adaylar arasından seç. |
| TYPE-C REOPEN | Baseline invalidate olmuştur; yalnız minimum replacement class araştırmasını aç. |

# 3. Invalidation ve Araştırma Kontrolü

- Değiştirilen bir komponentin sayısal özelliği yeni komponente taşınamaz.

- Part change: eski part-specific evidence → HISTORICAL\_INVALID\_FOR\_CURRENT\_BOM; yalnız eksik replacement evidence açılır; bağımlı hesaplar STALE olur.

- Topology change: yalnız doğrudan etkilenen ECAD modülleri ve downstream arayüz/hesaplar yeniden açılır.

- Missing deployment input, daha fazla generic web research gerekçesi değildir.

- Yeni araştırma yalnız named requirement, decision, evidence gap veya failed calculation kapatıyorsa açılır.

- Gerçek seçim açık değilse alternatif listesi büyütülmez; 2–4 aday sınırı korunur.

- Primary-source numerical limits; community repo ve marketing içerikten daha yüksek otoriteye sahiptir.

- Rejected alternatives kaydedilir; böylece sonraki ajanlar aynı araştırmayı tekrar etmez.

| **Araştırma kapatma kuralı**<br>RESEARCH\_CLOSED veya DECIDED(candidate) durumundaki bir konu, yalnız ilgili materyalin tekrar bulunmasıyla yeniden açılamaz. Yeniden açma için authoritative contradiction, component-limit violation, failed calculation veya implementation evidence ile varsayımın invalidate olması gerekir. |
| --- |

# 4. Ürün Baseline'ları ve İlk Mühendislik Gate'leri

| **Ürün** | **Mühendislik merkezi** | **İlk gate** |
| --- | --- | --- |
| P01 Solar Low-Power ESP32 Sensor | Enerji + sensör + low-power duty cycle; ESP32-WROOM-32E/BME280 baseline | Mevcut GATE-2 blocker'larını ve BQ24074 replacement-evidence propagation işini bitir. |
| P02 Dual USB CAN/CAN-FD | USB ↔ STM32H573 ↔ dual FDCAN channels | STM32H573RIT6 simultaneous package pin/peripheral feasibility. |
| P03 CAN/DroneCAN Battery Monitor | Measurement → calibration → engineering state → DroneCAN telemetry/health | Measurement envelope + calibration + telemetry/fault semantics. |
| P04 PV + AC-Coupled BESS | Factory load ↔ PV ↔ BESS ↔ grid/generator | Real load profile + existing SLD + dispatch objective. |

**Canonicality boundary:** ESP32-C3 + LiFePO4 + VL53L1X ve benzeri çalışmalar, authoritative repo P01 mimarisini açıkça yeniden açmadıkça NON-CANONICAL alternatif senaryolardır.

# 5. P01 — Mevcut Durum ve GATE-2 İş Akışı

| **Item** | **Current state** | **Required propagation** |
| --- | --- | --- |
| DEC-03 / P01-R02 charger | DECIDED(candidate): TI BQ24074 | Broad charger research açma; yalnız BQ24074-specific current/leakage/thermal/input evidence ekle. |
| I\_charger\_quiescent | OPEN for BQ24074 | MCP73871 ~30 µA kullanma; BQ24074 applicable current terms/conditions kaynakla, sonra power budget güncelle. |
| DEC-11 / P01-R06 protection | DECIDED(candidate), rationale narrowed | BQ24074 OVP charger abs-max envelope'i ele alır; TVS/ESD surge/ESD rolünde kalır. Yalnız leakage role yeniden hesaplanır. |
| P01-R03 main 3V3 | PROVISIONAL / BLOCKED | Regulator contract kapat; PV-01 brownout/dropout validation'ı koru. |
| P01-R04 battery protection | PROVISIONAL / BLOCKED | Protection boundary, procurement assumption ve PV-06 cutoff coordination kapat. |
| P01-R05 BME280/I2C gating | DECIDED(candidate) / VALIDATION\_REQUIRED | Yeniden araştırma yapma; switched-domain/high-Z contract'ı ECAD'e taşı. |
| GATE-2 | BLOCKED | R03/R04 closure + gerekli BQ24074 evidence propagation sonrası yeniden değerlendir. |

## 5.1 P01 Scenario × ECAD matrisi

| **Scenario** | **ECAD module** | **Focus** |
| --- | --- | --- |
| Solar only / cold Voc | Solar input + charger | Panel input envelope, BQ24074 limits/OVP, surge role. |
| USB only | USB-C + charger | VBUS/CC/protection ve charge/system behavior. |
| USB + solar / handover | Source arbitration + charger | No backfeed, source-state ve transient behavior. |
| Wi-Fi TX peak | Main 3V3 + ESP32 core | Peak/transient, dropout/brownout, decoupling. |
| Low battery | Battery + 3V3 + firmware policy | Cutoff coordination ve operating region. |
| Sensor off | Load switch + I2C | VDD/VDDIO/pull-up/high-Z/back-power. |
| Deep sleep | All always-on paths | Current/leakage budget with current-BOM evidence. |
| Battery measurement | ADC frontend | Divider leakage, settling, calibration/error. |
| USB debug | USB-UART + rail interlock | Debug VBUS ile system rail arasında backfeed/injection olmaması. |

## 5.2 P01 ECAD modül haritası

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

## 5.3 P01 deterministic execution queue

| **Queue** | **Action** | **STOP / closure criterion** |
| --- | --- | --- |
| P01-Q1 | BQ24074 replacement evidence propagate et; yalnız replacement-specific missing values ve stale calculations aç. | BQ24074 provenance explicit; stale power-budget terms identified. |
| P01-Q2 | P01-R03 main 3V3 decision/contract kapat. | Decision/contract updated; PV-01 validation plan named. |
| P01-Q3 | P01-R04 battery protection boundary kapat. | Protection boundary and PV-06 plan explicit. |
| P01-Q4 | Current BOM evidence ile power/energy budget yeniden hesapla. | No historical component current remains active. |
| P01-Q5 | GATE-2'yi yeniden değerlendir. | ECAD contract implementation authorize edilir veya exact blocker'da durulur. |
| P01-Q6 | Yalnız etkilenen ECAD modüllerini implement/refresh et. | Changed modules için ERC/DRC/interface checks; unrelated modules untouched. |

# 6. P02 — Deterministic Queue

| **Queue** | **Package** | **Output** |
| --- | --- | --- |
| P02-Q1 | STM32H573RIT6 pin/peripheral feasibility | Two FDCAN + USB + SWD + UART + ADC + clock/control/power pin map. Simultaneous mapping fails ise STOP. |
| P02-Q2 | CAN PHY + termination + protection | CAN-A/B ECAD contracts: transceiver/VIO, independent termination, TVS/CMC/ground. |
| P02-Q3 | Isolation decision | Non-isolated vs isolated architecture; power/area/creepage/EMI implications. |
| P02-Q4 | USB + debug UART | USB-device/debug ECAD contract; ayrı USB-UART gerekli mi karar. |
| P02-Q5 | Protected voltage ADC | Frontend + calibration/error budget. |
| P02-Q6 | Power architecture | Rails freeze; sonra KiCad implementation authorize. |

**Target ECAD sheets:** usb\_power, stm32h5, can\_channel\_a, can\_channel\_b, uart\_debug, voltage\_sense.

# 7. P03 — Deterministic Queue

| **Queue** | **Package** | **Output** |
| --- | --- | --- |
| P03-Q1 | Measurement envelope | Cell/pack voltage, current range, accuracy, common-mode, rate, isolation, fault limits. |
| P03-Q2 | Measurement architecture | MCU ADC+divider vs monitor/AFE; comparison + error budget. |
| P03-Q3 | Hall sensing + calibration | Range, offset/gain/drift, bandwidth, ADC compatibility, calibration model. |
| P03-Q4 | Measurement → DroneCAN | Raw → engineering unit → validity/health → DroneCAN field/update rate. |
| P03-Q5 | Fault/health + parameters | Fault semantics and AP\_Periph parameter contract. |
| P03-Q6 | Physical CAN + power/protection | Measurement/semantic contract sonrası ECAD physical layer. |
| P03-Q7 | Validation | Known-input/simulated-load/calibration/telemetry tests; unavailable 400 A bench iddiası yok. |

# 8. P04 — Deterministic Queue

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

# 9. Control Plane → Product Plane Dönüşüm Mantığı

| **Control-plane girdisi** | **Product-plane karşılığı** | **Kapanış kanıtı** |
| --- | --- | --- |
| Requirement / OPEN cell | ECAD, firmware, script, model veya hesap artefaktı | Named acceptance/closure criterion |
| Decision | Frozen interface/component/topology contract | Decision record + provenance |
| Scenario | İlgili ECAD/engineering module üzerinde davranış | Calculation / check / implementation evidence |
| Evidence gap | Minimum sufficient engineering action | Repository evidence path / updated state |
| Gate | Implementation authorization boundary | All prerequisite cells closed or exact blocker named |

| **Ana disiplin**<br>Governance dokümanları ürün artefaktının yerine geçmez. İş akışı ancak control-plane kararı, product-plane artefaktına ve onun closure evidence'ına bağlandığında ilerlemiş sayılır. |
| --- |

# 10. Uygulama Kuralları — Kısa Operasyon Protokolü

5. Authoritative current state'i repo + decision register + active gate üzerinden yükle.

6. İlk önkoşulu sağlanmış OPEN/BLOCKED hücreyi seç.

7. Bu hücre için gereken authority type'ı belirle: VERIFY / SELECT / REOPEN.

8. Yeni araştırma açmadan önce RESEARCH\_CLOSED ve current evidence durumunu kontrol et.

9. Yalnız gap'i kapatacak minimum engineering action'ı uygula.

10. Component/topology değişirse yalnız bağımlı kanıt/hesap/interface/ECAD modüllerini invalidate et.

11. Kapanış kriteri sağlandığında state/register/gate ve evidence path'i güncelle.

12. Yeni, non-blocking bulguları NEW-GAP/DEFERRED olarak ayır.

13. Bir sonraki queue item'ı tespit et.

14. STOP.

# 11. İş Akışında Yapılmaması Gerekenler

- Settled charger, I2C veya başka bir başlığı sırf yeni kaynak bulundu diye yeniden araştırmak.

- Eski MCP73871 sayısal akım değerlerini BQ24074 için kullanmak.

- P01 için non-canonical ESP32-C3/LiFePO4/VL53L1X senaryosunu mevcut baseline yerine geçirmek.

- P01 GATE-2 kapanmadan ilgisiz sonraki implementation kapsamını genişletmek.

- Bir parça değişimi sonrası bütün schematic/firmware/araştırma alanını invalid saymak.

- Fiziksel validation beklediği için research state'i OPEN'a çevirmek.

- Plan, gap listesi veya governance dokümanını evidence klasörüne üretilmiş kanıt gibi koymak.

- Kullanıcı/site girdisi eksikken generic internet araştırmasıyla bu girdiyi tahmin etmek.

# 12. Bu Handoff İçin Doğrudan Uygulanabilir İlk Akış

| **Aktif yürütme başlangıcı**<br>P01-Q1 → P01-Q2 → P01-Q3 → P01-Q4 → P01-Q5 → (gate izin verirse) P01-Q6. Bu sıra, current P01 state tablosunda belirtilen BQ24074 evidence propagation, main 3V3, battery protection ve power/energy budget bağımlılıklarını kapatmak üzere tanımlanmıştır. |
| --- |

| **Adım** | **Yetki tipi** | **Beklenen sonuç** |
| --- | --- | --- |
| P01-Q1 | TYPE-A VERIFY | BQ24074 replacement-specific provenance ve stale terms listesi. |
| P01-Q2 | TYPE-B SELECT / VERIFY | Main 3V3 decision/contract closure + PV-01 plan. |
| P01-Q3 | TYPE-B SELECT / VERIFY | Battery protection boundary + PV-06 plan. |
| P01-Q4 | CALCULATED | Current-BOM power/energy budget; historical current terms sıfırlanmış. |
| P01-Q5 | Gate re-evaluation | ECAD implementation authorization veya exact remaining blocker. |
| P01-Q6 | Product-plane implementation | Yalnız affected ECAD modules; ERC/DRC/interface checks. |

# 13. Handoff Success / Definition of Workflow Success

- Claude/Codex sonraki görevi proje yeniden planlaması yapmadan tanıyabiliyor.

- Closed research tekrar edilmiyor.

- Component replacement yalnız bağımlı evidence/calculations alanını invalidate ediyor.

- Her senaryo bir engineering/ECAD module ve closure evidence ile eşleşiyor.

- Control-plane dokümanları gerçek product-plane artefaktlara yol açıyor.

- P01 current state stale synthesis veya non-canonical senaryolarla üzerine yazılmıyor.

- P02/P03/P04 generic component search yerine gerçek ilk engineering gate'lerinden başlıyor.

- Her task bir closure criterion ve STOP ile bitiyor.

*Kaynak: Codex\_Handoff\_CAN\_Bus\_Enerji\_Uretimi\_Portfoy\_STATE\_DRIVEN\_REV\_19.09.2026.docx*
