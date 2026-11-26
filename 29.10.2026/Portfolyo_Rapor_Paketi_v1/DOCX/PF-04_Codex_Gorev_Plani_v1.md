# PF 04 Güç devresi inceleme örneği

Codex görev planlama raporu

Kemal Demirağ için hazırlanmıştır. Muhatap: çalışma alanını kuracak ve seçili örneği geliştirecek Codex.

Bu plan yalnız gösterilebilir ilk örneği üretir. Görevler T00 ile T06 sırasında yürütülür; karşılanmayan bir önkoşul bağımlı görevi durdurur. Bağımsız işler devam edebilir. Bu belgedeki komutlar hedef arayüzdür; mevcut bir uygulamanın çalıştığı iddia edilmez.

**Repo** Portfolyo/29.10.2026/002-Proje Çıktıları/power-circuit-review-case/

**Ortam** Python hesap betiği, PDF/şema görüntüleme aracı ve gerektiğinde KiCad. SPICE modeli ve fiziksel donanım ilk portföy sürümünün zorunlu bağımlılığı değildir.

**Başlama koşulu** Tek bölüm, net sınırları, kaynak revizyonu ve tam parça numaraları bulunmalı. Kaynak doğrulanana kadar yalnız sentetik hesap örneği çalıştırılabilir.

## Az sayıda dosyayla kurulacak yapı

| **Konum** | **Görevi** |
| --- | --- |
| reference/ | Kaynak revizyonu, şema kesiti ve veri sayfası bağlantıları |
| inputs/ | Parça koşulları ve açıkça varsayım olarak tanımlanmış yük durumları |
| analysis/ | Kayıp ve gerilim düşümü hesapları |
| demo/ | İnceleme PDF dosyası ve sonuç tablosu |
| tests/ | Sentetik hesap örnekleri ve girdi doğrulaması |

Ortak dosyalar: README.md, sources.lock.json, docs/case-study.md ve küçük bir tests alanı. Büyük test videoları ve ayrıntılı ham kayıtlar repo dışında 003-Proje Kayıtları içinde tutulur. Release manifesti bu kayıtların konumunu ve hashlerini gösterir.

### PF 04 T00 Devre bölümünü doğrula

**Bağımlılık** Yok. Girdi: LibreSolar URL.

**Yapılacak değişiklik** reference/source.json ve işaretli şema kesitini oluştur; commit, sayfa, net ve parçaları kaydet.

**Kabul** Gerçek bölüm ve tam parça numaraları kaynakta bulunur.

**Sorun halinde** Bölüm doğrulanamazsa kaynak bazlı analiz BLOCKED; sentetik hesap işi devam edebilir.

### PF 04 T01 Girdi tablosunu hazırla

**Bağımlılık** T00 veya sentetik dal. Girdi: Şema, veri sayfaları ve ek hesap örneği.

**Yapılacak değişiklik** inputs/component\_conditions.csv içine değer, birim, sıcaklık, gate koşulu, tipik/maksimum ayrımı ve kaynağı yaz.

**Kabul** Gerçek veri ile varsayımlar ayrı satır türünde; eksikler null.

**Sorun halinde** Tipik değeri garanti edilen sınır olarak kullanma.

## Çalışan örneğin üretilmesi

### PF 04 T02 İletim hesabını uygula

**Bağımlılık** T01. Girdi: Akım ve direnç girdileri.

**Yapılacak değişiklik** analysis/calculate.py ile P=I²R ve V=IR hesapla; birimleri ortak biçime getir.

**Kabul** Sentetik 1/3/5 A örnekleri sırasıyla 0,02/0,18/0,50 W verir.

**Sorun halinde** Bu sonuçları toplam anahtarlama kaybı veya gerçek sıcaklık diye etiketleme.

### PF 04 T03 Eksik bilgi ve duyarlılık

**Bağımlılık** T02. Girdi: Girdi koşulları.

**Yapılacak değişiklik** Eksik değer, negatif direnç ve bilinmeyen birimleri reddet; I ve R duyarlılığını üret.

**Kabul** A03 ile A05 sağlanır; termal koşul eksikse UNKNOWN açıkça görünür.

