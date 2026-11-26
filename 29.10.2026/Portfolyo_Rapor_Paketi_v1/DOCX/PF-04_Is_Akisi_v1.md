# PF 04 Güç devresi inceleme örneği

İş akışı raporu

Kemal Demirağ için hazırlanmıştır. Muhatap: çalışma alanını kuracak ve seçili örneği geliştirecek Codex.

Açık bir devrenin tek güç anahtarlama bölümünü kaynakları ve hesapları izlenebilir bir inceleme dosyasına dönüştürmek. Bu rapor, örneği yaptırırken hangi sırayla ilerleyeceğimizi ve teslimi nasıl kontrol edeceğimizi tanımlar.

**Gösterilecek sonuç** İşaretli şema, bileşen tablosu, tekrar çalıştırılabilir kayıp hesabı ve hangi fiziksel ölçümlerin gerektiğini gösteren kısa PDF üretmek.

**İlk sürüm** Başlangıç adayı LibreSolar mppt-2420-hc içindeki yüksek taraftan yük anahtarlama bölümüdür. Gerçek şema sayfası ve parça referansları ilk görevde seçilir. Bütün MPPT, BMS, çoklu kamera güç rayı veya yeni PCB tasarımı yapılmaz.

## Başlangıç girdileri

**Kaynak devre** LibreSolar mppt-2420-hc şema ve BOM dosyaları; doğru revizyon ve bölüm ilk görevde doğrulanacak.

**Parça dayanakları** Seçilen anahtarın tam parça numarası ve üretici veri sayfası; veri sayfası test koşulları kaydedilecek.

**Hesap örneği** Ek PF-04/synthetic\_loss\_cases.csv yalnız hesap mekanizmasını sınar; gerçek devre ölçümü değildir.

## Üretim sırası

### 1 Devre bölümünü sınırla

Girdi: Kaynak şema ve BOM. Çıktı: İşaretli bölüm ve parça listesi. Geçiş koşulu: Her referans gerçek kaynağa bağlı.

### 2 Hesap girdilerini çıkar

Girdi: Veri sayfası ve yük varsayımları. Çıktı: Değer, birim, koşul ve kaynak tablosu. Geçiş koşulu: Varsayım ile üretici sınırı ayrı.

### 3 Hesabı ve duyarlılığı üret

Girdi: Tanımlanmış akım ve direnç. Çıktı: Kayıp ve gerilim düşümü tablosu. Geçiş koşulu: Sentetik kabul örnekleri doğru.

### 4 İncelemeyi sun

Girdi: Şema, hesap ve belirsizlikler. Çıktı: Kısa PDF ve fiziksel ölçüm listesi. Geçiş koşulu: Uygunluk iddiası kanıt sınırını aşmıyor.

## Teslimin kabulü ve gösterimi

| **Kod** | **Deneme** | **Beklenen sonuç** |
| --- | --- | --- |
| A01 | Kaynak izlenebilirliği | Her gerçek parça, net ve hesap girdisi şema veya veri sayfasına bağlanır. |
| A02 | Hesap örneği | 5 A ve 0,02 ohm için iletim kaybı 0,5 W; gerilim düşümü 0,1 V çıkar. |
| A03 | Birim ve eksik bilgi | 20 milliohm ile 0,02 ohm eşdeğer; eksik R değeri tahmin edilmez. |
| A04 | Koşul duyarlılığı | Akım iki katına çıkınca sabit R ile kayıp dört katına çıkar. |
| A05 | Termal sınır | Montaj/soğutma koşulları yoksa birleşim sıcaklığı ve güvenli yük sonucu UNKNOWN kalır. |
| A06 | Rapor tutarlılığı | Şema işaretleri, parça tablosu, hesap ve fiziksel test planı aynı bölümle sınırlı. |

**Kemal tarafından kontrol** Raporu aç; bir doğru ve bir hatalı örneği çalıştır; görünen sonucun beklenenle neden eşleştiğini kendi cümlelerinle açıkla. Test sonucu anlaşılmıyorsa vaka çalışmasına doğrulandı yazma.

### Üç dakikalık gösterim

1. Şema üzerindeki seçili bölümün başlangıç ve bitişini göster.

2. Bir bileşenin parça numarasından veri sayfasındaki kullanılan koşula git.

3. Sentetik 5 A örneğini çalıştır ve 0,5 W sonucunu doğrula.

4. Gerçek bölüm için hesaplanabilen ve ölçüm gerektiren noktaları ayır.

**Müşteri anlatımı** Tek bir açık kaynak güç bölümünün kaynaklı masa başı incelemesi ve hesap örneği.

**Başvuru ilişkisi** Kamera güç kartı ve akım geri beslemeli ısıtıcı ilanlarında hesap ve teknik açıklama ihtiyacına yardımcı örnek. Tek anahtar incelemesi bu kartların bütün tasarım kapsamını karşılamaz.

**Sınır** Fiziksel doğrulama yapılmadı. Sentetik direnç ve akım değerleri gerçek kartın parça veya yük bilgisi değildir. Bağımsız teknik inceleme olmadan güvenli çalışma veya üretime uygunluk hükmü verilmez.

### Dosya yerleşimi

Raporlar: 001-Kayıt Notları ve Dokümantasyon/PF-04/. Uygulama: 002-Proje Çıktıları/power-circuit-review-case/. Gerçek çalıştırma kayıtları: 003-Proje Kayıtları/PF-04/<run-id>/. README, bir demo raporu, kısa vaka çalışması ve gerekli test kaydı yeterlidir.

### Kaynaklar ve inceleme sınırı

Repo tanımı HS load switch içeren MPPT kartını belirtir. Bu raporda şema üzerinden parça ve net isimleri doğrulanmadı. T00 çıktısı olmadan gerçek bileşenlere ilişkin uygunluk hükmü verilemez.

LibreSolar kaynak devre — https://github.com/LibreSolar/mppt-2420-hc

İlan bağlantısı: https://drive.google.com/file/d/1zGjnsaSEpM6I9kB3VZ8pei3sSqQzyXvd/view

Erişim tarihi 29.10.2026. İlanın güncel başvuru durumu doğrulanmadı. Kaynak ana sayfası incelemesi; kodun derlendiği veya testlerin geçtiği anlamına gelmez.
