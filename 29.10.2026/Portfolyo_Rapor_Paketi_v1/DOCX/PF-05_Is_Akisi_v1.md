# PF 05 Batarya test verisi raporlama

İş akışı raporu

Kemal Demirağ için hazırlanmıştır. Muhatap: çalışma alanını kuracak ve seçili örneği geliştirecek Codex.

Aynı sentetik deşarj deneyini iki CSV biçiminden okuyup ortak veriye, kalite raporuna ve doğrulanabilir kapasite sonucuna dönüştürmek. Bu rapor, örneği yaptırırken hangi sırayla ilerleyeceğimizi ve teslimi nasıl kontrol edeceğimizi tanımlar.

**Gösterilecek sonuç** İki farklı dosyanın aynı 0,5 Ah sonucuna ulaştığı ve hatalı verinin geçerli sonuç olarak sunulmadığı tek HTML raporunda görülsün.

**İlk sürüm** Tek hücre, tek kesintisiz deşarj deneyi ve iki açıkça tanımlanmış sentetik CSV biçimi. Ticari cihaz formatı desteği, BMS, SoH tahmini, bulut ve tam ontoloji entegrasyonu bu sürümde yer almaz.

## Başlangıç girdileri

**Biçim A** Ek PF-05/format\_a.csv: 0 ile 1800 s arasında 1801 örnek; akım -1 A; gerilim 4 V.

**Biçim B** Ek PF-05/format\_b.csv: aynı deney; zaman ms, akım +1000 mA, gerilim 4000 mV; pozitif işaret deşarj.

**Metadata ve beklenenler** Her biçim için metadata JSON, expected.json ve seçilmiş hatalı örnekler pakette hazırdır.

## Üretim sırası

### 1 Girdi sözleşmesini oku

Girdi: Hazır CSV, metadata ve beklenenler. Çıktı: Sütun, birim, kimlik ve işaret eşlemesi. Geçiş koşulu: A/B aynı fiziksel deneyi temsil ediyor.

### 2 Normalleştir ve kontrol et

Girdi: Ham dosya ve metadata. Çıktı: Kaynak satırını koruyan standart veri ve bulgular. Geçiş koşulu: Birim tahmini ve sessiz düzeltme yapılmıyor.

### 3 Kapasiteyi hesapla

Girdi: Kontrol edilmiş kesintisiz veri. Çıktı: Yöntemi ve geçerliliği yazılmış kapasite. Geçiş koşulu: 0,5 Ah bağımsız beklenenle eşleşiyor.

### 4 Hataları ve örneği sun

Girdi: Temel ve hatalı örnekler. Çıktı: HTML raporu ve tekrar çalıştırma kaydı. Geçiş koşulu: Geçersiz deneyler geçerli toplam sonuç almıyor.

## Teslimin kabulü ve gösterimi

| **Kod** | **Deneme** | **Beklenen sonuç** |
| --- | --- | --- |
| A01 | İki eşdeğer biçim | A ve B aynı standart veriyi ve 0,5 Ah değerini verir; tolerans 1e-9 Ah. |
| A02 | Eksik birim | Birim metadata içinde yoksa INVALID; kapasite null. |
| A03 | Zaman sorunu | Geri giden veya aynı zamana ait çelişkili kayıtlar satır numarasıyla görünür; sessiz sıralama yok. |
| A04 | Geçersiz sayı | Boş, NaN veya sayısal olmayan akım geçerli sonuca dönüşmez. |
| A05 | Veri boşluğu | 1,5 s üzerindeki aralık integral dışında kalır; tam kapasite null ve durum INCOMPLETE. |
| A06 | Kimlik ve tekrar | Hücre/deney kimliği zorunlu; aynı girdi ve ayarlar aynı sayısal sonuçları üretir. |

**Kemal tarafından kontrol** Raporu aç; bir doğru ve bir hatalı örneği çalıştır; görünen sonucun beklenenle neden eşleştiğini kendi cümlelerinle açıkla. Test sonucu anlaşılmıyorsa vaka çalışmasına doğrulandı yazma.

### Üç dakikalık gösterim

1. A ve B dosyalarını aynı komutla çalıştır; 0,5 Ah sonuçlarını karşılaştır.

2. Birim eksik örneği aç; sonucu ve ilgili metadata alanını göster.

3. Veri boşluğu örneğini aç; tam kapasitenin verilmediğini göster.

4. Kaynak dosya hash değerini ve kullanılan ayarları raporda göster.

**Müşteri anlatımı** İki tanımlı sentetik batarya test biçimi için veri kontrolü ve kapasite raporlama örneği.

**Başvuru ilişkisi** Batarya test dosyası dönüştürme, laboratuvar veri kontrolü ve Python raporlama işleri için örnek. Birebir batarya ilanı henüz doğrulanmadı; veri kalite kontrolü ilanlarına aktarılabilir beceri gösterir.

**Sınır** Gerçek hücre deneyine veya ticari test cihazına ait doğrulama yapılmadı. Kapasite hesabı bataryanın güvenliğini, ömrünü veya BMS doğruluğunu kanıtlamaz.

### Dosya yerleşimi

Raporlar: 001-Kayıt Notları ve Dokümantasyon/PF-05/. Uygulama: 002-Proje Çıktıları/battery-test-data-validator/. Gerçek çalıştırma kayıtları: 003-Proje Kayıtları/PF-05/<run-id>/. README, bir demo raporu, kısa vaka çalışması ve gerekli test kaydı yeterlidir.

### Kaynaklar ve inceleme sınırı

Battery Data Toolkit ortak veri yapısı için adaydır; API ve sürüm ilk görevde incelenir. v1 için en küçük yerel CSV işlem hattı yeterlidir; uyarlayıcı eklenirse sabit sürüm kullanılır. domain-battery kavram referansıdır; JSON-LD ve SHACL bu sade sürümün zorunlu teslimi değildir.

Battery Data Toolkit — https://github.com/ROVI-org/battery-data-toolkit

Battery domain kavram kaynağı — https://github.com/emmo-repo/domain-battery

İlan bağlantısı: https://www.upwork.com/freelance-jobs/apply/Data-Engineer-Video-Sensor-Pipeline-for-Robot-Training-Data\_~022115542279426878609/

Erişim tarihi 29.10.2026. İlanın güncel başvuru durumu doğrulanmadı. Kaynak ana sayfası incelemesi; kodun derlendiği veya testlerin geçtiği anlamına gelmez.
