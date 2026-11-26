# PF 03 ROS 2 IMU sürücüsü doğrulaması

Codex görev planlama raporu

Kemal Demirağ için hazırlanmıştır. Muhatap: çalışma alanını kuracak ve seçili örneği geliştirecek Codex.

Bu plan yalnız gösterilebilir ilk örneği üretir. Görevler T00 ile T06 sırasında yürütülür; karşılanmayan bir önkoşul bağımlı görevi durdurur. Bağımsız işler devam edebilir. Bu belgedeki komutlar hedef arayüzdür; mevcut bir uygulamanın çalıştığı iddia edilmez.

**Repo** Portfolyo/29.10.2026/002-Proje Çıktıları/ros2-imu-validation-lab/

**Ortam** Linux veya WSL2 içinde doğrulanmış ROS ortamı. Sanal seri port için PTY kullanılabilir; port ayarı yeterli olmazsa dar bir taşıma arayüzü eklenir ve upstream farkı belgelenir.

**Başlama koşulu** Sürücü gerçek protokol fixture dosyasıyla kurulabilmeli. Sanal portun başlangıç komutlarına cevap vermesi gerekip gerekmediği kaynak koddan çıkarılmalı.

## Az sayıda dosyayla kurulacak yapı

| **Konum** | **Görevi** |
| --- | --- |
| src/ | ROS çalışma alanındaki test paketi ve gerekirse ince adaptör |
| fixtures/ | Kaynaklı ham protokol kayıtları ve beklenen ROS alanları |
| tools/ | Sanal seri kaynak ve senaryo çalıştırıcısı |
| config/ | Dağıtım, port, frame ve senaryo ayarları |
| demo/ | Mesaj karşılaştırması ve sonuç raporu |

Ortak dosyalar: README.md, sources.lock.json, docs/case-study.md ve küçük bir tests alanı. Büyük test videoları ve ayrıntılı ham kayıtlar repo dışında 003-Proje Kayıtları içinde tutulur. Release manifesti bu kayıtların konumunu ve hashlerini gösterir.

### PF 03 T00 Dağıtım ve başlangıç kaydı

**Bağımlılık** Yok. Girdi: Sürücü URL ve demo hedefi.

**Yapılacak değişiklik** Dal, commit, package bağımlılıkları, mevcut testler ve derleme sonucunu kaydet.

**Kabul** Seçilen ROS ortamında upstream derlenir; araç sürümleri belirli.

**Sorun halinde** Destek uyuşmazsa aynı belgede karar kaydet; dağıtımı sessizce değiştirme.

### PF 03 T01 Protokol ve beklenen mesaj

**Bağımlılık** T00. Girdi: Kaynak kod, cihaz belgesi ve mevcut fixture.

**Yapılacak değişiklik** fixtures içinde bir doğru paket, alan haritası ve bağımsız expected\_message.json oluştur.

**Kabul** Birim dönüşümleri ve paket sınırı kaynağa bağlı; rastgele sentetik protokol yok.

**Sorun halinde** El sıkışma veya checksum bilinmiyorsa veri kaynağını henüz çalıştırma.

## Çalışan örneğin üretilmesi

### PF 03 T02 Sanal taşıma katmanı

**Bağımlılık** T01. Girdi: Protokol ve driver port parametresi.

**Yapılacak değişiklik** tools içinde PTY tabanlı cihaz taklidi kur; gerekirse dar adaptörü patch olarak belgele.

**Kabul** Taklit gerçek driver okuma yolunu çalıştırır; tek paketten gerçek topic mesajı çıkar.

**Sorun halinde** Yalnız doğrudan ROS mesajı yayımlayıp sürücü test edildi deme.

### PF 03 T03 Paket ve durum testleri

**Bağımlılık** T02. Girdi: Ek senaryolar.

**Yapılacak değişiklik** Parçalı, eksik, bozuk ve kesintili akışları uygula; bounded test süreleri ve gerçek lifecycle gözlemlerini kaydet.

**Kabul** A02 ile A05 testleri çalışır; mevcut davranış ile yapılan değişiklik ayrı gösterilir.

