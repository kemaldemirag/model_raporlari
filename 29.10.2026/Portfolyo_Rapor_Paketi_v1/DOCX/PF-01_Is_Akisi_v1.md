# PF 01 PCB kontrol ve üretim teslimi

İş akışı raporu

Kemal Demirağ için hazırlanmıştır. Muhatap: çalışma alanını kuracak ve seçili örneği geliştirecek Codex.

Bir açık kaynak kartın dosyalarından incelemeye açık bir üretim paketi ve bulgu özeti üretmek. Bu rapor, örneği yaptırırken hangi sırayla ilerleyeceğimizi ve teslimi nasıl kontrol edeceğimizi tanımlar.

**Gösterilecek sonuç** Müşteri BOM dosyasını, kartın etkileşimli montaj görünümünü, üretim çıktılarının listesini ve açık bulguları aynı örnekte inceleyebilsin.

**İlk sürüm** Tek kart, tek donanım revizyonu ve tek montaj varyantı. Mevcut tasarımın otomatik kontrolleri ve dosya tutarlılığı ele alınır. Kartı yeniden tasarlama, panelleme, RF onayı ve fiziksel çalışabilirlik değerlendirmesi bu sürümün dışında kalır.

## Başlangıç girdileri

**Kaynak kart** OLIMEX ESP32-S3-DevKit-LiPo adaydır. Yerel dosya yolu ve commit henüz belirlenmedi; kaynak biçimi ilk görevde doğrulanacak.

**Teslim sözleşmesi** Ek PF-01/release\_contract.json; gerekli dosyalar ve eksik çıktı davranışı tanımlıdır.

**Araçlar** KiCad ile uyumlu KiBot ve InteractiveHtmlBom sürümleri kaynak dosyası açıldıktan sonra sabitlenir.

## Üretim sırası

### 1 Kaynağı seç ve kaydet

Girdi: Aday kart ve teslim sözleşmesi. Çıktı: Dosya envanteri, lisans notu, commit ve sürüm kaydı. Geçiş koşulu: Kart açılıyor; dosyalar aynı revizyonda.

### 2 Başlangıç kontrollerini al

Girdi: Sabitlenmiş kart. Çıktı: Ham ERC ve DRC kayıtları ile başlangıç bulgu listesi. Geçiş koşulu: Bulgular gizlenmeden rapora bağlanıyor.

### 3 Teslimi üret

Girdi: Kart ve sürümle uyumlu ayarlar. Çıktı: Gerber, drill, BOM, konum dosyaları ve HTML görünümü. Geçiş koşulu: Sözleşmedeki gerekli roller dolu.

### 4 Çıktıyı sına ve sun

Girdi: Temel teslim ve iki hatalı kopya. Çıktı: Doğrulama kaydı ve kısa vaka çalışması. Geçiş koşulu: A01 ile A06 arasındaki kontroller sonuçlandı.

## Teslimin kabulü ve gösterimi

| **Kod** | **Deneme** | **Beklenen sonuç** |
| --- | --- | --- |
| A01 | Temel teslim | Sözleşmedeki tüm gerekli çıktılar mevcut, açılabilir ve doğru revizyona bağlı. |
| A02 | BOM ve konum | Montaja girecek referanslar eşleşir; DNP ve THT politikası açıkça yazılır. |
| A03 | Eksik dosya | Kopya teslimden drill dosyası silindiğinde eksik çıktı raporlanır ve başarı kodu dönmez. |
| A04 | Çelişkili referans | Kopya BOM içindeki olmayan parça referansı tespit edilir. |
| A05 | Mevcut ERC ve DRC | Başlangıç bulguları saklanır; istisna varsa kimlik ve gerekçeyle görünür kalır. |
| A06 | Tekrar üretim | İkinci temiz çıktı dizininde dosya listesi ve anlamlı içerik eşleşir; zaman damgası farkları açıklanır. |

**Kemal tarafından kontrol** Raporu aç; bir doğru ve bir hatalı örneği çalıştır; görünen sonucun beklenenle neden eşleştiğini kendi cümlelerinle açıkla. Test sonucu anlaşılmıyorsa vaka çalışmasına doğrulandı yazma.

### Üç dakikalık gösterim

1. review.html içindeki kapsamı, kaynak kart sahibini ve açık bulguları göster.

2. assembly.html üzerinde seçilen bir referansı BOM satırıyla eşleştir.

3. Kopya teslimden bir dosya çıkarıp doğrulayıcının hatayı yakaladığını göster.

4. Aynı işlemi temiz çıktı dizininde tekrar çalıştır ve yeni kayıt konumunu göster.

**Müşteri anlatımı** Açık kaynak bir kart için üretim dosyası hazırlama ve teslim tutarlılığı kontrolü örneği.

**Başvuru ilişkisi** Arı kovanı telemetrisinin üretim dosyası teslimi ve sensör PCB ilanının dosya hazırlama bölümüyle ilişkili. nRF52840 tasarım incelemesine yalnız otomatik kontrol ve rapor düzeni açısından destek olur.

**Sınır** Bu çalışma kartın elektriksel çalışmasını, RF performansını veya üretime kesin uygunluğunu kanıtlamaz. Kaynak tasarım sahipliği açıkça belirtilir.

### Dosya yerleşimi

Raporlar: 001-Kayıt Notları ve Dokümantasyon/PF-01/. Uygulama: 002-Proje Çıktıları/pcb-release-review-workbench/. Gerçek çalıştırma kayıtları: 003-Proje Kayıtları/PF-01/<run-id>/. README, bir demo raporu, kısa vaka çalışması ve gerekli test kaydı yeterlidir.

### Kaynaklar ve inceleme sınırı

OLIMEX ana sayfasında HARDWARE alanı ve farklı varlıklar için farklı lisanslar görülmüştür. Bu raporda kartın yerel KiCad dosyaları açılmadı. KiBot ve InteractiveHtmlBom araç adaylarıdır; sürüm uyumu çalıştırılarak doğrulanacak.

OLIMEX kaynak kart — https://github.com/OLIMEX/ESP32-S3-DevKit-LiPo

KiBot — https://github.com/INTI-CMNB/KiBot

InteractiveHtmlBom — https://github.com/openscopeproject/InteractiveHtmlBom

İlan bağlantısı: https://drive.google.com/file/d/13m44Rl88cDkfZytds0OdmTK6W\_S67LqB/view

Erişim tarihi 29.10.2026. İlanın güncel başvuru durumu doğrulanmadı. Kaynak ana sayfası incelemesi; kodun derlendiği veya testlerin geçtiği anlamına gelmez.
