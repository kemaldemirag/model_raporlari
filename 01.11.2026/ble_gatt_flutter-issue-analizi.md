# dubemezeagwu/ble_gatt_flutter: Tüm Issue Analizi

**Repo:** [dubemezeagwu/ble_gatt_flutter](https://github.com/dubemezeagwu/ble_gatt_flutter)
**Toplam issue:** 8 (0 açık, 8 kapalı)
**Sahip:** [dubemezeagwu](https://github.com/dubemezeagwu) (Dubem Ezeagwu, Ontario, Kanada; @decadevs)
**Yıldız / Fork:** 1 / 0
**Diller:** Dart (116.7k satır ağırlığında), az miktarda Ruby, HTML, Swift, Kotlin, Objective-C
**Rapor tarihi:** 1 Kasım 2026

Bu rapor, listenin beşinci sırasındaki ble_gatt_flutter reposundaki tüm issue'ları analiz eder.

## En kritik tespit: bunlar gerçek topluluk issue'ları değil

Çekilen veriler net bir sonuç veriyor: **8 "issue"nun tamamı bizzat repo sahibi (dubemezeagwu) tarafından açılmış ve her biri aynı numaralı bir PR ile birebir eşleşiyor.** Issue gövdeleri boş; başlıklar kişisel geliştirme kilometre taşları:

| Issue | Başlık | Açılma | Tuvalete karşılık gelen PR |
|---|---|---|---|
| [#8](https://github.com/dubemezeagwu/ble_gatt_flutter/issues/8) | "Dev" | 22 Haziran 2026 | dev dalından merge: isolate katmanı refactoring |
| [#7](https://github.com/dubemezeagwu/ble_gatt_flutter/issues/7) | "chore: Update project exclusions" | 3 Haziran 2026 | dev dalından merge |
| [#6](https://github.com/dubemezeagwu/ble_gatt_flutter/issues/6) | "main: fix on android permission for version 12+" | 22 Mayıs 2026 | Android 12+ BLE tarama izni düzeltmesi |
| [#5](https://github.com/dubemezeagwu/ble_gatt_flutter/issues/5) | "main: final cleanup and validation before submission" | 21 Mayıs 2026 | Teslim öncesi temizlik |
| [#4](https://github.com/dubemezeagwu/ble_gatt_flutter/issues/4) | "main: app full functionality and main user flow achieved" | 18 Mayıs 2026 | Tam işlevsellik |
| [#3](https://github.com/dubemezeagwu/ble_gatt_flutter/issues/3) | "main: major implementations and fixes done (camera + persistence + isolate)" | 15 Mayıs 2026 | Kamera + kalıcılık + isolate |
| [#2](https://github.com/dubemezeagwu/ble_gatt_flutter/issues/2) | "progress 2: foundation + UI scaffolding and full BLE integration" | 13 Mayıs 2026 | Temel + BLE entegrasyonu |
| [#1](https://github.com/dubemezeagwu/ble_gatt_flutter/issues/1) | "docs: added debugging tips to claude.md" | 8 Mayıs 2026 | Dokümantasyon |

Yani bu repo, klasik anlamda bir "issue takibi" içermiyor: **hata bildirimi, özellik talebi veya topluluk sorusu yok.** Issue'lar, tek kişilik bir geliştirme akışının (dev dalı → main merge) kayıtları. Ayrıca 0 fork, 1 yıldız ve hiçbir dış katkıcı yok.

## Proje ne yapıyor?

README'den: **BLE Data Recorder** — Flutter ile geliştirilmiş, gerçek zamanlı BLE sensör verisi kayıt uygulaması (öncelikli olarak Android):

- **Nordic UART Service (NUS)** cihazlarına BLE bağlantısı
- Canlı grafikle gerçek zamanlı veri görselleştirme
- Veri oturumları sırasında **senkronize video kaydı**
- JSON (ham veri) ve MP4 (video) olarak dışa aktarma
- CPU-yoğun işlemi UI iş parçacığından ayıran **worker isolate**
- Üstel geri çekilme ile otomatik yeniden bağlanma
- Flutter 3.32.8, Dart 3.8.1, minSdk 23, hedef SDK 28; test cihazı Infinix X652A

Geliştirme süreci, commit mesajlarından anlaşıldığı kadarıyla yapay zekâ destekli araçlarla yürütülmüş (CLAUDE.md, "claude debugging tips" — bunlardan biri issue #1). "Masitek BLE Data Recorder" adından "LE Data Recorder"a yeniden adlandırılmış (Ağustos 2026).

## Katkı açısından değerlendirme

**Katkı fırsatı yok.** Nedenleri:

1. **Açık issue yok** — katkı talebi karşılayacak bir talep defteri yok.
2. **Tek kişilik akış** — herkes kendi PR'ını kendi issue'suyla eşleştiriyor; topluluk süreci mevcut değil.
3. **Uzmanlık uyumsuzluğu** — Flutter/Dart mobil uygulama geliştirme, senin ilgi alanların (FPGA/ASIC, RTL, gömülü C, KiCad/PCB, robotik) ile örtüşmüyor. Tek kesişim noktası BLE protokolü; ama o da burada uygulama katmanında (FlutterBlue), sensör/donanım katmanında değil.
4. **Lisans yok** — README ve repo'da lisans dosyası bulunmuyor; katkı ve yeniden kullanım hakları belirsiz.

## Öneri

Bu repoyu **listeden çıkar**. "Katkı yaptığım repolar" listesinin amacı katkı hedeflerini takip etmekse, bu repo o amaca uymuyor. Yerine, aynı BLE donanım alanında gerçekten katkıya açık bir hedef düşünülebilir — örneğin önceki araştırmada bulunan [bahildebrand/soil-moisture-pcb](https://github.com/bahildebrand/soil-moisture-pcb) (KiCad, sensör) veya BLE donanım tarafıyla ilgileniyorsan Nordic nRF SDK tabanlı açık kaynak gömülü projeler.

## Kaynaklar

- [dubemezeagwu/ble_gatt_flutter](https://github.com/dubemezeagwu/ble_gatt_flutter) — repo ve README
- Issue'lar: [#1](https://github.com/dubemezeagwu/ble_gatt_flutter/issues/1)–[#8](https://github.com/dubemezeagwu/ble_gatt_flutter/issues/8) (hepsi kapalı, hepsi sahibine ait)
- Sahip: [dubemezeagwu](https://github.com/dubemezeagwu)
