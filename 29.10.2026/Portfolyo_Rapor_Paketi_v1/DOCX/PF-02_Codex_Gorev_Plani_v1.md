# PF 02 Sensör sürücüsü hata testleri

Codex görev planlama raporu

Kemal Demirağ için hazırlanmıştır. Muhatap: çalışma alanını kuracak ve seçili örneği geliştirecek Codex.

Bu plan yalnız gösterilebilir ilk örneği üretir. Görevler T00 ile T06 sırasında yürütülür; karşılanmayan bir önkoşul bağımlı görevi durdurur. Bağımsız işler devam edebilir. Bu belgedeki komutlar hedef arayüzdür; mevcut bir uygulamanın çalıştığı iddia edilmez.

**Repo** Portfolyo/29.10.2026/002-Proje Çıktıları/sensor-driver-test-lab/

**Ortam** C derleyicisi ve CMake/CTest. Önce mevcut derleme sistemi kullanılır. Sahte HAL ve saat kaynağı ile donanım bekleme süreleri test dışında tutulur.

**Başlama koşulu** Kaynak sürücü bilgisayarda derlenmeli; HAL imzaları ve mevcut testler incelenmeli. Bir doğru ham cevap için bağımsız beklenen değer yazılmalı.

## Az sayıda dosyayla kurulacak yapı

| **Konum** | **Görevi** |
| --- | --- |
| src/ | Ölçüm durumu ve tazelik sarmalayıcısı |
| tests/fakes/ | Sahte I2C ve sahte saat |
| tests/fixtures/ | Kaynağı gösterilen ham cevaplar |
| config/ | scenarios.json ve tazelik politikası |
| demo/ | HTML veya metin senaryo özeti |

Ortak dosyalar: README.md, sources.lock.json, docs/case-study.md ve küçük bir tests alanı. Büyük test videoları ve ayrıntılı ham kayıtlar repo dışında 003-Proje Kayıtları içinde tutulur. Release manifesti bu kayıtların konumunu ve hashlerini gösterir.

### PF 02 T00 Sürücü durumunu kaydet

**Bağımlılık** Yok. Girdi: Kaynak URL ve senaryo sözleşmesi.

**Yapılacak değişiklik** Commit, lisans, derleme komutu ve mevcut tests içeriğini docs/baseline.txt içine yaz.

**Kabul** Mevcut test sonucu veya çalıştırılamama nedeni gerçek kayıtla destekli.

**Sorun halinde** Başarısız upstream testi silme veya yeni çalışmanın başarısı diye sunma.

### PF 02 T01 Sahte HAL ve doğru örnek

**Bağımlılık** T00. Girdi: HAL imzaları ve üretici protokolü.

**Yapılacak değişiklik** tests/fakes içine I2C ve saat taklitleri ekle; kaynaklı doğru cevap fixture dosyasını ve bağımsız beklenen değeri oluştur.

**Kabul** Gerçek sürücü kodu sahte HAL ile çağrılır; yalnız taklit kodun kendisi sınanmaz.

**Sorun halinde** CRC veya dönüşüm bilinmiyorsa fixture uydurma; kaynağı tamamla.

## Çalışan örneğin üretilmesi

### PF 02 T02 Ölçüm durumunu uygula

**Bağımlılık** T01. Girdi: scenarios.json.

**Yapılacak değişiklik** src içinde değer, valid, status, last\_success\_ms alanlarını yöneten ince katman yaz.

**Kabul** Başarı zamanı yalnız başarılı ölçümle değişir; ilk ölçüm yokken NO\_DATA döner.

**Sorun halinde** Eski değerle yeni zaman damgasını birleştirme.

### PF 02 T03 Hata ve tazelik testleri

**Bağımlılık** T02. Girdi: Doğru cevap ve kontrollü hatalar.

**Yapılacak değişiklik** CRC bit çevirme, HAL hata dizisi, en çok 3 deneme, 4999/5000 ms sınırı ve toparlanma testlerini ekle.

**Kabul** A02 ile A05 davranışları sahte saatle deterministik doğrulanır.

