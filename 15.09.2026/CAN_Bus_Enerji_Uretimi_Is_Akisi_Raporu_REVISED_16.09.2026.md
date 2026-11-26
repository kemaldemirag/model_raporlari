**CAN Bus + Energy Generation Portfolio**

**REVİZE İŞ AKIŞI RAPORU**

Codex Handoff revizyonu baz alınarak hazırlanmıştır<br>16.09.2026

| **Handoff ID** | CAN-ENERGY-PORTFOLIO-2026-09-15 |
| --- | --- |
| **Handoff durumu** | REVISED / EVIDENCE REQUIRED / RESEARCH DELTA INTEGRATED |
| **Workspace tipi** | Job-derived engineering portfolio |
| **Rapor odağı** | Araştırma deltası + scenario gate + kanıt zinciri + Codex icra sırası |

# 1. Yönetici Özeti

Revize Handoff, önceki yapıyı korurken iş akışını belirgin biçimde sıkılaştırıyor. Ana değişiklik, araştırma kayıtlarının artık doğrudan Codex girdisi sayılmaması; yalnızca bir gereksinimi, kararı, doğrulama yöntemini, test senaryosunu veya kanıt beklentisini değiştiren/savunan araştırmaların yürütme bağlamına alınmasıdır. Buna ek olarak specification-scenario matrix ve Phase 0A/0B kapıları, özellikle sayısal değerlerin erken ve kanıtsız biçimde “gereksinim”e dönüşmesini engelliyor.

- Öncelik hâlâ PROJECT-01 ve PROJECT-02; ancak artık bu iki proje için araştırma -> requirement -> scenario -> decision/evidence gap zinciri açıkça kurulacak.

- PROJECT-01 için I2C interface contract ve power-state model, PCB’den önce zorunlu ara çıktılar haline geldi.

- PROJECT-02 için CAN physical-layer/data-rate/termination scenario matrix, sayısal CAN FD profilleri seçilmeden önce zorunlu hale geldi.

- PROJECT-03, generic “battery monitor” araştırmasından çıkarılıp measurement-to-telemetry eksenine daraltıldı.

- PROJECT-04, operating-scenario matrix tarafından sürülen tekrar üretilebilir bir hesap/simülasyon paketi olarak daha net tanımlandı.

- Broad GitHub discovery tekrar edilmeyecek; yalnız unresolved-gap verification yapılacak.

# 2. Önceki Handoff’a Göre Kritik Revizyonlar

| **Alan** | **Önceki yaklaşım** | **Revize yaklaşım** | **İş akışına etkisi** |
| --- | --- | --- | --- |
| Araştırma girdisi | İlgili repo/kaynakların derlenmesi | Kaynak önce sınıflandırılır ve yalnız canonical requirement/decision/test/evidence bağını etkiliyorsa Codex bağlamına alınır | Araştırma gürültüsü azalır; Codex limiti gereksiz tüketilmez |
| Scenario sistemi | Dolaylı / proje bazlı | Requirement ID + specification axis + candidate scenario + source + decision status + verification + evidence path | Sayısal varsayımın gereksinime dönüşmesi kontrol edilir |
| Phase 0 | Skeleton + governance | Phase 0 + 0A + 0B | Uygulama öncesi araştırma eşleme ve sayısal-spec gate zorunlu olur |
| PROJECT-01 | Low-power + I2C araştırma | I2C contract, power-state model, ACK/NACK, missing-device, address collision, re-init ve enerji senaryoları | PCB’den önce davranış sözleşmesi çıkarılır |
| PROJECT-02 | CAN/FDCAN mimarisi | CAN spec matrix, termination test cases, disconnected channel, bus-off/recovery, data-rate evidence | Bitrate gibi rakamlar kanıtsız sabitlenmez |
| PROJECT-03 | CAN battery telemetry | Measurement-to-telemetry contract ve daraltılmış AFE/Hall/calibration/DroneCAN araştırması | Generic araştırma yerine ölçüm semantiğine bağlanır |
| PROJECT-04 | PV+BESS boyutlandırma | Operating scenario matrix + outage/low-SOC/BESS-unavailable/generator-fail/ATS-return senaryoları | Model yalnız normal işletmeye göre değil, arıza/transfer durumlarına göre de sınanır |

