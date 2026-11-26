# Benzer Sorunları Yaşayan Repolar: Karşılaştırmalı Araştırma

**Kaynak:** Listenizdeki 5 repo üzerinden benzer sorunları (duraklamış bakım, KiCad/PCB sorunları, FPGA RTL doğrulama) yaşayan diğer GitHub repoları.
**Tarih:** 1 Kasım 2026

Bu rapor, listenizdeki repoların **soru türlerine göre** benzer sorunları yaşayan diğer GitHub repolarını bulur ve karşılaştırır. Amaç, aynı tür katkı fırsatlarının daha geniş bir ekosistemde nerede olduğunu göstermek ve katkı stratejinizi çeşitlendirmektir.

## Yöntem

GitHub arama API'sini kullanarak şu sorgular çalıştırıldı:

- `teensy custom pcb kicad`, `teensy 4.1 custom board`
- `i.mx rt1062 kicad`
- `fpga nes apu verilog`, `nes fpga`, `8086 systemverilog fpga cpu`
- `x86 cpu rtl verilog`, `8086 verilog core`
- `capacitive soil moisture sensor kicad`

Her sonuç için yıldız sayısı, son güncelleme tarihi ve açık issue'lar kontrol edildi. "Benzer sorun" kriteri: **soru tipinin aynı olması** (KiCad/PCB revizyonu, FPGA RTL doğrulama, duraklamış bakım), **değil** konuyla birebir örtüşmesi.

## Sınıflandırma: 4 sorun tipi

### Tip 1: KiCad/PCB tasarımı ve revizyonu (Teensy-4.1-example benzeri)

