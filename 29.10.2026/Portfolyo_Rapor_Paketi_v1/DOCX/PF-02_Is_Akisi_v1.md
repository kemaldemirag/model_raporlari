# PF 02 Sensör sürücüsü hata testleri

İş akışı raporu

Kemal Demirağ için hazırlanmıştır. Muhatap: çalışma alanını kuracak ve seçili örneği geliştirecek Codex.

SHT4x sürücüsünü bilgisayarda sahte I2C cevaplarıyla çalıştırıp ölçüm ve hata davranışını göstermek. Bu rapor, örneği yaptırırken hangi sırayla ilerleyeceğimizi ve teslimi nasıl kontrol edeceğimizi tanımlar.

**Gösterilecek sonuç** Tek çalıştırmada geçerli ölçüm, CRC hatası, iletişim hatası, eski ölçüm ve toparlanma sonuçları yan yana görülsün.

**İlk sürüm** Tek sensör ailesi ve bilgisayarda C sürücüsü testi. Retry, eski veri ve son geçerli ölçüm politikası bize ait ince bir uygulama katmanında tanımlanır. ESP32 firmware, BLE, bulut, mobil uygulama ve gerçek I2C zamanlaması kapsam dışıdır.

## Başlangıç girdileri

**Sürücü** Sensirion embedded-i2c-sht4x ve mevcut tests alanı; commit ilk görevde sabitlenecek.

**Davranış sözleşmesi** Ek PF-02/scenarios.json; en çok 3 deneme ve 5000 ms tazelik sınırı önerilen demo politikasıdır.

**Protokol örneği** Ham cevap ve CRC beklenen değerleri kaynak kod ile üretici belgesinden doğrulanarak hazırlanacak; şu an ölçüm yakalaması yok.

## Üretim sırası

### 1 Başlangıcı incele

Girdi: Sürücü ve mevcut testler. Çıktı: HAL haritası ve derleme kaydı. Geçiş koşulu: Sürücü başarıyla derlenir; eksik test iddiası incelemeye dayanır.

### 2 Sahte cevabı bağla

Girdi: HAL ve doğrulanmış ham veri. Çıktı: Bilinen girdiden ölçüm üreten host örneği. Geçiş koşulu: Beklenen değer test uygulamasından bağımsız hesaplanır.

### 3 Hata politikalarını uygula

Girdi: Senaryo sözleşmesi. Çıktı: CRC, iletişim, eski ölçüm ve toparlanma durumları. Geçiş koşulu: Eski veri yeni ölçüm olarak görünmez.

### 4 Gösterimi kaydet

Girdi: Altı kabul denemesi. Çıktı: Senaryo özeti ve kısa müşteri açıklaması. Geçiş koşulu: Sonuçlar bir komutla yeniden üretilebilir.

## Teslimin kabulü ve gösterimi

| **Kod** | **Deneme** | **Beklenen sonuç** |
| --- | --- | --- |
| A01 | Doğru cevap | Beklenen ham cevap bağımsız dönüşüm hesabıyla uyuşur. |
| A02 | CRC hatası | Bozuk cevap yeni geçerli ölçüm olarak kaydedilmez; CRC\_ERROR görünür. |
| A03 | İletişim hatası | Sahte HAL hatası en çok 3 denemede sonlanır; sonsuz döngü oluşmaz. |
| A04 | Tazelik | Son başarıdan 5000 ms sonra veri STALE olur; eski değer yeni zamanla işaretlenmez. |
| A05 | Toparlanma | Hata ardından gelen geçerli cevap yeni ölçüm olarak kabul edilir. |
| A06 | Tekrarlanabilirlik | Sahte saatle aynı senaryo dizisi aynı durum ve değerleri verir. |

**Kemal tarafından kontrol** Raporu aç; bir doğru ve bir hatalı örneği çalıştır; görünen sonucun beklenenle neden eşleştiğini kendi cümlelerinle açıkla. Test sonucu anlaşılmıyorsa vaka çalışmasına doğrulandı yazma.

### Üç dakikalık gösterim

1. Geçerli cevap senaryosunu aç ve beklenen/gözlenen değeri göster.

2. CRC baytını değiştir; yeni ölçümün geçersiz olduğuna bak.

3. Sahte saati 5000 ms sınırına getir; eski değerin nasıl işaretlendiğini göster.

4. Hata ardından doğru cevap verip toparlanma kaydını aç.

**Müşteri anlatımı** SHT4x sürücüsü çevresinde donanımsız hata senaryoları ve ölçüm durumu testleri.

**Başvuru ilişkisi** Dar kapsamlı sensör sürücüsü testi ve gömülü C hata yönetimi işleri. Arı kovanı ilanı donanım tasarımı istediğinden bu örnek o ilana yardımcı kanıttır; ESP32 yerel kontrol ilanını tek başına karşılamaz.

**Sınır** Gerçek sensör doğruluğu, fiziksel I2C hattı ve MCU üzerindeki gecikmeler ölçülmüş değildir. Host testi bir HAL çağrısının gerçek donanımda asla bloklanmayacağını kanıtlamaz.

### Dosya yerleşimi

Raporlar: 001-Kayıt Notları ve Dokümantasyon/PF-02/. Uygulama: 002-Proje Çıktıları/sensor-driver-test-lab/. Gerçek çalıştırma kayıtları: 003-Proje Kayıtları/PF-02/<run-id>/. README, bir demo raporu, kısa vaka çalışması ve gerekli test kaydı yeterlidir.

### Kaynaklar ve inceleme sınırı

Kaynak listesinde tests, sensirion\_i2c\_hal.c ve sht4x\_i2c.c görülmüştür. Mevcut testlerin başarısı ve eksikleri bu raporda çalıştırılarak belirlenmedi. Eklenen politikalar upstream özelliği diye sunulmaz.

Sensirion sürücüsü — https://github.com/Sensirion/embedded-i2c-sht4x

Sürücü uygulaması — https://github.com/Sensirion/embedded-i2c-sht4x/blob/master/sht4x\_i2c.c

İlan bağlantısı: https://drive.google.com/file/d/1LwDAVcCARMTVHZfqcJ3HInCDxoi0dYnf/view

Erişim tarihi 29.10.2026. İlanın güncel başvuru durumu doğrulanmadı. Kaynak ana sayfası incelemesi; kodun derlendiği veya testlerin geçtiği anlamına gelmez.