**Sorun halinde** İşletim sistemi sleep süresini beklenen sonuç yerine kullanma.

### PF 02 T04 Senaryo özetini üret

**Bağımlılık** T03. Girdi: Test sonuçları.

**Yapılacak değişiklik** scripts/run\_demo.py ile demo/report.html üret; girdi, beklenen, gözlenen, durum ve süreyi göster.

**Kabul** Kaynağı ve sarmalayıcı katkısını ayırır; başarısız senaryolar görünür.

**Sorun halinde** Fiziksel sensör ölçümü yapılmış gibi görüntü veya iddia üretme.

### Uygulama kararları

Tazelik yaşı son başarılı ölçümden itibaren hesaplanır; yaş 5000 ms veya üzerindeyse STALE. CRC hatasında last\_success\_ms değişmez. İlk başarılı ölçüm yoksa NO\_DATA. En çok 3 deneme uygulama politikasıdır; sürücünün kendi politikası diye etiketlenmez.

## Kabul ve portföy teslimi

### PF 02 T05 Temiz ortamda doğrula

**Bağımlılık** T04. Girdi: A01 ile A06 testleri.

**Yapılacak değişiklik** Yeni build dizininde derle; CTest sonuçlarını ve araç sürümlerini evidence kaydına aktar.

**Kabul** Tüm gerekli denemeler çalışmış; olumsuz örnekler beklenen hataları üretmiş.

**Sorun halinde** Derleme geçmesini davranış testi yerine kabul etme.

### PF 02 T06 Gösterilebilir teslimi kapat

**Bağımlılık** T05. Girdi: Rapor, testler ve kaynak bilgisi.

**Yapılacak değişiklik** README, kısa vaka çalışması ve demo komutunu hazırla; kaynak lisansını koru.

**Kabul** Donanım olmadan yeni kullanıcı aynı senaryoları görebiliyor.

**Sorun halinde** MCU veya saha doğrulaması kapsam dışı ifadesini kaldırma.

### Çalıştırma sözleşmesi

cmake -S . -B build<br>cmake --build build<br>ctest --test-dir build --output-on-failure<br>python scripts/run\_demo.py --out demo

Komutlar T00 ortam kararına göre tamamlanır. run\_demo geliştirme sırasında oluşturulur; bugün hazır değildir. --out gösterim kökünü belirtir; PF-01 üretim dosyaları bunun output alt dizinine gider. Çıkış kodları: 0 başarılı işlem, 2 beklenen veri/kabul sorunu, 1 araç hatası. Genel demo, bozuk örnekleri beklendiği gibi reddettiğinde başarılı olabilir; her örneğin sonucu ayrı kalır.

### Her görev sonunda bırakılacak kayıt

task\_id, status, input\_hashes, changed\_files, command, exit\_code, expected, observed ve next\_task alanlarını içeren kısa bir görev kaydı bırak. Komut çalıştırılmadıysa NOT\_RUN; gerekli girdi yoksa BLOCKED yaz. Kayıt tamamlandığında kapsam içindeki sıradaki göreve geç; her adımda yeniden izin isteme.

### Codex için ilk görev metni

PF-02 için bu Görev Planı ile eşleşen İş Akışı raporunu oku. Portfolyo/29.10.2026 içindeki manifestten repo yolunu bul. Mevcut sürücü testlerini ve HAL arayüzünü incele. Var olan testleri değiştirmeden başlangıç kaydı al; sonra bir sahte cevapla derlenebilir en küçük örneği oluştur. İlk çıktı T00 kaydı ve kaynak kilidi olsun. Bu turda T00 sonucuna göre T01'e geç; engel varsa dosya ve gerekçeyi açıkça yaz. Önceden var olan kullanıcı dosyalarını koru.

**Bitiş koşulu** İş Akışı raporundaki A01 ile A06 kontrolleri sonuçlanmış; demo açılmış; kısa vaka çalışması kaynak ve sınırları belirtmiş; yeniden çalıştırma kaydı var. Yalnız test sayısı veya klasörlerin varlığı tamamlanma ölçütü değildir.
