# "Katkı Yaptığım Repolar – current" Listesi: Derinlemesine Katkı Stratejisi

**Hazırlanma tarihi:** 1 Kasım 2026
**Kaynak:** https://github.com/stars/kemaldemirag/lists/katk%C4%B1-yapt%C4%B1%C4%9F%C4%B1m-repolar-current/

## En kritik bulgu

Listenin adı "katkı **yaptığım** repolar" olsa da GitHub'da bu üç repo için senin adına açılmış bir upstream PR veya issue bulunamıyor. Mevcut 10 PR'ın tamamı kendi hesabındaki ([kemaldemirag](https://github.com/kemaldemirag)) repolarda. Yani bu liste, hedeflenen katkılar için bir çalışma listesi, değilse henüz gerçek PR açılmamış.

Daha da kritik bir uyarı: üç projenin tümü de özellikle `docker:` ve `sim/` dosyalarına yapılan son taahhütten bu yana büyük ölçüde durakladı. Bu "dev olmayan" bakım durumları, stratejini doğrudan etkiler.

| Proje | Son anlamlı taahhüt | Duraklama süresi |
|---|---|---|
| [Miceuz/i2c-moisture-sensor](https://github.com/Miceuz/i2c-moisture-sensor) | `29dc4af Add calibration document` | ~2.5 yıl |
| [brianbennett/fpga_nes](https://github.com/brianbennett/fpga_nes) | `a12778b build: remove use of defunct codesourcery toolchain` | ~2 yıl |
| [jamieiles/80x86](https://github.com/jamieiles/80x86) | `344c33d docker: move to Quartus 20.1` | ~5 yıl |

Bakımcıların şu anki aktiviteleri bunu doğruluyor:
- [Miceuz](https://github.com/Miceuz) (Albertas Mickėnas, Technarium, Vilnius) şu anda [MeshCore](https://github.com/Miceuz/MeshCore) projesine yoğunlaşmış (Ekim 2026'da aktif).
- [jamieiles](https://github.com/jamieiles) (Jamie Iles, NUVACORE, Bristol) şu anda [merv](https://github.com/jamieiles/merv) ve [rxv](https://github.com/jamieiles/rxv) projelerine yoğunlaşmış (Ekim 2026'da aktif).
- [brianbennett](https://github.com/brianbennett) (Brian Bennett, Northborough, MA) hiçbir açık olayda görünmüyor; projeyi büyük ölçüde terk etti.

**Sonuç:** Üç projede de PR'larının kabul edilme ihtimali düşük. Amaç kabul edilmek değilse, strateji tamamen değişir (bkz. "Önerilen strateji").

## Açık konuların gerçek durumu

Bakımcılar veya diğer kullanıcılar hiçbir açık konuya yorum yazmamış. Bu, konuların gündemde olduğunu değil, tartışma talebinin ciddiye alınmadığını gösterir.

| Issue | Proje | Asıl istek |
|---|---|---|
| [#43](https://github.com/Miceuz/i2c-moisture-sensor/issues/43) | i2c-moisture-sensor | Birden çok sensörle ölçüm (I2C adresleme) |
| [#37](https://github.com/Miceuz/i2c-moisture-sensor/issues/37) | i2c-moisture-sensor | ESP32 / MQTT / LoRa sürümü |
| [#33](https://github.com/Miceuz/i2c-moisture-sensor/issues/33) | i2c-moisture-sensor | Daha kısa toprak giriş derinliği |
| [#31](https://github.com/Miceuz/i2c-moisture-sensor/issues/31) / [#30](https://github.com/Miceuz/i2c-moisture-sensor/issues/30) | i2c-moisture-sensor | Qwiic konnektörü ekle |
| [#34](https://github.com/brianbennett/fpga_nes/issues/34) | fpga_nes | `unknown error initializing com5` (SW/hata ayıklama aracı) |
| [#33](https://github.com/brianbennett/fpga_nes/issues/33) | fpga_nes | `$4003/$4007` yazıldığında pulse kanal sıralayıcısı sıfırlanmalı |
| [#31](https://github.com/brianbennett/fpga_nes/issues/31) | fpga_nes | IRQ'lar `P` register'ındaki kesme kapatma bitine uymalı |
| [#29](https://github.com/brianbennett/fpga_nes/issues/29) | fpga_nes | `$4017` yazıları 1–2 APU döngüsü gecikmeli olmalı |
| [#28](https://github.com/brianbennett/fpga_nes/issues/28) | fpga_nes | Frame counter IRQ üretimi bağlanmalı |
| [#16](https://github.com/jamieiles/80x86/issues/16) | 80x86 | Derleme sorununu çöz (`Docker` ortamı) |
| [#15](https://github.com/jamieiles/80x86/issues/15) | 80x86 | Orijinal 80186 bacak ↔ SystemVerilog sinyal eşleme tablosu |
| [#13](https://github.com/jamieiles/80x86/issues/13) | 80x86 | GateMate FPGA desteği ekle |
| [#12](https://github.com/jamieiles/80x86/issues/12) | 80x86 | NEC V20 serisi ve x87 FPU desteği |

## Teknik bağlam: koda bakarak değerlendirme

### [Miceuz/i2c-moisture-sensor](https://github.com/Miceuz/i2c-moisture-sensor)

Repo kökünde eski ve yeni KiCad dosyaları yan yana duruyor: `.sch`, `.pro`, `rescue.lib`, `cache.dcm`, `fp-lib-table` (KiCad 5 ve öncesi) ile `.kicad_pro`, `.kicad_pcb` (KiCad 6+). `3d`, `fp-info-cache`, `gerbers` dizinleri hâlâ el ile yönetiliyor. Yazılım tarafında `src/Makefile`, `main.c`, `thermistor.c`, `twi.c`, `programmer/` var; `ParticlePhoton-tutorial.md` ve `Soil Moisture Sensor Calibration.pdf` dokümantasyonu tamamlıyor.

**Katılım senin ilgi alanına uyar:** Sensör, I2C, KiCad PCB, ATtiny gömülü yazılım. Senin [stm32-can-modbus-gateway](https://github.com/kemaldemirag/stm32-can-modbus-gateway) ve [esp32-s3-automotive-interfaces](https://github.com/kemaldemirag/esp32-s3-automotive-interfaces) projelerindeki yetenek setiyle doğrudan örtüşür.

### [brianbennett/fpga_nes](https://github.com/brianbennett/fpga_nes)

`hw/src/` altında `cart`, `cpu`, `cmn`, `hci`, `ppu` dizinleri ve `nes_top.v`, `vram.v`, `wram.v` var. APU modülleri `hw/src/cpu/apu/apu*.v` altında: `apu.v`, `apu_pulse.v`, `apu_triangle.v`, `apu_noise.v`, `apu_length_counter.v`, `apu_envelope_generator.v`, `apu_div.v`, `apu_mixer.v`, `apu_frame_counter.v`. Yazılım tarafı `sw/` altında Windows Visual Studio çözümü (`nesdbg.sln`) ve C++ kaynakları, `sw/asm` altında assembly test romları (`bubble16.asm`, `bubble8.asm`, `trivial.asm`).

**Katılım senin ilgi alanına uyar:** RTL, APU donanım hatası ayıklama, SystemVerilog. Ayrıca `hw/src/ise` Xilinx ISE proje dosyaları içerir; bu araç 2013'ten sonra desteklenmiyor, modern Vivado'ya taşıma büyük bir teknik borçtur.

### [jamieiles/80x86](https://github.com/jamieiles/80x86)

`rtl/` altında modern bir SystemVerilog çekirdeği: `Core.sv`, `CSIPSync.sv`, `Divider.sv`, `Fifo.sv`, `Flags.sv`, `IP.sv`, `ImmediateReader.sv`, `InsnDecoder.sv`, `JumpTest.sv`, `LoadStore.sv`, `LoopCounter.sv`, `MemArbiter.sv`, `ModRMDecode.sv`, `Prefetch.sv`, `SegmentOverride.sv`, `SegmentRegisterFile.sv`, `TempReg.sv`. `rtl/alu/` ve `rtl/microcode/` büyük alt modüller içerir (`ADC`, `ADD`, `AND`, `BOUND`, `CALL`, `CMP`, `DIV`, `ENTER`, `ESC`, `HLT`, `INT`, `IO`, `JMP`, `LEA`, `LEAVE`, `LOOP`, `MOV`, `MUL`, `NEG`, `NOT`, `OR`, `POP`, `PUSH`, `RET`, `ROL`, `ROR`, `SAR`, `SBB`, `SCAS`, `SHL`, `SHR`, `STOS`, `SUB`, `TEST`, `WAIT`, `XCHG`, `XLAT`, `XOR`).

`fpga/` altında referans tasarımlar var: `de0-nano`, `de0-cv`, `hdp001`, ayrıca `VGA`, `bios`, `pic`, `ps2`, `sdram`, `spi`, `uart`, `leds`, `irq`, `timer`, `common`. `tests/` kapsamlı bir doğrulama çerçevesi ve `sim/` Verilator tabanlı bir simülatör içeriyor (`RTLCPU`, `cppmodel`, `display`, `PIC`, `Keyboard`, `Mouse`, `UART`, `SPI`, `Timer`).

**Katılım senin ilgi alanına uyar:** ASIC standard hücre tasarımya en yakın mimari örnek. GateMate taşıması (#13) FPGA kariyer hedefin için tam uyumlu.

## Önerilen strateji: "PR açma, portföy yapma"

Üç projenin de kabul ihtimali düşük olduğundan, amacı kabul ettirmek değil, **teknik öğrenme ve portföy oluşturma** olarak yeniden çerçevele:

1. **Hedefi değiştir:** "Bu repoya katkı yapayım" yerine "Bu repo üzerinden öğrenerek kendi projeme katkı yapayım". Fork alıp kendi hesabında çöz, kendi `README`'de neden yapıldığını ve ne öğrenildiğini yaz.
2. **Üç hedefi üç farklı öğrenme ekseni olarak kullan:**
   - `i2c-moisture-sensor`: KiCad modernizasyonu, sensör kalibrasyonu, gömülü I2C protokolü.
   - `fpga_nes`: RTL hata ayıklama, APU zamanlaması, doğrulama/testbench pratiği.
   - `80x86`: ASIC standard hücre, SystemVerilog mimarisi, doğrulama çerçevesi kullanımı.
3. **Bakımcı etkileşimini düşük tut:** Issue veya PR açma. Yorum yazmak bile bakımcının ilgisini çekmeyecek. Zaten üçünde de hiç yorum yok.
4. **Zamanı sınırlı tut:** Her projeye haftada birkaç saat; amaç "PR açma" değil "anlayıp kendi tarzında yeniden kurmak".
5. **Listeyi güncelle:** Listeyi "hedef" olarak yeniden adlandır ve bu derinleştirme raporunu listenin açıklamasına bağla. Bu, öz farkındalığı gösterir.

## Hangi katkı işine başlamalı?

Senin profilin ([STM32 CAN/Modbus gateway](https://github.com/kemaldemirag/stm32-can-modbus-gateway), [ESP32-S3 CAN/LIN/GNSS interfaces](https://github.com/kemaldemirag/esp32-s3-automotive-interfaces), [Zephyr RTOS experiments](https://github.com/kemaldemirag/Zephyr-RTOS-Experiments), FPGA-UART, humanoid robotics CAN/RealSense) gömülü, sensör, FPGA ve robotik odağını doğruluyor. Sıralama önerim:

1. **`80x86` GateMate taşıması (#13).** FPGA kariyer hedefin için en yüksek öğrenme değeri. Cologne Chip GateMate, görece yeni bir açık kaynak FPGA çipleri ailesi; RTL'yi bu hedefe taşımak, tasarım portability, zamanlama ve sentez ayarlarını öğretir. Çıktı: kendi hesabında `80x86-gatemate-port` reposu, ayrıntılı bir port deneyimi `README`.
2. **`fpga_nes` APU düzeltmeleri (#31, #33, #28, #29).** Gerçek donanım hataları; her biri küçük RTL düzeltmesi + testbench. Çıktı: kendi hesabında `fpga-nes-apu-fixes` reposu, 6502/APU zamanlaması notları.
3. **`i2c-moisture-sensor` KiCad modernizasyonu ve Qwiic revizyonu.** En hızlı, en düşük riskli. Çıktı: KiCad 8 temiz bir şema, Qwiic konnektörü ve yeni bir `README`.

## Kaynaklar

- [Repo listesi](https://github.com/stars/kemaldemirag/lists/katk%C4%B1-yapt%C4%B1%C4%9F%C4%B1m-repolar-current/)
- [Miceuz/i2c-moisture-sensor](https://github.com/Miceuz/i2c-moisture-sensor) — açık issue'lar [43](https://github.com/Miceuz/i2c-moisture-sensor/issues/43), [37](https://github.com/Miceuz/i2c-moisture-sensor/issues/37), [33](https://github.com/Miceuz/i2c-moisture-sensor/issues/33), [31](https://github.com/Miceuz/i2c-moisture-sensor/issues/31), [30](https://github.com/Miceuz/i2c-moisture-sensor/issues/30)
- [brianbennett/fpga_nes](https://github.com/brianbennett/fpga_nes) — açık issue'lar [34](https://github.com/brianbennett/fpga_nes/issues/34), [33](https://github.com/brianbennett/fpga_nes/issues/33), [31](https://github.com/brianbennett/fpga_nes/issues/31), [29](https://github.com/brianbennett/fpga_nes/issues/29), [28](https://github.com/brianbennett/fpga_nes/issues/28)
- [jamieiles/80x86](https://github.com/jamieiles/80x86) — açık issue'lar [16](https://github.com/jamieiles/80x86/issues/16), [15](https://github.com/jamieiles/80x86/issues/15), [13](https://github.com/jamieiles/80x86/issues/13), [12](https://github.com/jamieiles/80x86/issues/12)
- [kemaldemirag](https://github.com/kemaldemirag) — kişisel repo ve PR geçmişi