**Sorun halinde** Takılı test dış zaman aşımıyla kesilir; sessizce başarılı sayılmaz.

### PF 03 T04 Mesaj karşılaştırma raporu

**Bağımlılık** T03. Girdi: ROS çıktıları ve bağımsız beklenenler.

**Yapılacak değişiklik** tools/run\_demo.py ile demo/report.html üret; topic, frame, zaman, değer ve hata davranışını göster.

**Kabul** A01 ve A06 sayısal toleransları protokol ölçeğine göre yazılmış.

**Sorun halinde** Gerçek olmayan quaternion, covariance veya kalibrasyon sonucu ekleme.

### Uygulama kararları

Sürücünün normal yaşam döngüsünü önce gözlemle. Kesintide otomatik yeniden bağlanma zorunlu v1 özelliği değildir; belgelenmiş bir toparlanma yolu yeterlidir. İstenen davranış ile upstream davranışı farklıysa düzeltmeyi ayrı patch ve regresyon testiyle göster.

## Kabul ve portföy teslimi

### PF 03 T05 Temiz derleme ve kabul

**Bağımlılık** T04. Girdi: Altı kabul denemesi.

**Yapılacak değişiklik** Yeni build/install/log dizinlerinde derle; colcon test ve test-result kayıtlarını sakla.

**Kabul** Gerekli denemeler gerçekten yürütülmüş; zaman aşımı ve sonuç özeti mevcut.

**Sorun halinde** Eski log dizinindeki başarıyı yeni çalıştırmaya bağlama.

### PF 03 T06 Demo ve vaka çalışması

**Bağımlılık** T05. Girdi: Rapor ve kabul kayıtları.

**Yapılacak değişiklik** Kurulum, iki terminal demo akışı, toparlanma adımları ve docs/case-study.md yaz.

**Kabul** Yeni kullanıcı aynı paketle aynı mesajı ve aynı hata kaydını görebilir.

**Sorun halinde** SLAM veya gerçek IMU doğruluğu iddiasına genişletme.

### Çalıştırma sözleşmesi

colcon build --symlink-install<br>colcon test --event-handlers console\_direct+<br>colcon test-result --verbose<br>python3 tools/run\_demo.py --out demo

Komutlar T00 ortam kararına göre tamamlanır. ROS ortamı source edildikten sonra çağrılır. run\_demo geliştirme sırasında oluşturulur; bugün hazır değildir. --out gösterim kökünü belirtir; PF-01 üretim dosyaları bunun output alt dizinine gider. Çıkış kodları: 0 başarılı işlem, 2 beklenen veri/kabul sorunu, 1 araç hatası. Genel demo, bozuk örnekleri beklendiği gibi reddettiğinde başarılı olabilir; her örneğin sonucu ayrı kalır.

### Her görev sonunda bırakılacak kayıt

task\_id, status, input\_hashes, changed\_files, command, exit\_code, expected, observed ve next\_task alanlarını içeren kısa bir görev kaydı bırak. Komut çalıştırılmadıysa NOT\_RUN; gerekli girdi yoksa BLOCKED yaz. Kayıt tamamlandığında kapsam içindeki sıradaki göreve geç; her adımda yeniden izin isteme.

### Codex için ilk görev metni

PF-03 için bu Görev Planı ile eşleşen İş Akışı raporunu oku. Portfolyo/29.10.2026 içindeki manifestten repo yolunu bul. Seçilen dağıtım dalını ve mevcut testleri incele. Protokol başlangıç alışverişini ve bir doğru paketi kaynaklara bağla; sürücü kurulmadan ROS test sonucu üretme. İlk çıktı T00 kaydı ve kaynak kilidi olsun. Bu turda T00 sonucuna göre T01'e geç; engel varsa dosya ve gerekçeyi açıkça yaz. Önceden var olan kullanıcı dosyalarını koru.

**Bitiş koşulu** İş Akışı raporundaki A01 ile A06 kontrolleri sonuçlanmış; demo açılmış; kısa vaka çalışması kaynak ve sınırları belirtmiş; yeniden çalıştırma kaydı var. Yalnız test sayısı veya klasörlerin varlığı tamamlanma ölçütü değildir.