# 3. Revize Ana İş Akışı

Kaynak İş İlanı / Araştırma Kaydı<br>↓<br>Reference Classification<br>↓<br>Requirement Mapping<br>↓<br>Specification Axis<br>↓<br>Candidate Scenario<br>↓<br>Decision / Evidence Gap<br>↓<br>Codex Action<br>↓<br>Architecture / Interface Contract<br>↓<br>Implementation / Simulation<br>↓<br>Verification<br>↓<br>Evidence<br>↓<br>Validation / Release Gate

Revizyonun omurgası budur: ham araştırma doğrudan implementasyona gitmez. Önce karar veya kanıt açığına sıkıştırılır; Codex yalnız o açığı kapatmak için çalışır.

# 4. Faz Bazlı Uygulama Planı

## Phase 0 — Workspace ve Governance

- Workspace skeleton, root README, evidence policy, job-source map ve requirement templates oluştur.

- Reference-classification template ekle.

- Specification-scenario-matrix template ekle.

- Boş placeholder klasör üretme; her klasör anlamlı artifact veya amaç/durum README’si içersin.

## Phase 0A — Araştırma Eşleme

- PROJECT-01 ve PROJECT-02 için mevcut araştırmaları canonical requirement’lara eşle.

- Her araştırma girdisini RELEVANT, SUPPORTING, NEEDS\_VERIFICATION veya OUT\_OF\_SCOPE olarak sınıflandır.

- OPEN decision register ve scenario matrix oluştur.

- Broad web/GitHub keşfi yapma; yalnız mevcut kaydın hangi kararı veya açığı etkilediğini belirle.

## Phase 0B — Specification / Scenario Gate

- Her candidate numeric value için üç durumdan biri zorunlu: source-supported, decision-supported veya OPEN/ASSUMED + verification plan.

- Bu kapı geçilmeden PCB, firmware, numeric CAN profile, BESS duration vb. sabitlenmeyecek.

## Phase 1 — PROJECT-01 + PROJECT-02

- PROJECT-01: mevcut I2C/low-power araştırmasını interface ve power-state contracts’a dönüştür.

- PROJECT-02: CAN physical-layer/data-rate/termination scenario matrix’i kur; sonra mimari ve numeric profile seçimine geç.

## Phase 2 — PROJECT-03

- Measurement/protocol interfaces açık hale geldikten sonra başlat.

- İlk araştırma hedefi measurement-to-telemetry evidence olmalı.

## Phase 3 — PROJECT-04

- Operating-scenario matrix tarafından sürülen tekrar üretilebilir hesap/simülasyon paketi oluştur.

- Bütün varsayımları açıkça göster; duration gibi değerleri gerekçesiz gereksinime dönüştürme.

## Phase 4 — Cross-project Consistency

- Terminology, voltage domains, CAN semantics, status isimleri, evidence paths ve interface contracts arasında tutarlılık kontrolü yap.

## Phase 5 — Release

- Yalnız traceable evidence bulunan içerikler release’e alınsın.

# 5. PROJECT-01 — Solar-Powered Low-Power Sensor

Revize Handoff’ta PROJECT-01 yalnız güç bütçesi ve schematic konusu değildir; davranışsal bir I2C/power-state sözleşmesine dönüşmüştür.

sleep → wake → sensor power-on → stabilization → I2C init/discovery → measurement → telemetry → peripheral shutdown → sleep

- I2C voltage domain ve pull-up stratejisi doğrulanacak.

- Address inventory, discovery, initialization, ACK/NACK ve missing-device davranışı tanımlanacak.

- Seçili topoloji mümkün kılıyorsa duplicate-address handling kararı kaydedilecek.

- Sensor power gating sonrası stabilization delay ve bus re-initialization davranışı tanımlanacak.

- Board-level sleep-current budget yalnız ESP32 datasheet değeri değil; regulator Iq, divider leakage, sensor standby/power-gating etkileriyle hesaplanacak.

- Energy-surplus, energy-neutral ve energy-deficit senaryoları scenario matrix’te tutulacak; numeric threshold’lar kaynak yoksa OPEN kalacak.

## PROJECT-01 zorunlu yeni artifact’lar

- i2c\_interface\_contract.md

- power\_state\_model.md

- specification/scenario matrix kaydı

