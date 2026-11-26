# brianbennett/fpga_nes: Tüm Issue Analizi

**Repo:** [brianbennett/fpga_nes](https://github.com/brianbennett/fpga_nes)
**Toplam issue:** 36 (15 açık, 21 kapalı)
**Bakımcı:** [Brian Bennett](https://github.com/brianbennett) (Northborough, MA; büyük ölçüde terk etmiş, ~2 yıldır güncelleme yok)
**Rapor tarihi:** 1 Kasım 2026

Bu rapor, listenizin ikinci sırasındaki fpga_nes reposundaki **tüm 36 issue'yu** analiz eder. Her birine katkı yapmanın teknik bağlamını, zorluğunu ve kariyer hedeflerinizle uyumunu verir.

## Açık issue'lar: tam analiz

### 1. [#34 "unknown error initiaizing com5"](https://github.com/brianbennett/fpga_nes/issues/34) — 2013

**Talep:** Çinli bir kullanıcı, ISE'nin ürettiği bit dosyasını yükleyip seri portu açtıktan sonra `NesDbg` aracında `Unknown Error initiaizing com5` hatası alıyor. Seri ayarları 38400, ODD, tek bit stop.

**Topluluk etkileşimi:** Bu issue'da **en çok yorum** var (6). 2014'te Çinli kullanıcılar benzer sorunları paylaşmış. Bakımcı [brianbennett](https://github.com/brianbennett) sonunda **çözümü bulmuş**: FPGA bağlantısı COM5'te değil; `NesDbg` bunu sabit kodlamış. Bir fork ([denghongcai/fpga_nes](https://github.com/denghongcai/fpga_nes)) komut satırı parametresiyle COM portunu belirtmeyi ekleyerek bunu çözmüş: [commit](https://github.com/denghongcai/fpga_nes/commit/021a890721ca7dfa8bf397dcce3d0221d1f0095a). Ama bu fix **upstream'e alınmamış**.

**Katılım planı:** Bu, **en somut ve çözülmüş ama uygulanmamış** iş. Denghongcai'nin çözümünü upstream'e taşıyarak PR açmak en kolay katkı. `sw/src/serialcomm.cpp` veya `sw/src/nesdbg.cpp` içindeki COM5 sabit kodunu komut satırı argümanıyla değiştir.

**Zorluk:** Düşük. İyi bir ilk PR.

### 2. [#33 "Reset pulse channel sequencer on write to $4003/$4007"](https://github.com/brianbennett/fpga_nes/issues/33) — 2012

**Talep:** `$4003` veya `$4007` adreslerine yazıldığında pulse kanalının sıralayıcısının sıfırlanması gerekiyor. Gövde boş; yalnızca başlıkta talep var.

**Teknik bağlam:** `hw/src/cpu/apu/apu_pulse.v` dosyasında pulse kanalının sıralayıcısı var. NES APU spesifikasyonuna göre `$4003`/`$4007` yazısı kanalın periyodunu ve sıralayıcıyı sıfırlar. Bu, gerçek donanım zamanlaması sorunu.

**Katılım planı:** `apu_pulse.v` dosyasında `$4003` ve `$4007` yazılarını algılayan bir sinyal ekleyip sıralayıcıyı sıfırla. Testbench ile doğrula.

**Zorluk:** Orta. RTL doğrulama pratiği kazandırır.

### 3. [#31 "IRQs need to honor P register's interrupt disable bit"](https://github.com/brianbennett/fpga_nes/issues/31) — 2012

**Talep:** Kesmeler (IRQ), işlemcinin P register'ındaki kesme kapatma bitine uymalı. Gövde boş.

**Teknik bağlam:** 6502 çekirdeğinde P register'ının I biti (kesme kapatma) IRQ'ları maskelemeli. Ama fpga_nes'in çekirdeği bu biti yok sayıyor.

**Katılım planı:** `hw/src/cpu/rp2a03.v` veya ilgili çekirdek dosyasında IRQ sinyalini P register'ının I bitiyle AND'le. Testbench ile doğrula.

**Zorluk:** Orta. RTL doğrulama pratiği.

### 4. [#29 "Fix APU 'jitter'"](https://github.com/brianbennett/fpga_nes/issues/29) — 2012

**Talep:** `$4017`'ye yazılar **1 ila 2 APU döngüsü sonra** etkili olmalı. Bu, frame counter'ın zamanlamasıyla ilgili; şu an hemen etkili oluyor ve bu "jitter" (titreme) yaratıyor.

**Teknik bağlam:** `hw/src/cpu/apu/apu_frame_counter.v` dosyası `$4017` yazılarını işliyor. Gerçek NES APU'da bu yazı 1-2 APU döngüsü gecikmeli etki eder; fpga_nes bunu hemen yapıyor ve ses titremesine yol açıyor.

**Katılım planı:** `apu_frame_counter.v`'de `$4017` yazısını 1-2 döngülük bir kayıt defterine (register) sokup sonra uygula. Testbench ile doğrula.

**Zorluk:** Orta-yüksek. Gerçek donanım zamanlaması.

### 5. [#28 "Hookup frame counter IRQ generation"](https://github.com/brianbennett/fpga_nes/issues/28) — 2012

**Talep:** Frame counter'ın kesme üretimini bağla. Gövde boş.

**Teknik bağlam:** `apu_frame_counter.v` kesme üretmiyor ya da ürettiği kesme hiçbir yere bağlı değil. Bu, APU'nun oyunlara per-kare zamanlama sinyali vermesi için kritik.

**Katılım planı:** Frame counter'dan IRQ sinyali çıkar, bunu çekirdeğin kesme girişine bağla. `apu.v` veya `rp2a03.v` düzeyinde bağlantı yap.

**Zorluk:** Orta. Sistem düzeyinde bağlantı işi.

### 6. [#22 "Use shared CPU pulse between 6502 core and APU"](https://github.com/brianbennett/fpga_nes/issues/22) — 2012

**Talep:** 6502 çekirdeği ile APU arasında paylaşılan bir CPU pulse (saat darbesi) kullan.

**Teknik bağlam:** Şu anda 6502 çekirdeği ve APU farklı saat darbeleri kullanıyor. Bu, kaynak israfına ve zamanlama uyumsuzluğuna yol açıyor.

**Katılım planı:** Ortak bir pulse üreticisi yap, hem çekirdeğe hem APU'ya dağişken saat ver.

**Zorluk:** Orta-yüksek. Mimari düzeyde değişiklik.

### 7. [#20 "Add configurable COM port for NesDbg"](https://github.com/brianbennett/fpga_nes/issues/20) — 2012

**Talep:** Şu anda COM5'e sabit kodlanmış. Yapılandırılabilir COM portu ekle.

**Katılım planı:** Bu, [#34](https://github.com/brianbennett/fpga_nes/issues/34)'teki çözümün resmileştirilmesi. Aynı PR zincirinde ele alınabilir. Komut satırı argümanı veya yapılandırma dosyasıyla COM portunu belirt.

**Zorluk:** Düşük.

### 8. Oyun uyumluluk sorunları (2012, #19, #18, #17, #16, #15)

Bu altı issue, **belirli oyunların görsel/çalışma sorunlarını** listeler:

| Issue | Oyun | Sorun |
|---|---|---|
| [#19](https://github.com/brianbennett/fpga_nes/issues/19) | Soccer | Kaydırma sırasında üst durum çubuğunda bozulma |
| [#18](https://github.com/brianbennett/fpga_nes/issues/18) | Slalom | Dönüş başladığında demoda bozulma |
| [#17](https://github.com/brianbennett/fpga_nes/issues/17) | Othello | Başlık ekranı gelmeden önce kilitlenme |
| [#16](https://github.com/brianbennett/fpga_nes/issues/16) | Mach Rider | Demoda belirgin bozulma |
| [#15](https://github.com/brianbennett/fpga_nes/issues/15) | 1942 | Bozulma; üst yazıda eksik harfler, hafif titreme |

**Teknik bağlam:** Bu sorunların çoğu **PPU zamanlaması ve sprite DMA** sorunlarından kaynaklanıyor. [#6 "Fix sprite DMA timing"](https://github.com/brianbennett/fpga_nes/issues/6) ve [#11 "BRAM frame buffer support"](https://github.com/brianbennett/fpga_nes/issues/11) ile doğrudan bağlantılı.

**Katılım planı:** Bu issue'lar ayrı ayrı ele alınmamalı. Kök nedenler: [#6](https://github.com/brianbennett/fpga_nes/issues/6) (sprite DMA zamanlaması), [#8](https://github.com/brianbennett/fpga_nes/issues/8) (RGB emphasis bitleri) ve [#7](https://github.com/brianbennett/fpga_nes/issues/7) (monokrom mod). Bunları çözünce oyun sorunlarının çoğu düzelir.

**Zorluk:** Yüksek. PPU zamanlaması karmaşık.

### 9. [#14 "Auto input for test automation"](https://github.com/brianbennett/fpga_nes/issues/14) — 2012

**Talep:** `NesDbg` Lua betiklerinin FPGA'ye denetleyici girişi göndermesine izin ver.

**Katılım planı:** `sw/src/scriptmgr.cpp` dosyasına Lua API ekle; denetleyici durumunu FPGA'ye yazan bir komut ekle.

**Zorluk:** Orta. Yazılım katkısı.

### 10. [#13 "Frame capture, ROM test automation"](https://github.com/brianbennett/fpga_nes/issues/13) — 2012

**Talep:** BRAM frame buffer desteklendikten sonra kare yakalama ve `NesDbg` desteği ekle; ROM'ların otomatik testi için.

**Bağımlılık:** [#11 BRAM frame buffer support](https://github.com/brianbennett/fpga_nes/issues/11)'a bağlı.

**Katılım planı:** Önce [#11](https://github.com/brianbennett/fpga_nes/issues/11)'i çöz; sonra kare yakalama API'si ekle.

**Zorluk:** Yüksek. Uzun vadeli iş.

### 11. [#12 "Improve CPU/PPU timing correlation"](https://github.com/brianbennett/fpga_nes/issues/12) — 2012

**Talep:** BRAM frame buffer kullanıldıktan sonra CPU ve PPU zamanlamalarını orijinal NES ile tam eşleştirmeye çalış.

**Bağımlılık:** [#11](https://github.com/brianbennett/fpga_nes/issues/11)'e bağlı.

**Katılım planı:** Uzun vadeli; öncelik [#11](https://github.com/brianbennett/fpga_nes/issues/11).

**Zorluk:** Yüksek.

### 12. [#11 "BRAM frame buffer support"](https://github.com/brianbennett/fpga_nes/issues/11) — 2012

**Talep:** PPU, piksel rengini çıktı olarak hesaplamak yerine bir "arka tampon"a (back buffer) yazmalı. Bu şunları sağlar:
- **Mükemmel PPU zamanlaması.** Şu anda PPU zamanlamaları VGA sinyal gereksinimlerine dayanıyor. Renk üretimini görüntüden ayırınca farklı bir saatte çalışabilir.
- **Otomatik test için kare yakalama.**

**Katılım planı:** `hw/src/ppu/` altındaki PPU modüllerinde bir BRAM frame buffer ekle. Renk hesaplamayı görüntü saatinden ayır.

**Zorluk:** Yüksek. Mimari düzeyde büyük değişiklik.

### 13. [#10 "Implement switchable support for real/fake joypads."](https://github.com/brianbennett/fpga_nes/issues/10) — 2012

**Talep:** Gerçek NES denetleyicilerinin taklitlerden farklı sinyal polaritesi var.

**Katılım planı:** Denetleyici modülünde polarite değiştirme desteği ekle.

**Zorluk:** Orta.

### 14. [#9 "Implement SRAM controller"](https://github.com/brianbennett/fpga_nes/issues/9) — 2012

**Talep:** Kart veri depolaması için blockram yerine SRAM kullan. Bu, NROM olmayan mapper'ları desteklemenin ilk adımı.

**Katılım planı:** FPGA kartındaki harici SRAM'e bir denetleyici ekle. Bu, [#3](https://github.com/brianbennett/fpga_nes/issues/3) ile bağlantılı.

**Zorluk:** Orta-yüksek. Donanım bağımlı.

### 15. [#8 "Implement RGB emphasis bits"](https://github.com/brianbennett/fpga_nes/issues/8) ve [#7 "Implement monochrome mode"](https://github.com/brianbennett/fpga_nes/issues/7) — 2012

**Talep:** PPU'nun renk vurgu bitlerini ve monokrom modunu uygula.

**Katılım planı:** PPU'nun palet işleme modülünde bu bitleri işle. [#15 1942](https://github.com/brianbennett/fpga_nes/issues/15) gibi oyun sorunlarını çözer.

**Zorluk:** Orta.

### 16. [#6 "Fix sprite DMA timing"](https://github.com/brianbennett/fpga_nes/issues/6) — 2012

**Talep:** Sprite DMA'ları çok hızlı bitiyor; orijinal NES zamanlamasıyla eşleşmeli.

**Teknik bağlam:** Orijinal NES'te sprite DMA 513-514 CPU döngüsü sürer. fpga_nes bunu çok hızlı yapıyor ve bu, oyun bozulmalarına yol açıyor.

**Katılım planı:** Sprite DMA modülünde bekleme durumları ekle; doğru döngü sayısını uygula. Testbench ile doğrula.

**Zorluk:** Orta. Ama **çok sayıda oyun sorununu** çözer.

### 17. [#3 "Implement alternate path for uploading cart data"](https://github.com/brianbennett/fpga_nes/issues/3) — 2012

**Talep:** Şu anda CPU'nun belleğe yazmak için kullandığı normal yolu kullanıyor. Mapper'ları desteklemek için fiziksel banklara (mapped adresler yerine) yüklemeli.

**Katılım planı:** Kart yükleme modülünde fiziksel bank adreslemesi ekle. [#9 SRAM controller](https://github.com/brianbennett/fpga_nes/issues/9) ile bağlantılı.

**Zorluk:** Orta-yüksek.

## Kapalı issue'lar: öğrenme değeri

Kapalı issue'lar, **projenin mimari kararlarını ve bakımcının yaklaşımını** anlamak için değerli:

- **[#36 "Update README.md"](https://github.com/brianbennett/fpga_nes/issues/36), [#35 "Denghongcai master"](https://github.com/brianbennett/fpga_nes/issues/35)** — PR'lar; bakımcının nasıl çalıştığını gösterir.
- **[#32 "IRQ should push a B value of 0."](https://github.com/brianbennett/fpga_nes/issues/32), [#30 "IRQ Interrupts should be level based"](https://github.com/brianbennett/fpga_nes/issues/30)** — IRQ ile ilgili önceki düzeltmeler; [#31](https://github.com/brianbennett/fpga_nes/issues/31)'i anlamak için önemli.
- **[#24 "Fix $4017 write behavior to clock appropriate frame counter signals"](https://github.com/brianbennett/fpga_nes/issues/24), [#29 "Fix APU 'jitter'"](https://github.com/brianbennett/fpga_nes/issues/29)** — APU zamanlaması düzeltmeleri; [#29](https://github.com/brianbennett/fpga_nes/issues/29)'un devamı.
- **[#26 "Use smarter sequence generator for pulse duty cycle"](https://github.com/brianbennett/fpga_nes/issues/26), [#23 "Refactor apu_div to remove internal period regs, remove apu_div_const"](https://github.com/brianbennett/fpga_nes/issues/23)** — APU modülü iyileştirmeleri.
- **[#25 "SMB flagpole sound effect"](https://github.com/brianbennett/fpga_nes/issues/25)** — Ses doğrulama örneği.
- **[#2 "Implement unofficial opcodes"](https://github.com/brianbennett/fpga_nes/issues/2)** — 6502 resmi olmayan talimatları; commit geçmişinde `ARR`, `ASR`, `AAC`, `SYA`, `SXA` gibi talimatlar eklendiğini görüyoruz.
- **[#1 "Fix BRK implementation"](https://github.com/brianbennett/fpga_nes/issues/1)** — 6502 BRK talimatı düzeltmesi.
- **[#21 "Upgrade to ISE 14.1"](https://github.com/brianbennett/fpga_nes/issues/21)** — Xilinx ISE sürüm yükseltmesi; ISE artık desteklenmiyor, Vivado'ya taşınması büyük bir teknik borç.

## Katkı öncelik sırası

1. **[#34 / [#20 COM portu yapılandırması](https://github.com/brianbennett/fpga_nes/issues/34)** — Çözüm var (fork'ta) ama uygulanmamış. En kolay ve en somut iş. Denghongcai'nin fix'ini upstream'e taşı.
2. **[#6 Sprite DMA zamanlaması](https://github.com/brianbennett/fpga_nes/issues/6)** — Birçok oyun sorununu çözer; testbench ile doğrulanabilir. RTL doğrulama pratiği.
3. **[#33 Pulse kanal sıralayıcı sıfırlama](https://github.com/brianbennett/fpga_nes/issues/33)** — Küçük ve iyi tanımlı RTL işi.
4. **[#29 APU jitter](https://github.com/brianbennett/fpga_nes/issues/29), [#28 Frame counter IRQ](https://github.com/brianbennett/fpga_nes/issues/28)** — APU zamanlaması; orta zorlukta.
5. **[#11 BRAM frame buffer](https://github.com/brianbennett/fpga_nes/issues/11)** — Uzun vadeli, mimari değişiklik; PPU zamanlamasını mükemmelleştirir.

## Kariyer uyumu ve strateji

Bu repo, **FPGA/RTL kariyer hedefinizle en uyumlu** projelerden biri. Aşağıdakileri öğrenirsiniz:

- **6502/APU donanım doğrulama** — Gerçek donanım zamanlamasını (sprite DMA, APU jitter, IRQ maskeleme) testbench ile doğrulama.
- **SystemVerilog/Verilog RTL tasarım** — `hw/src/` altındaki modüller.
- **FPGA sentez ve araç zinciri** — ISE'den Vivado'ya taşıma büyük bir teknik borç.
- **Donanım-yazılım arayüzü** — `NesDbg` aracı, seri iletişim, Lua otomasyonu.

Ama **bakımcı terk etmiş**; PR kabul edilme ihtimali düşük. Strateji: **öğrenme odaklı** yaklaş, kendi hesabınızda çözümleri paylaşın.

## Kaynaklar

- [brianbennett/fpga_nes](https://github.com/brianbennett/fpga_nes) — repo
- Açık issue'lar: [#34](https://github.com/brianbennett/fpga_nes/issues/34), [#33](https://github.com/brianbennett/fpga_nes/issues/33), [#31](https://github.com/brianbennett/fpga_nes/issues/31), [#29](https://github.com/brianbennett/fpga_nes/issues/29), [#28](https://github.com/brianbennett/fpga_nes/issues/28), [#22](https://github.com/brianbennett/fpga_nes/issues/22), [#20](https://github.com/brianbennett/fpga_nes/issues/20), [#19](https://github.com/brianbennett/fpga_nes/issues/19), [#18](https://github.com/brianbennett/fpga_nes/issues/18), [#17](https://github.com/brianbennett/fpga_nes/issues/17), [#16](https://github.com/brianbennett/fpga_nes/issues/16), [#15](https://github.com/brianbennett/fpga_nes/issues/15), [#14](https://github.com/brianbennett/fpga_nes/issues/14), [#13](https://github.com/brianbennett/fpga_nes/issues/13), [#12](https://github.com/brianbennett/fpga_nes/issues/12), [#11](https://github.com/brianbennett/fpga_nes/issues/11), [#10](https://github.com/brianbennett/fpga_nes/issues/10), [#9](https://github.com/brianbennett/fpga_nes/issues/9), [#8](https://github.com/brianbennett/fpga_nes/issues/8), [#7](https://github.com/brianbennett/fpga_nes/issues/7), [#6](https://github.com/brianbennett/fpga_nes/issues/6), [#3](https://github.com/brianbennett/fpga_nes/issues/3)
- Denghongcai'nin COM port çözümü: [commit](https://github.com/denghongcai/fpga_nes/commit/021a890721ca7dfa8bf397dcce3d0221d1f0095a)
- Bakımcı: [brianbennett](https://github.com/brianbennett) — büyük ölçüde terk etmiş
