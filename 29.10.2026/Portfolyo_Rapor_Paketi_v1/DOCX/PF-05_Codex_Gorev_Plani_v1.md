# PF 05 Batarya test verisi raporlama

Codex görev planlama raporu

Kemal Demirağ için hazırlanmıştır. Muhatap: çalışma alanını kuracak ve seçili örneği geliştirecek Codex.

Bu plan yalnız gösterilebilir ilk örneği üretir. Görevler T00 ile T06 sırasında yürütülür; karşılanmayan bir önkoşul bağımlı görevi durdurur. Bağımsız işler devam edebilir. Bu belgedeki komutlar hedef arayüzdür; mevcut bir uygulamanın çalıştığı iddia edilmez.

**Repo** Portfolyo/29.10.2026/002-Proje Çıktıları/battery-test-data-validator/

**Ortam** Python sanal ortamı. İlk sürümde CSV ve JSON için standart kütüphane yeterlidir; test aracı ve HTML şablon bağımlılığı kullanılırsa sürümleri kilitlenir.

**Başlama koşulu** Eklerdeki iki CSV, metadata ve expected.json birlikte verilmelidir. Yeni cihaz dosyası eklenmesi ayrı kapsam olarak kaydedilir.

## Az sayıda dosyayla kurulacak yapı

| **Konum** | **Görevi** |
| --- | --- |
| examples/ | İki biçim, metadata, beklenenler ve hatalı örnekler |
| src/ | Okuma, normalleştirme, kontrol ve kapasite hesabı |
| tests/ | Bağımsız beklenenlerle birim ve entegrasyon testleri |
| config/ | Veri boşluğu ve işaret politikası |
| demo/ | report.html, normalized.csv ve findings.json |

Ortak dosyalar: README.md, sources.lock.json, docs/case-study.md ve küçük bir tests alanı. Büyük test videoları ve ayrıntılı ham kayıtlar repo dışında 003-Proje Kayıtları içinde tutulur. Release manifesti bu kayıtların konumunu ve hashlerini gösterir.

### PF 05 T00 Girdi ve kaynak kaydı

**Bağımlılık** Yok. Girdi: Ek PF-05 klasörü ve kaynak URLleri.

**Yapılacak değişiklik** Örnekleri examples altına kopyala; hashlerini kaydet. Bağımlılık seçimini ve gerekiyorsa Toolkit sürümünü yaz.

**Kabul** 1801 satırlık iki örnek ve metadata birbiriyle ilişkili; sentetik etiketi görünür.

**Sorun halinde** Veriyi gerçek laboratuvar kaydı olarak adlandırma.

### PF 05 T01 Bağımsız kabul verileri

**Bağımlılık** T00. Girdi: expected.json ve hata örnekleri.

**Yapılacak değişiklik** Testlerde kullanılacak golden değerleri ayrı tut; 1 A × 1800 s / 3600 hesabını kayıt altına al.

**Kabul** Test bekleneni üretim hesap fonksiyonuyla oluşturulmuyor.

**Sorun halinde** Tüm testleri yalnız sabit akımla sınırlama; değişken akım örneğini de ekle.

## Çalışan örneğin üretilmesi

### PF 05 T02 Okuma ve normalleştirme

**Bağımlılık** T01. Girdi: CSV ve metadata.

**Yapılacak değişiklik** src içinde elapsed\_s, current\_a, voltage\_v alanlarını üret; ortak işaret pozitif şarj, negatif deşarj.

**Kabul** A ve B ortak alanlarda eşleşir; source\_row ve orijinal dosya korunur.

**Sorun halinde** İşareti voltaj trendinden veya sayılardan tahmin etme.

### PF 05 T03 Kontrol ve kapasite

**Bağımlılık** T02. Girdi: Standart veri ve policy.json.

**Yapılacak değişiklik** Kimlik, birim, zaman, sonlu sayı ve boşluk kontrollerini ekle; geçerli aralıklarda trapez yöntemi kullan.

**Kabul** Tam geçerli deşarj 0,5 Ah; gap örneğinde total\_capacity\_ah null.

**Sorun halinde** Eksik noktaları doldurma; zamanı sıralayarak veya tekrarları silerek düzeltme.

