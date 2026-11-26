# 80x86 Projesi Issue Analizi ve Katkı Planı

**Kaynak:** Görseldeki issue listesi ([jamieiles/80x86](https://github.com/jamieiles/80x86))
**Tarih:** 1 Kasım 2026

Bu rapor, 80x86 projesinin açık issue'larını **tam metin, yorumlar ve bağlamla** değerlendirir. Her issue için katkı yapmanın teknik zorluğunu, öğrenme değerini ve senin kariyer hedeflerinle uyumunu analiz eder.

## Yüksek öğrenme değeri, orta-yüksek zorluk

### [#13 GateMate FPGA desteği](https://github.com/jamieiles/80x86/issues/13)

**Talep sahibi:** Lothar (Bulgaristan'daki bir dev kartı kullanıyor). Bulgar bir dev kart üreticisi değerlendirme kartlarını yaklaşık 50€'ya satıyor; Lothar bu tasarımı 8086 FPGA silikonu olarak kullanmak istiyor. Cologne Chip GateMate için açık kaynak bir ILA (çip içi mantık analizörü) yapmış, ama kartta FTDI olmadığı için bu ILA'yı kullanamamaktan emin olamıyor. Kartta 4096 renkli VGA ve PS/2 klavye var; Lothar ayrıca bir fare portu eklemek istiyor.

**Teknik bağlam:** GateMate, Cologne Chip'in görece yeni bir açık kaynak FPGA çip ailesi. 80x86 RTL'si `rtl/` altında SystemVerilog modülleri içeriyor: `Core.sv`, `CSIPSync.sv`, `Divider.sv`, `Fifo.sv`, `Flags.sv`, `IP.sv`, `ImmediateReader.sv`, `InsnDecoder.sv`, `LoadStore.sv`, `LoopCounter.sv`, `MemArbiter.sv`, `ModRMDecode.sv`, `Prefetch.sv`, `SegmentOverride.sv`, `SegmentRegisterFile.sv`, `TempReg.sv` ve `alu/`, `microcode/` alt dizinleri. `fpga/` altında referans tasarımlar var: `de0-nano`, `de0-cv`, `hdp001`, `VGA`, `bios`, `pic`, `ps2`, `sdram`, `spi`, `uart`, `leds`, `irq`, `timer`, `common`.

**Katılım planı:** Bu, kariyer hedefin için **en yüksek öğrenme değerine sahip** iş. Yeni bir FPGA hedefine RTL taşımayı öğrenirsin. Ayrıca açısal ortaklar sentez, zamanlama, kısıtlama dosyaları ve GDSII üretimi öğrenilir. Somut adımlar:
1. Cologne Chip [GateMate docs](https://www.colognechip.com/gatemate) ve [GateMate open source](https://github.com/colognechip/gatemate) reposunu incele.
2. `fpga/de0-cv`'yi örnek alıp `fpga/gatemate/` altında yeni bir referans tasarımı oluştur.
3. 80x86 RTL'sini GateMate sentezine sok; Xilinx/Altera'ya özgü PLL, block RAM ve JTAG altyapısını değiştir.
4. Kartın 4096 renkli VGA ve PS/2 klavye modüllerini bağla.
5. Çıktıyı kendi hesabında `80x86-gatemate-port` reposu olarak paylaşıp `README`'de port deneyimini ayrıntılı yaz.

**Zorluk:** Yüksek. Ama bu senin FPGA/ASIC kariyer hedefiyle en uyumlu iş.

### [#15 80186 bacak eşleme tablosu](https://github.com/jamieiles/80x86/issues/15)

**Talep sahibi:** Ryan. Bir 80186 CPU'lu proje için bu tasarımı kullanmak istiyor. Bazı sinyaller açık: `nmi = NMI`, `irq[0] = INT0`, `data_m_data_in[15:0] & data_m_data_out[15:0] = AD0-AD15`. Ama `DRQ0/DRQ1`, `TEST`, `BHE` (`data_m_bytesel`'in yarısı mı?) belirsiz. Orijinal CPU spesifikasyonuna dayanan bir sinyal efsanesi, ters mühendislik çabasını büyük ölçüde azaltırdı.

**Teknik bağlam:** 80186 orijinal bacakları [Intel MCS-86 datasheet](https://www.intel.com/content/dam/www/programmable/en/documents/80186-80188-datasheet.pdf)'te tanımlı. RTL tarafında `Core.sv` bu sinyalleri yönetiyor.

**Katılım planı:** Dokümantasyon katkısı; yüksek öğrenme değeri ve düşük teknik risk. Orijinal datasheet'i okuyup `rtl/Core.sv`'nin sinyallerini gerçek bacaklara eşleyen bir `docs/pin-mapping.md` yaz. Bu hem `80x86` mimarisini derinlemesine öğrenmeni sağlar hem de katkı en hızlı kabul edilme ihtimaline sahip iş.

**Zorluk:** Orta. Ama mimari öğrenimi için çok değerli.

### [#14 asciidoctor-pdf kurulum hatası](https://github.com/jamieiles/80x86/issues/14)

**Talep sahibi:** s80x86'ı denemek isteyen bir kullanıcı. Ortamı kurmaya çalışırken `asciidoctor-pdf` kurulamıyor. `public_suffix`, Ruby 3.0 gerektiriyor, ama Ruby 2.7 kullanılıyor.

**Teknik bağlam:** Bu, projenin Docker ortamındaki bağımlılık sürüm uyumsuzluğu. Dockerfile veya `scripts/` içindeki Ruby sürümü güncellenmesi gerekiyor.

**Katılım planı:** En kolay başlangıç işi. Dockerfile'daki Ruby sürümünü güncelle ve `public_suffix` bağımlılığını sabitle. Ama öğrenme değeri düşük; yalnızca Docker pratik kazandırır.

**Zorluk:** Düşük. İlk PR'ın için uygun ama kariyer değerine katkısı sınırlı.

## Orta öğrenme değeri, düşük zorluk

### [#11 Belirsiz yapı sistemi](https://github.com/jamieiles/80x86/issues/11)

**Talep sahibi:** DE0-NANO için projeyi derlemeye çalışan bir kullanıcı. Geliştirme rehberini defalarca okumuş ama bir kritik parçayı kaçırıyor. `./scripts/build` tüm CI testlerini geçiyor ama Quartus projesi ve Verilator sim ortamı için hangi hedeflerin çağrılacağını bilmiyor.

**Çözüm (topluluk tarafından):** [user7182](https://github.com/user7182) 26 Mart 2022'de yorum yazmış:
```
./scripts/build
./scripts/ci/fpga de0-nano
```
Sonra Quartus'ta açılacak bir `Top.qpf` dosyası `_build/fpga-de0-cv/fpga/de0-nano` altında oluşur. Komut satırından nano'yu programlamak için docker ortamına girip `ninja de0-nano-program` çalıştır.

**Katılım planı:** Bu issue **zaten topluluk tarafından çözülmüş**, ama çözüm rehbere taşınmamış. `docs/build-system.md` yazıp bu adımları resmileştirmek katkı olur. Öğrenme değeri orta: Docker, CI ve FPGA sentez akışını öğrenirsin.

**Zorluk:** Düşük.

## Düşük öğrenme değeri, düşük zorluk (tarihsel)

### [#2 Spartan 6 desteği](https://github.com/jamieiles/80x86/issues/2)

**Talep sahibi:** [newinnovations](https://github.com/newinnovations). RTL'yi küçük Xilinx Spartan-6 tabanlı cihazlara (lx9 ve lx16) taşımak istiyor. Kodun Intel'e özgü olup olmadığını ve çalışma miktarını soruyor.

**Cevap (bakımcı [jamieiles](https://github.com/jamieiles)):** Satıcıya özgü kod çok az — yalnızca PLL, block RAM ve sanal JTAG; standart arayüzlere sahip. Build sisteminin Xilinx araç zincirine uyarlanması dışında çok iş gerekmez. Ama Xilinx akışıyla deneyimi yok.

**Topluluk cevabı:** SystemVerilog desteklenmediği için eski Xilinx araç zinciri büyük olasılıkla sorun olacaktır. Yalnızca VHDL ve düz Verilog destekliyor.

**Durum:** Bu issue aslında GateMate taşımasının (#13) atası. Ama 2026'da Spartan-6 çok eski ve Xilinx Vivado her zaman SystemVerilog destekliyor. Artık geçerli değil. GateMate taşımaya odaklan.

### [#8 ve [#9 MCGA sorunları](https://github.com/jamieiles/80x86/issues/8)

**Talep sahibi:** DE0-CV kartı kullanan kullanıcılar. MCGA modu etkinleştirildiğinde SD karta yazma çöküyor (#8). Monkey Island 1 CGA modunda çalışıyor ama MCGA'da başlangıçta kilitleniyor (#9).

**Teknik bağlam:** Bu sorunlar 2019'dan kalma. `sim/display` ve `fpga/VGA` modülleriyle ilgili olabilir. Kartın VGA tespit kısmındaki bir sorun da olabilir. Ama bu sorunlar tam olarak çözülmemiş ve yeniden üretmek için DE0-CV kartına erişim gerekiyor.

**Katılım planı:** Önerilmez. Hata yeniden üretimi için donanım gerekiyor ve sorun 7 yıldır açık. Yerine GateMate taşımasına odaklan.

### [#12 NEC V20 serisi ve x87 FPU desteği](https://github.com/jamieiles/80x86/issues/12)

**Talep sahibi:** İsmi belirsiz. NEC V20 serisi, 286 gerçek mod talimatları, 8080 modu, PC/XT I/O, Weitek 1067 FPU, 80287, NEC FPU'lar, 8087, 80187, Am9511/Intel 8231 APU ve Am9512/Intel 8232 FPU desteği istiyor.

**Durum:** Bu issue aslında bir "yol haritası" talebi. Kapsamı çok geniş ve bakımcının bu yönde bir hedefi yok. Ama RTL'ye yeni talimatlar eklemek (286 gerçek mod talimatları) öğrenme değeri yüksek.

**Katılım planı:** Orta öncelik. Önce 286 gerçek mod talimatlarını `rtl/` altına ekle. Bu, x86 mimarisini derinlemesine öğrenirsin. Ama projenin bakım durumunu göze alırsak bu büyük bir iş.

**Zorluk:** Yüksek. Uzun vadeli bir iş.

## Önerilen öncelik sırası

1. **[#14 asciidoctor-pdf kurulum hatası](https://github.com/jamieiles/80x86/issues/14)** — Hızlı, düşük riskli ilk PR. Ama öğrenme değeri düşük.
2. **[#15 80186 bacak eşleme tablosu](https://github.com/jamieiles/80x86/issues/15)** — Dokümantasyon, mimari öğrenimi için yüksek değer. Portföyün için güzel bir katkı.
3. **[#11 Yapı sistemi belgeleri](https://github.com/jamieiles/80x86/issues/11)** — Topluluk çözümünü rehbere taşı. CI/Docker öğrenimi.
4. **[#13 GateMate FPGA desteği](https://github.com/jamieiles/80x86/issues/13)** — En yüksek öğrenme değeri. FPGA kariyer hedefin için ideal. Uzun vadeli, kendi hesabında port olarak planla.
5. **[#12 NEC V20 serisi ve x87 FPU desteği](https://github.com/jamieiles/80x86/issues/12)** — Uzun vadeli; 286 gerçek mod talimatları ekleme işi. Ama büyük iş.

## Kaynaklar

- [jamieiles/80x86](https://github.com/jamieiles/80x86)
- Açık issue'lar: [#15](https://github.com/jamieiles/80x86/issues/15), [#14](https://github.com/jamieiles/80x86/issues/14), [#13](https://github.com/jamieiles/80x86/issues/13), [#12](https://github.com/jamieiles/80x86/issues/12), [#11](https://github.com/jamieiles/80x86/issues/11), [#9](https://github.com/jamieiles/80x86/issues/9), [#8](https://github.com/jamieiles/80x86/issues/8), [#2](https://github.com/jamieiles/80x86/issues/2)
- Bakımcı etkinliği: [jamieiles](https://github.com/jamieiles) şu an [merv](https://github.com/jamieiles/merv) ve [rxv](https://github.com/jamieiles/rxv) projelerine odaklı; 80x86 yaklaşık 5 yıldır güncellenmiyor.
- Cologne Chip GateMate kaynakları: [GateMate docs](https://www.colognechip.com/gatemate), [GateMate open source](https://github.com/colognechip/gatemate)