**Sorun halinde** Kaynak dışı theta veya sıcaklık sınırı uydurma.

### PF 04 T04 İnceleme raporunu oluştur

**Bağımlılık** T03 ve gerçek kaynak için T00. Girdi: Şema kesiti, hesap ve kaynaklar.

**Yapılacak değişiklik** scripts/run\_demo.py ile demo/review.pdf üret; bölüm, koşullar, hesaplar, açık bulgular ve ölçüm planını ekle.

**Kabul** Her bulgu ilgili bileşen ve dayanağa bağlanır; gözlem ile çıkarım ayrıdır.

**Sorun halinde** Yalnız sentetik veri varsa dosyayı hesap demosu olarak adlandır.

### Uygulama kararları

İletim kaybı hesabı yalnız seçili koşuldaki direnç için geçerlidir. Anahtarlama kaybı, gate sürme ve termal montaj bilinmeden toplam kayıp veya sıcaklık sonucu çıkarma. Sentetik örneğin doğru çıkması gerçek devrenin teknik kabulü değildir.

## Kabul ve portföy teslimi

### PF 04 T05 Hesap ve içerik kabulü

**Bağımlılık** T04. Girdi: A01 ile A06.

**Yapılacak değişiklik** Bağımsız elle hesapla sayıları karşılaştır; şema referanslarını kaynakla kontrol et.

**Kabul** Sayısal doğrulama kaydı ve teknik inceleme durumu ayrı alanlarda.

**Sorun halinde** Teknik değerlendirme yapılmadıysa bunu PASS alanına dönüştürme.

### PF 04 T06 Portföy anlatımını tamamla

**Bağımlılık** T05. Girdi: İnceleme dosyası ve doğrulama kaydı.

**Yapılacak değişiklik** README ve kısa vaka çalışmasında devre kapsamını, katkıyı ve yapılmamış ölçümleri yaz.

**Kabul** Kaynak bazlı inceleme veya yalnız hesap demosu olduğu ilk ekranda anlaşılır.

**Sorun halinde** Kamera güç kartı veya BMS tasarımı tamamlandı şeklinde geniş iddia kurma.

### Çalıştırma sözleşmesi

python scripts/run\_demo.py --out demo<br>python -m pytest -q

Komutlar T00 ortam kararına göre tamamlanır. run\_demo geliştirme sırasında oluşturulur; bugün hazır değildir. --out gösterim kökünü belirtir; PF-01 üretim dosyaları bunun output alt dizinine gider. Çıkış kodları: 0 başarılı işlem, 2 beklenen veri/kabul sorunu, 1 araç hatası. Genel demo, bozuk örnekleri beklendiği gibi reddettiğinde başarılı olabilir; her örneğin sonucu ayrı kalır.

### Her görev sonunda bırakılacak kayıt

task\_id, status, input\_hashes, changed\_files, command, exit\_code, expected, observed ve next\_task alanlarını içeren kısa bir görev kaydı bırak. Komut çalıştırılmadıysa NOT\_RUN; gerekli girdi yoksa BLOCKED yaz. Kayıt tamamlandığında kapsam içindeki sıradaki göreve geç; her adımda yeniden izin isteme.

### Codex için ilk görev metni

PF-04 için bu Görev Planı ile eşleşen İş Akışı raporunu oku. Portfolyo/29.10.2026 içindeki manifestten repo yolunu bul. Aday kartta yüksek taraftan yük anahtarlama bölümünü bul. Şema sayfasını, net sınırlarını, parça referanslarını ve tam parça numaralarını kaydet; bulunamazsa yalnız sentetik hesap kısmını tamamla. İlk çıktı T00 kaydı ve kaynak kilidi olsun. Bu turda T00 sonucuna göre T01'e geç; engel varsa dosya ve gerekçeyi açıkça yaz. Önceden var olan kullanıcı dosyalarını koru.

**Bitiş koşulu** İş Akışı raporundaki A01 ile A06 kontrolleri sonuçlanmış; demo açılmış; kısa vaka çalışması kaynak ve sınırları belirtmiş; yeniden çalıştırma kaydı var. Yalnız test sayısı veya klasörlerin varlığı tamamlanma ölçütü değildir.