### PF 05 T04 Rapor ve tek komut

**Bağımlılık** T03. Girdi: Standart veri, bulgular ve sonuç.

**Yapılacak değişiklik** scripts/run\_demo.py ile normalized.csv, findings.json ve çevrimdışı report.html üret.

**Kabul** Kaynak satırı, hesap yöntemi, durum, hash ve katkı sınırı raporda var.

**Sorun halinde** INVALID veya INCOMPLETE durumunu yeşil başarı olarak sunma.

### Uygulama kararları

Kapasite yalnız monoton artan zamanlı ve sonlu akım içeren aralıklarda hesaplanır. Deşarj akımı ortak biçimde negatiftir. Ardışık noktaların akım büyüklüklerinin ortalaması zaman farkıyla çarpılır ve 3600 ile bölünür. Eşik üstü boşlukta toplam kapasite null; ayrı observed\_capacity\_ah alanı yalnız kapsanan aralıkların toplamını gösterebilir. v1 karışık şarj/deşarj işaretli koşuları reddeder.

## Kabul ve portföy teslimi

### PF 05 T05 Kabul denemeleri

**Bağımlılık** T04. Girdi: A01 ile A06 ve değişken akım örneği.

**Yapılacak değişiklik** Temiz çıktı dizinlerinde tüm örnekleri çalıştır; 0/2/1 çıkış kodlarını ve beklenenleri doğrula.

**Kabul** Geçerli, geçersiz ve eksik veri davranışları ayrı sonuçlanır; tolerans sağlanır.

**Sorun halinde** Aracın kendi hatasını veri hatası diye raporlayıp geçme.

### PF 05 T06 Müşteri gösterimini paketle

**Bağımlılık** T05. Girdi: HTML, örnekler ve test kayıtları.

**Yapılacak değişiklik** README, bir sayfalık vaka çalışması ve demo paketi hazırla; küçük veri dosyalarını ekle.

**Kabul** Başka kullanıcı iki dosyanın eşdeğerliğini ve hata yakalamayı yeniden görebilir.

**Sorun halinde** Gerçek cihaz desteği ve ticari laboratuvar doğrulaması iddiası ekleme.

### Çalıştırma sözleşmesi

python scripts/run\_demo.py --out demo<br>python -m pytest -q

Komutlar T00 ortam kararına göre tamamlanır. run\_demo geliştirme sırasında oluşturulur; bugün hazır değildir. --out gösterim kökünü belirtir; PF-01 üretim dosyaları bunun output alt dizinine gider. Çıkış kodları: 0 başarılı işlem, 2 beklenen veri/kabul sorunu, 1 araç hatası. Genel demo, bozuk örnekleri beklendiği gibi reddettiğinde başarılı olabilir; her örneğin sonucu ayrı kalır.

### Her görev sonunda bırakılacak kayıt

task\_id, status, input\_hashes, changed\_files, command, exit\_code, expected, observed ve next\_task alanlarını içeren kısa bir görev kaydı bırak. Komut çalıştırılmadıysa NOT\_RUN; gerekli girdi yoksa BLOCKED yaz. Kayıt tamamlandığında kapsam içindeki sıradaki göreve geç; her adımda yeniden izin isteme.

### Codex için ilk görev metni

PF-05 için bu Görev Planı ile eşleşen İş Akışı raporunu oku. Portfolyo/29.10.2026 içindeki manifestten repo yolunu bul. Eklerdeki CSV ve metadata dosyalarını olduğu gibi al. Önce bağımsız expected.json değerlerini oku; A ve B için ortak kolon ve işaret sözleşmesini uygula. İlk çıktı T00 kaydı ve kaynak kilidi olsun. Bu turda T00 sonucuna göre T01'e geç; engel varsa dosya ve gerekçeyi açıkça yaz. Önceden var olan kullanıcı dosyalarını koru.

**Bitiş koşulu** İş Akışı raporundaki A01 ile A06 kontrolleri sonuçlanmış; demo açılmış; kısa vaka çalışması kaynak ve sınırları belirtmiş; yeniden çalıştırma kaydı var. Yalnız test sayısı veya klasörlerin varlığı tamamlanma ölçütü değildir.