Bu, listenizdeki [jenschr/Teensy-4.1-example](https://github.com/jenschr/Teensy-4.1-example) ile aynı sorun tipini yaşayan repolar.

| Repo | Açıklama | Yıldız | Son güncelleme | Durum |
|---|---|---|---|---|
| [itisnt/Teensy-4.1-Custom-Board](https://github.com/itisnt/Teensy-4.1-Custom-Board) | Teensy 4.1 için özel kart; Ethernet, 2x I2C ve RX1/TX1 portu | 0 | Eylül 2024 | Küçük, yeni; açık issue yok |
| [bahildebrand/soil-moisture-pcb](https://github.com/bahildebrand/soil-moisture-pcb) | KiCad'de yönlendirilmiş kapasitif toprak nem sensörü | 8 | Ekim 2018 | **Duraklamış**; 2 açık issue |
| [GewoonMaarten/soil-sensor](https://github.com/GewoonMaarten/soil-sensor) | Kapasitif toprak nem sensörü şema ve PCB | ? | ? | Benzer iş |
| [holochn/kicad_capacitive_soil_moisture_sensor](https://github.com/holochn/kicad_capacitive_soil_moisture_sensor) | KiCad'de kapasitif toprak nem sensörü | ? | ? | Benzer iş |
| [RonMcKay/capacitive-soil-moisture-sensor](https://github.com/RonMcKay/capacitive-soil-moisture-sensor) | Kompakt ve pil destekli kapasitif toprak nem sensörü | ? | ? | Benzer iş |

**Kritik bulgu:** [bahildebrand/soil-moisture-pcb](https://github.com/bahildebrand/soil-moisture-pcb), [Miceuz/i2c-moisture-sensor](https://github.com/Miceuz/i2c-moisture-sensor) ile **aynı konuyu** (kapasitif toprak nem sensörü) işliyor. Açık issue'ları:

- **[#2 "code"](https://github.com/bahildebrand/soil-moisture-pcb/issues/2)** — Kullanıcı kodu bulamıyor; bakımcı hâlâ elinde tuttuğu proje kodunu paylaşırsa güzel olurdu.
- **[#1 "value of capacitor"](https://github.com/bahildebrand/soil-moisture-pcb/issues/1)** — Kullanıcı direnç ve kondansatör değerlerini soruyor; 100k ve 220k etiketleri görmüş, kondansatör değerini öneriyor.

Bu, [Miceuz/i2c-moisture-sensor](https://github.com/Miceuz/i2c-moisture-sensor)'daki [#43 "Birden çok sensörle ölçüm"](https://github.com/Miceuz/i2c-moisture-sensor/issues/43) sorununa çok benziyor: kullanıcılar somut teknik detayları istiyor ama bakım duraklamış.

### Tip 2: FPGA NES emülatörü ve APU doğrulama (fpga_nes benzeri)

Bu, [brianbennett/fpga_nes](https://github.com/brianbennett/fpga_nes) ile aynı sorun tipini yaşayan repolar.

| Repo | Açıklama | Yıldız | Son güncelleme | Durum |
|---|---|---|---|---|
| [nand2mario/nestang](https://github.com/nand2mario/nestang) | Sipeed Tang Primer 25K, Nano 20K ve Primer 20K kartlarında FPGA NES | 458 | Haziran 2025 | **Aktif**; 38 açık issue |
| [andkorzh/RP2A03-7-](https://github.com/andkorzh/RP2A03-7-) | Tersine mühendislik tabanlı, saat-dakik FPGA NES 2A03 APU klonu | 44 | Eylül 2026 | **Aktif**; 2 açık issue |
| [strigeus/fpganes](https://github.com/strigeus/fpganes) | Verilog'de NES | ? | ? | Küçük proje |
| [spiritualized1997/openFPGA-NES](https://github.com/spiritualized1997/openFPGA-NES) | Analogue Pocket için openFPGA NES çekirdeği | ? | ? | Benzer iş |
| [agg23/openfpga-NES](https://github.com/agg23/openfpga-NES) | Analogue Pocket için NES | ? | ? | Benzer iş |
| [zf3/nes260](https://github.com/zf3/nes260) | Xilinx KV260 için NES emülatörü | ? | ? | Benzer iş |
| [Redherring32/UniversalPPU](https://github.com/Redherring32/UniversalPPU) | NES ve benzeri sistemlerin grafik çipi için FPGA değişimi | ? | ? | Benzer iş |
| [pgate1/NES_on_FPGA](https://github.com/pgate1/NES_on_FPGA) | FPGA'de NES uygulaması | ? | ? | Benzer iş |

**Kritik bulgu:** [nand2mario/nestang](https://github.com/nand2mario/nestang) **458 yıldız** ve **38 açık issue** ile **en aktif** alternatif. Açık issue'ları [brianbennett/fpga_nes](https://github.com/brianbennett/fpga_nes)'inkilere benziyor:

- **[#105 "Missing HDMI audio fix"](https://github.com/nand2mario/nestang/issues/105)** — Kullanıcı bazı HDMI cihazlarında ses alamıyor; `sample` sinyalinin işaretliğini değiştiren bir diff bile paylaşmış. Bu, [brianbennett/fpga_nes](https://github.com/brianbennett/fpga_nes)'teki [#29 "$4017 yazıları gecikmeli olmalı"](https://github.com/brianbennett/fpga_nes/issues/29) sorununa benziyor: **APU ses zamanlaması ve doğrulama**.
- **[#108 "Tang Primer 25K'de çalışmıyor"](https://github.com/nand2mario/nestang/issues/108)** — Kartta siyah ekran ve "nestang" yazısı; UART üzerinden veri iletilmiyor. Bu, [brianbennett/fpga_nes](https://github.com/brianbennett/fpga_nes)'teki [#34 "unknown error initializing com5"](https://github.com/brianbennett/fpga_nes/issues/34) sorununa benziyor: **kart-simülatör iletişim ve hata ayıklama**.

**Diğer kritik bulgu:** [andkorzh/RP2A03-7-](https://github.com/andkorzh/RP2A03-7-) doğrudan **APU klonu** yapıyor ve [brianbennett/fpga_nes](https://github.com/brianbennett/fpga_nes)'teki APU modülleri (`apu_pulse.v`, `apu_triangle.v`, `apu_noise.v`, `apu_frame_counter.v`) ile **aynı mimariyi** ele alıyor. Açık issue'ları:

- **[#3 "PPU-Lite V3 uyumluluğu"](https://github.com/andkorzh/RP2A03-7-/issues/3)** — Kullanıcı, ABD tarzı ön yükleyicili NES'yi tamamen yeni parçalarla kurmak istiyor; OEM PPU ve CPU yerine bu klon kartlarını kullanmak istiyor. Bu, kart yığını ve uyumluluk sorunu.
- **[#2 "BOM ve CPL dosyaları"](https://github.com/andkorzh/RP2A03-7-/issues/2)** — Kullanıcı JLCPCB'den tam lehimlenmiş PCB sipariş etmek istiyor; BOM ve CPL dosyalarını istiyor. Bu, [bahildebrand/soil-moisture-pcb](https://github.com/bahildebrand/soil-moisture-pcb)'deki [#1 "kondansatör değeri"](https://github.com/bahildebrand/soil-moisture-pcb/issues/1) sorununa benziyor: **üretim dosyaları eksik**.

### Tip 3: x86 CPU çekirdeği ve doğrulama (80x86 benzeri)

Bu, [jamieiles/80x86](https://github.com/jamieiles/80x86) ile aynı sorun tipini yaşayan repolar.

| Repo | Açıklama | Yıldız | Son güncelleme | Durum |
|---|---|---|---|---|
| [Tape-Out/ao486](https://github.com/Tape-Out/ao486) | Aleksander Osman'ın ao486 çekirdeği; 486 SX'nin her özelliği | ? | ? | Siyah kutu olarak |
| [SavourySnaX/8088_Core](https://github.com/SavourySnaX/8088_Core) | Verilog 8088 (ve nihayetinde 8086) çekirdeği; MiSTer_KonixMultisystem'de kullanılıyor | ? | ? | Küçük proje |
| [soumajit-m/8086-pipelined-core-verilog](https://github.com/soumajit-m/8086-pipelined-core-verilog) | Ayrık BIU/EU, 6 bayt önceden getirme FIFO, 20-bit segmentasyon ve Artix-7 FPGA sentezi ile sentezlenebilir Intel 8086 boru hattı çekirdeği | 0 | Ekim 2026 | **Yeni ve aktif**; 0 açık issue |
| [gumpavasantha-dot/custom_8086_processor_core_development](https://github.com/gumpavasantha-dot/custom_8086_processor_core_development) | Synopsys tabanlı testbench doğrulama ile özel 8086 işlemci çekirdeği ve SoC geliştirme | ? | ? | Öğrenme projesi |
| [kritikl/mp_16bit](https://github.com/kritikl/mp_16bit) | Modüler 16-bit mikro işlemci çekirdeği; ayrık BIU + EU, 8086 tarzı segmentasyon ve 6 bayt talimat kuyruğu | ? | ? | Öğrenme projesi |
| [bahattinyunus/teknofest_cip_tasarimi](https://github.com/bahattinyunus/teknofest_cip_tasarimi) | **Türkçe!** TEKNOFEST Çip Tasarım Yarışması için 32-bit RISC-V tabanlı (ZND-8086 çekirdeği) işlemci mimarisi; RTL'den GDSII üretimine | ? | ? | Türk kullanıcı, seninle uyumlu |

**Kritik bulgu:** [soumajit-m/8086-pipelined-core-verilog](https://github.com/soumajit-m/8086-pipelined-core-verilog) **Ekim 2026'da aktif** ve [jamieiles/80x86](https://github.com/jamieiles/80x86) ile **aynı mimari kavramları** (BIU/EU ayrımı, önceden getirme FIFO, segmentasyon) kullanıyor. Bu, öğrenme için ideal bir paralel proje.

**Dikkat çekici:** [bahattinyunus/teknofest_cip_tasarimi](https://github.com/bahattinyunus/teknofest_cip_tasarimi) **Türkçe** bir repo ve TEKNOFEST Çip Tasarım Yarışması için geliştirilmiş. Bu, senin Türkçe bağlamın ve ASIC kariyer hedefinle **çok uyumlu**. RISC-V tabanlı ama "ZND-8086" adını taşıyor.

### Tip 4: Duraklamış bakım ve topluluk katkısı eksikliği

Bu, listenizdeki üç projenin hepsinde ortak sorun: **bakımcı başka projelere odaklanmış**.

| Repo | Duraklama süresi | Bakımcının şu anki odağı |
|---|---|---|
| [Miceuz/i2c-moisture-sensor](https://github.com/Miceuz/i2c-moisture-sensor) | ~2.5 yıl | [MeshCore](https://github.com/Miceuz/MeshCore) |
| [brianbennett/fpga_nes](https://github.com/brianbennett/fpga_nes) | ~2 yıl | Yeni bir odağı görünmüyor |
| [jamieiles/80x86](https://github.com/jamieiles/80x86) | ~5 yıl | [merv](https://github.com/jamieiles/merv), [rxv](https://github.com/jamieiles/rxv) |
| [bahildebrand/soil-moisture-pcb](https://github.com/bahildebrand/soil-moisture-pcb) | ~8 yıl | Yeni bir odağı görünmüyor |

**Kritik bulgu:** Bu, **açık kaynak donanım ekosisteminin genel bir sorunu**. Yukarıdaki tüm repolarda benzer durumlar var. Bu, katkı stratejisi için önemli bir çıkarım sağlar: **PR kabul ettirmek değil, öğrenmek ve portföy oluşturmak**.

## Karşılaştırmalı katkı önerileri

Aşağıdaki tablo, her sorun tipi için hem listenizdeki repoları hem de benzer sorunları yaşayan diğer repoları sıralar ve önerilen katkı işini verir.

| Sorun tipi | Listenizdeki repo | Alternatif repo | Önerilen katkı |
|---|---|---|---|
| KiCad/PCB revizyonu | [jenschr/Teensy-4.1-example](https://github.com/jenschr/Teensy-4.1-example) | [itisnt/Teensy-4.1-Custom-Board](https://github.com/itisnt/Teensy-4.1-Custom-Board) | USB-C çip seçimi, hiyerarşik şema, güç pinleri |
| KiCad/PCB revizyonu | [Miceuz/i2c-moisture-sensor](https://github.com/Miceuz/i2c-moisture-sensor) | [bahildebrand/soil-moisture-pcb](https://github.com/bahildebrand/soil-moisture-pcb) | BOM/CPL dosyaları, kondansatör değerleri, Qwiic |
| FPGA NES APU doğrulama | [brianbennett/fpga_nes](https://github.com/brianbennett/fpga_nes) | [nand2mario/nestang](https://github.com/nand2mario/nestang) | HDMI ses işaretliği, APU zamanlaması |
| FPGA NES APU doğrulama | [brianbennett/fpga_nes](https://github.com/brianbennett/fpga_nes) | [andkorzh/RP2A03-7-](https://github.com/andkorzh/RP2A03-7-) | BOM/CPL dosyaları, PPU uyumluluğu |
| x86 CPU doğrulama | [jamieiles/80x86](https://github.com/jamieiles/80x86) | [soumajit-m/8086-pipelined-core-verilog](https://github.com/soumajit-m/8086-pipelined-core-verilog) | Artix-7 sentez, doğrulama |
| ASIC/çip tasarımı (Türkçe) | Yok | [bahattinyunus/teknofest_cip_tasarimi](https://github.com/bahattinyunus/teknofest_cip_tasarimi) | TEKNOFEST bağlamı, RTL-GDSII akışı |

## Strateji önerileri

1. **Aktif repolara yönel.** [nand2mario/nestang](https://github.com/nand2mario/nestang) (458 yıldız, 38 açık issue) ve [jenschr/Teensy-4.1-example](https://github.com/jenschr/Teensy-4.1-example) bakımcıları aktif. Bu, PR kabul edilme ihtimalini artırır.
2. **Benzer sorun tiplerini kariyer eksenlerine bağla.** KiCad/PCB sorunları senin PCB yeteneklerinle, FPGA RTL doğrulama sorunları senin FPGA/ASIC hedeflerinle doğrudan uyumlu.
3. **Türkçe bağlamı değerlendir.** [bahattinyunus/teknofest_cip_tasarimi](https://github.com/bahattinyunus/teknofest_cip_tasarimi) gibi Türkçe repolar senin dil bağlamınla uyumlu; TEKNOFEST Çip Tasarım Yarışması senin ASIC kariyer hedefin için güçlü bir portföy fırsatı.
4. **Duraklamış repoları öğrenme aracı olarak kullan.** [bahildebrand/soil-moisture-pcb](https://github.com/bahildebrand/soil-moisture-pcb) gibi duraklamış repolar, sorun çözme pratiği için ideal; PR kabul edilme beklentisi düşük tutulmalı.

## Kaynaklar

- [nand2mario/nestang](https://github.com/nand2mario/nestang) — açık issue'lar [#105](https://github.com/nand2mario/nestang/issues/105), [#108](https://github.com/nand2mario/nestang/issues/108)
- [andkorzh/RP2A03-7-](https://github.com/andkorzh/RP2A03-7-) — açık issue'lar [#3](https://github.com/andkorzh/RP2A03-7-/issues/3), [#2](https://github.com/andkorzh/RP2A03-7-/issues/2)
- [bahildebrand/soil-moisture-pcb](https://github.com/bahildebrand/soil-moisture-pcb) — açık issue'lar [#2](https://github.com/bahildebrand/soil-moisture-pcb/issues/2), [#1](https://github.com/bahildebrand/soil-moisture-pcb/issues/1)
- [soumajit-m/8086-pipelined-core-verilog](https://github.com/soumajit-m/8086-pipelined-core-verilog)
- [bahattinyunus/teknofest_cip_tasarimi](https://github.com/bahattinyunus/teknofest_cip_tasarimi)
- [itisnt/Teensy-4.1-Custom-Board](https://github.com/itisnt/Teensy-4.1-Custom-Board)
- [Tape-Out/ao486](https://github.com/Tape-Out/ao486)