## PROJECT-01 Gate

- PCB implementation başlamadan önce I2C contract + power-state model + source/decision-supported numeric assumptions hazır olmalı.

# 6. PROJECT-02 — USB CAN/CAN FD Interface

- STM32H573 peripheral capabilities ve FDCAN pin mappings primary evidence ile doğrulanacak.

- USB device implementation ve USB-C CC davranışı kanıta bağlanacak.

- Transceiver voltage compatibility yanında physical-layer data-rate limiti de açıkça incelenecek.

- Nominal bitrate × data bitrate profilleri “candidate” olarak kalacak; MCU + transceiver + topology kanıtı olmadan requirement olmayacak.

- Selectable 120 Ω termination için no/one/two-termination test cases oluşturulacak.

- Disconnected-channel, bus-off ve recovery davranışları test senaryolarına eklenecek.

- FPGA-CAN veya farklı controller örnekleri yalnız SUPPORTING kalacak; STM32 mimarisi decision register ile değişmeden ana uygulama girdisi olmayacak.

## PROJECT-02 zorunlu yeni artifact

- can\_specification\_matrix.md

## PROJECT-02 Gate

- Numeric CAN FD profilleri, termination ve data-rate seçimleri primary evidence veya approved decision olmadan sabitlenmeyecek.

# 7. PROJECT-03 — CAN Battery Monitoring Node

- Cell AFE/ADC architecture, Hall current sensor polarity, ADC error budget, calibration points ve reproducibility birlikte ele alınacak.

- Plausibility checks yalnız threshold değil; stale/impossible measurement davranışını da kapsayacak.

- Measurement-to-telemetry field mapping açık bir sözleşmeye bağlanacak.

- DroneCAN feasibility ve BatteryInfo/cell-voltage semantics yalnız applicable ise kullanılacak.

- Generic “battery monitor” araması evidence sayılmayacak; cell AFE, Hall sensor, ADC/calibration, fault/status, DroneCAN ve AP\_Periph eksenlerine daraltılacak.

- Candidate component ve numeric ranges, job constraints + primary references desteklemeden OPEN kalacak.

## PROJECT-03 zorunlu yeni artifact

- measurement\_to\_telemetry\_contract.md

# 8. PROJECT-04 — PV + BESS + Backup Generation

- BESS power \[kW\], energy \[kWh\], duration \[h\], SOC window ve efficiency ayrı eksenler olarak modellenmeli.

- Dispatch strategy peak shaving, self-consumption ve reserve behavior ayrımıyla tanımlanmalı.

- Generator start/stop assumptions ve ATS transfer-mode assumptions açıkça belgelenmeli.

- Minimum senaryolar: normal day, PV surplus, evening/production peak, grid outage, outage + low BESS SOC, BESS unavailable, generator unavailable/failed start, ATS transfer ve grid-return.

- 1 h / 2 h / 4 h gibi duration değerleri yalnız sensitivity candidate; load model ve design objective gerekçesi olmadan requirement değildir.

## PROJECT-04 zorunlu yeni artifact

- operating\_scenario\_matrix.md

# 9. Araştırma Politikası ve Codex Limit Koruması

Revize Handoff’un en önemli operasyonel değişikliklerinden biri araştırma sıkıştırma kuralıdır:

raw records → requirement mapping → specification axis → candidate scenario → decision/evidence gap → Codex action

- Her external reference için title, organization/author, URL, access date, use, affected requirement, evidence strength ve action kaydedilecek.

- Bir kaynağın Araştırma Kayıtları / Referans Listeler içinde bulunması onu otomatik Handoff input yapmaz.

- Codex GitHub işi broad discovery değil, unresolved-gap verification ile sınırlı olacak.

- FPGA, robotics/navigation, Firebase/cloud identity ve unrelated PWM sonuçları canonical requirement veya approved decision açıkça kapsam içine almıyorsa OUT\_OF\_SCOPE kalacak.

- Sadece screened findings shared/references/github\_research.md içine alınacak.

# 10. Evidence, Status ve Validation Kuralları

- VERIFIED, VALIDATED, TESTED, PASS, COMPLETE, PRODUCTION READY ve MANUFACTURING READY kanıtsız kullanılamaz.

