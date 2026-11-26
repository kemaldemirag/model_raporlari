# PF 03 ROS 2 IMU sürücüsü doğrulaması

İş akışı raporu

Kemal Demirağ için hazırlanmıştır. Muhatap: çalışma alanını kuracak ve seçili örneği geliştirecek Codex.

Belirli USB IMU sürücüsünün bilinen veri ve iletişim kesintisi karşısındaki ROS mesajlarını tekrarlanabilir biçimde göstermek. Bu rapor, örneği yaptırırken hangi sırayla ilerleyeceğimizi ve teslimi nasıl kontrol edeceğimizi tanımlar.

**Gösterilecek sonuç** Sanal seri kaynaktan gelen veri, oluşan ROS mesajı ve beklenen sonuç aynı raporda karşılaştırılsın.

**İlk sürüm** Tek ROS dağıtımı ve tek protokol biçimi. Varsayılan demo hedefi Ubuntu 24.04 ile ROS 2 Jazzy; ilgili kaynak dalı ve bağımlılıklar ilk görevde doğrulanır. SLAM, sensör füzyonu, robot kontrolü ve fiziksel USB testi kapsam dışıdır.

## Başlangıç girdileri

**Sürücü** rt-net rt\_usb\_9axisimu\_driver; uygun Jazzy dalı ve commit ilk görevde sabitlenecek.

**Protokol** Cihazın varsayılan firmware belgesi ve mevcut test örnekleri; ASCII veya binary arasından desteklenen tek biçim seçilecek.

**Senaryolar** Ek PF-03/scenarios.json; tam paket, parçalı paket, eksik paket, kesinti ve yeniden başlatma.

## Üretim sırası

### 1 Ortamı ve protokolü sabitle

Girdi: Sürücü dalı ve cihaz belgeleri. Çıktı: Sürüm kaydı ve protokol özeti. Geçiş koşulu: Tek dağıtımda derleme ve fixture hazırlığı tamam.

### 2 Sanal cihazı bağla

Girdi: Doğru paket ve başlangıç alışverişi. Çıktı: Gerçek sürücüyü besleyen PTY veya dar adaptör. Geçiş koşulu: Beklenen topic üzerinde mesaj görülüyor.

### 3 Hataları ve toparlanmayı sına

Girdi: Parçalı ve kesintili akışlar. Çıktı: Mesaj ve lifecycle gözlem kayıtları. Geçiş koşulu: Gözlenen davranış ile istenen davranış ayrılıyor.

### 4 Karşılaştırmayı sun

Girdi: Test kaydı ve rapor. Çıktı: HTML mesaj karşılaştırması ve kısa kullanım videosu taslağı. Geçiş koşulu: Gösterim yeni ortamda tekrar çalışıyor.

## Teslimin kabulü ve gösterimi

| **Kod** | **Deneme** | **Beklenen sonuç** |
| --- | --- | --- |
| A01 | Tam paket | Bilinen paket beklenen topic türünü, sayısal değerleri ve birimleri üretir. |
| A02 | Parçalı okuma | Aynı bayt dizisinin farklı parçalara bölünmesi aynı ölçümü verir. |
| A03 | Eksik veya bozuk paket | Yeni geçerli ölçüm oluşmaz; eski değer yeni paket gibi sunulmaz. |
| A04 | Kesinti | Akış durduğunda yayın ve lifecycle davranışı ölçülür; gözlenen sonuç yazılır. |
| A05 | Yeniden başlatma | Belgelenen cleanup/configure/activate yolu veya test edilen yeniden başlatma akışıyla veri geri gelir. |
| A06 | Mesaj sözleşmesi | frame\_id, zaman alanları ve birimler denetlenir; sensörün üretmediği yönelim uydurulmaz. |

**Kemal tarafından kontrol** Raporu aç; bir doğru ve bir hatalı örneği çalıştır; görünen sonucun beklenenle neden eşleştiğini kendi cümlelerinle açıkla. Test sonucu anlaşılmıyorsa vaka çalışmasına doğrulandı yazma.

### Üç dakikalık gösterim

1. Sanal kaynağı çalıştırıp gelen ham paketi ve ROS mesajını karşılaştır.

2. Aynı paketi parçalara bölerek aynı sonucun geldiğini göster.

3. Akışı durdur; yayın ve lifecycle kaydını aç.

4. Belgelenen toparlanma yolunu çalıştırıp yeni ölçüm geldiğini göster.

**Müşteri anlatımı** Belirli bir ROS 2 IMU sürücüsü için donanımsız protokol ve mesaj doğrulama örneği.

**Başvuru ilişkisi** ROS2 Development for Robot ilanındaki sensör entegrasyonu ve hata ayıklama bölümüne destek. Robotun bütün navigasyon ve aktüatör işini karşıladığı iddia edilmez.

**Sınır** Gerçek sensör kalibrasyonu, USB elektriksel davranışı, zaman eşzamanlama doğruluğu veya robot navigasyon başarısı gösterilmiş değildir.

### Dosya yerleşimi

Raporlar: 001-Kayıt Notları ve Dokümantasyon/PF-03/. Uygulama: 002-Proje Çıktıları/ros2-imu-validation-lab/. Gerçek çalıştırma kayıtları: 003-Proje Kayıtları/PF-03/<run-id>/. README, bir demo raporu, kısa vaka çalışması ve gerekli test kaydı yeterlidir.

### Kaynaklar ve inceleme sınırı

README, dağıtım dallarını ve lifecycle davranışını açıklıyor; active durumda iletişim hatasının kendiliğinden durum geçişi yapmadığını belirtiyor. Otomatik toparlanma varmış veya yokmuş gibi varsayılmayacak; seçilen commit üzerinde ölçülecek.

ROS 2 IMU sürücüsü — https://github.com/rt-net/rt\_usb\_9axisimu\_driver

Cihaz ve protokol kaynağı — https://github.com/rt-net/RT-USB-9AXIS-00

İlan bağlantısı: https://www.upwork.com/freelance-jobs/apply/ROS2-Development-for-Robot\_~022098842208730689176/

Erişim tarihi 29.10.2026. İlanın güncel başvuru durumu doğrulanmadı. Kaynak ana sayfası incelemesi; kodun derlendiği veya testlerin geçtiği anlamına gelmez.