- Hardware status yalnız DESIGNED, SIMULATED, FABRICATED, ASSEMBLED, BENCH\_TESTED, FIELD\_TESTED olabilir.

- DRC/ERC sonucu yalnız design evidence’tır; bench/field validation yerine geçmez.

- Her proje validation\_matrix.csv + validation\_summary.md tutmalı.

- Scenario matrix ile validation matrix birbirinin yerine geçmez: ilki neyin/niçin test edileceğini ve hangi aday değerin seçileceğini yönetir; ikincisi test sonucunu ve evidence path’i kaydeder.

# 11. Zorunlu Kapılar ve Sprint-Fail Koşulları

**GATE-0 —** Reference classification + requirement mapping yoksa araştırma Codex yürütmesine alınmaz.

**GATE-1 —** Candidate numeric value source/decision-supported değil ve OPEN/ASSUMED olarak işaretli değilse implementasyona geçilmez.

**GATE-2 —** PROJECT-01 I2C interface contract ve power-state model yoksa PCB implementation başlamaz.

**GATE-3 —** PROJECT-02 CAN specification matrix ve termination/data-rate evidence yoksa numeric profile seçilmez.

**GATE-4 —** PROJECT-03 measurement-to-telemetry contract yoksa protocol/firmware “tamamlandı” sayılmaz.

**GATE-5 —** PROJECT-04 operating scenario matrix yoksa BESS/generator/ATS modeli release adayı değildir.

**FAIL —** Broad GitHub keşfinin tekrar edilmesi, unsupported numeric requirement üretimi, evidence path olmadan VERIFIED/PASS kullanılması, placeholder üretimin ilerleme gibi gösterilmesi veya client-delivery izlenimi verilmesi sprint başarısızlığıdır.

# 12. Revize İlk Codex Aksiyonu

1. Workspace skeleton ve Phase-0 governance dosyalarını oluştur.

2. Reference-classification ve specification-scenario-matrix şablonlarını ekle.

3. PROJECT-01 ve PROJECT-02 requirement inventory + OPEN decision register + scenario matrix üret.

4. PROJECT-01 mevcut I2C/low-power araştırmasını i2c\_interface\_contract.md ve power\_state\_model.md içine sıkıştır.

5. PROJECT-02 CAN physical-layer/data-rate/termination scenario matrix’i oluştur; numeric profile seçme.

6. Yalnız unresolved gap’ler için hedefli doğrulama araştırması yap.

7. Gates geçilmeden PCB layout, firmware implementation veya release üretme.

# 13. Minimum Çalışma Paketi — Bu Revizyona Göre

can\_energy\_portfolio/<br>├── README.md<br>├── docs/<br>│   ├── job\_sources.md<br>│   ├── portfolio\_map.md<br>│   ├── engineering\_rules.md<br>│   ├── evidence\_policy.md<br>│   ├── terminology.md<br>│   ├── reference\_classification\_template.md<br>│   └── specification\_scenario\_matrix\_template.md<br>├── 01\_solar\_low\_power\_sensor/<br>│   ├── 02\_requirements/requirements.md<br>│   ├── decision\_register.md<br>│   ├── i2c\_interface\_contract.md<br>│   └── power\_state\_model.md<br>├── 02\_usb\_can\_fd\_interface/<br>│   ├── 02\_requirements/requirements.md<br>│   ├── decision\_register.md<br>│   └── can\_specification\_matrix.md<br>└── shared/references/github\_research.md

# 14. Sonuç

Bu revizyon, portföyü “daha fazla araştırma ve daha fazla dosya” yönünde değil, “daha az ama karar üreten araştırma; daha açık scenario gate; daha az Codex tekrar işi; daha izlenebilir kanıt” yönünde geliştiriyor. En önemli operasyonel değişiklik budur.

- Olmazsa olmaz zincir: research -> requirement -> specification axis -> scenario -> decision/evidence gap -> Codex action -> verification -> evidence.

- PROJECT-01 ve PROJECT-02 artık yalnız ilk uygulanacak projeler değil; aynı zamanda araştırma sıkıştırma ve specification gate metodunun pilot projeleridir.

- Bu nedenle yeni Handoff, önceki rapordaki “Phase 0 → Project-01/02” akışını koruyor fakat Phase 0’ı 0A/0B ile gerçek bir mühendislik kapısına dönüştürüyor.
