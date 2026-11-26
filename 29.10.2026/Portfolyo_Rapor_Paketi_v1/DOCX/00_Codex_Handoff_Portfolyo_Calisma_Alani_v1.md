# Portfolyo çalışma alanının üretilmesi

Codex Handoff raporu

Kemal Demirağ için hazırlanmıştır. Muhatap: çalışma alanını kuracak ve seçili örneği geliştirecek Codex.

Portfolyo/29.10.2026 altında beş küçük ve gösterilebilir çalışma için sade bir alan kur. Bu handoff yalnız klasörleri, başlangıç dosyalarını, rapor yerleşimini ve proje bağlantılarını hazırlar. Uygulama geliştirmesini her reponun Görev Planı yürütür.

**Kapsam kararı** Kök Portfolyo klasörü altındadır. Projelerim ölçeğinde araştırma arşivi, sekiz bölümlü doküman sistemi, ortak platform veya üretim ürünü kurulmayacak. Üç tanıdık ana klasör, beş bağımsız örnek ve kısa müşteri açıklamaları yeterlidir.

## Paket içeriği

Bir ortak Handoff, beş İş Akışı ve beş Codex Görev Planlama raporu bulunur. Her raporun düzenlenebilir DOCX ve okumalık PDF sürümü vardır. Girdi\_Ekleri içindeki küçük sözleşmeler ve sentetik veriler, ilgili repoya aktarılacak başlangıç girdileridir.

| **Kod** | **Repo** | **Gösterilecek ilk çıktı** |
| --- | --- | --- |
| PF-01 | pcb-release-review-workbench | Müşteri BOM dosyasını, kartın etkileşimli montaj görünümünü, üretim çıktılarının listesini ve açık bulguları aynı örnekte inceleyebilsin. |
| PF-02 | sensor-driver-test-lab | Tek çalıştırmada geçerli ölçüm, CRC hatası, iletişim hatası, eski ölçüm ve toparlanma sonuçları yan yana görülsün. |
| PF-03 | ros2-imu-validation-lab | Sanal seri kaynaktan gelen veri, oluşan ROS mesajı ve beklenen sonuç aynı raporda karşılaştırılsın. |
| PF-04 | power-circuit-review-case | İşaretli şema, bileşen tablosu, tekrar çalıştırılabilir kayıp hesabı ve hangi fiziksel ölçümlerin gerektiğini gösteren kısa PDF üretmek. |
| PF-05 | battery-test-data-validator | İki farklı dosyanın aynı 0,5 Ah sonucuna ulaştığı ve hatalı verinin geçerli sonuç olarak sunulmadığı tek HTML raporunda görülsün. |

### Bu belgenin doğrulama durumu

Raporlar 29.10.2026 konuşması ve bağlantılı kaynak ana sayfaları üzerinden hazırlanmıştır. Bu pakette uygulama repoları oluşturulmadı, upstream kodları derlenmedi ve kabul testleri yürütülmedi. Kaynak commitleri null bırakılmıştır; T00 görevleri bunları gerçek değerlerle doldurur. Sentetik veri üretimi, gerçek batarya veya kart deneyi değildir.

## Sade klasör düzeni

| **Konum** | **İçerik** |
| --- | --- |
| README.md | Beş örnek, her birinin durumu, demo ve rapor bağlantıları |
| PROJECT-MANIFEST.json | Proje kodu, repo/doküman/kanıt yolları ve kaynak durumu |
| 001-Kayıt Notları ve Dokümantasyon/ | Ortak Handoff ve her PF kodu altında iki rapor; tek kaynak envanteri |
| 002-Proje Çıktıları/ | Beş örneğin ayrı repo kökü veya mevcut repoya kayıtlı bağlantı |
| 003-Proje Kayıtları/ | PF kodu ve run-id altında gerçek log, görsel ve gerektiğinde kısa video |

Handoff dosyası 001-Kayıt Notları ve Dokümantasyon/ altında; İş Akışı ve Görev Planı 001-Kayıt Notları ve Dokümantasyon/PF-xx/ altında tutulur. Tam kaynak raporları, eski Projelerim arşivi ve iş ilanı ekran görüntüleri bu alana topluca kopyalanmaz; bağlantı ve kısa amaç kaydı yeterlidir.

### Her repoda aynı küçük dış çerçeve

README.md, sources.lock.json, docs/case-study.md, demo/ ve tests/ bulunur. İç kaynak dizinleri projenin teknolojisine göre değişir. Demosu bulunmayan proje için boş başarı görseli üretme. Ortak kod kütüphanesi veya monorepo bağımlılığı kurma; beş örnek tek başına çalıştırılabilir olmalı.

### Mevcut repo ve fork kuralı

Önce hedef kökteki README, manifest ve Git remotelarını incele. Mevcut aynı proje varsa yeniden repo açma; manifestte mevcut yolu kullan. Beş portföy projesi varsayılan olarak bize ait yeni entegrasyon örnekleridir; referans kullanmak otomatik olarak fork anlamına gelmez.

Kaynak repoya doğrudan katkı yapılacaksa kullanıcının kuralı geçerlidir: fork varsa Forkladığım Repolar altındaki yerel klon ve kullanıcının remote adresi; fork yoksa Katkı Yaptığım Repolar altındaki klon ve doğrulanmış upstream katkı yolu. Yazma yetkisi varsayma; yetki yoksa patch/PR yolu kaydet. Bu durumda Portfolyo manifesti mevcut çalışma konumuna işaret eder; ikinci klon oluşturulmaz.

### Kaynak kilidi

Her kaynak için URL, kullanım rolü, alınan commit, dosya yolu, lisans dayanağı ve erişim tarihini yaz. Branch adı tek başına sürüm kilidi sayılmaz. Bilinmeyen değer null ve durumu UNVERIFIED kalır. İlk sürümde üçüncü taraf kaynak kodunu topluca kopyalamak zorunlu değildir.

## Kurulum görevleri ve kontrolü

### H00 Hedefi doğrula

Kullanıcının yerel Portfolyo kökünü bul; 29.10.2026 alanının içeriğini ve remoteları kaydet. Belirlenemeyen mutlak yolu tahmin etme.

### H01 Klasörleri hazırla

Eksik üç ana klasörü ve PF rapor/kanıt alanlarını oluştur. Var olan dosyaları silme, yeniden adlandırma veya üzerine yazma.

### H02 On bir raporu yerleştir

Ortak Handoff ile beş rapor çiftini doğru yere kopyala. Aynı hash varsa işlem yapma; farklı içerik varsa sürüm çatışması kaydı bırak.

### H03 Başlangıç girdilerini bağla

Girdi\_Ekleri/PF-xx dosyalarını ilgili repo examples veya config alanına eşleştir. Her proje README içinde girdi rolünü ve eksikleri açıkla.

### H04 Manifesti üret

PROJECT-MANIFEST.template.json örneğini gerçek yerel yollarla doldur. Repo durumu SCAFFOLD, demo/test durumu NOT\_RUN olarak başlar.

### H05 Kurulumu doğrula

Göreli yolların varlığını, rapor sayısını, JSON ayrıştırmasını ve girdi hashlerini kontrol et. Kurulumu ikinci kez çalıştırınca içerik değişmemeli.

### H06 Geliştirmeye teslim et

Kurulum özetini ve her projenin ilk görevini ver. Seçilen reponun Görev Planı ile devam edilir; tüm projeler aynı anda geliştirilmez.

### Kurulum kabul koşulları

Kök Portfolyo altında; tam 11 ayrı rapor doğru yerlerde; beş proje yolu tekil; gerçek olmayan Git URL/commit yok; örnek veriler sentetik olarak etiketli; ikinci kurulum kullanıcı içeriğini değiştirmiyor. Mevcut harici repoya işaret varsa hedef erişilebilir ve manifestte açıkça gösterilmiş.

Kurulum raporu; oluşturulan, zaten mevcut, atlanan ve engellenen öğeleri ayrı listeler. Oluşturulmuş klasör uygulama tamamlandı anlamına gelmez. Bu handoff kendi başına uzak repo açma, push, yayınlama veya müşteriye dosya gönderme talimatı değildir.

## Gösterim kapsamı ve ilk kullanım

| **Proje** | **İlk çalıştırma için gerekli doğrulama** |
| --- | --- |
| PF-01 | Native şema ve PCB aynı revizyona ait olmalı; proje gerekli kütüphanelerle açılabilmeli. Bu doğrulanmadan üretim çıktısı görevine geçilmez. |
| PF-02 | Kaynak sürücü bilgisayarda derlenmeli; HAL imzaları ve mevcut testler incelenmeli. Bir doğru ham cevap için bağımsız beklenen değer yazılmalı. |
| PF-03 | Sürücü gerçek protokol fixture dosyasıyla kurulabilmeli. Sanal portun başlangıç komutlarına cevap vermesi gerekip gerekmediği kaynak koddan çıkarılmalı. |
| PF-04 | Tek bölüm, net sınırları, kaynak revizyonu ve tam parça numaraları bulunmalı. Kaynak doğrulanana kadar yalnız sentetik hesap örneği çalıştırılabilir. |
| PF-05 | Eklerdeki iki CSV, metadata ve expected.json birlikte verilmelidir. Yeni cihaz dosyası eklenmesi ayrı kapsam olarak kaydedilir. |

### En kısa kullanım sırası

Önce bu Handoff ile alanı kur. Sonra tek bir repo seçip İş Akışı ile kabul koşullarını oku; aynı reponun Codex Görev Planını çalıştır. Başvuru odaklı ilk aday PF-01; dış kaynak çözümlemesi beklenirken hazır sentetik girdileri bulunan PF-05 üzerinde de bağımsız ilerlenebilir. Birini bitirmek diğer dördünün tamamlanmasına bağlı değildir.

### Portföyde tamamlanmış sayılma ölçütü

Bir açılabilir demo veya inceleme dosyası, tekrar çalıştırma talimatı, bir doğru ve bir hata örneği, gerçek kabul kaydı ve kısa müşteri açıklaması bulunmalı. Görsel veya video yalnız gerçek demo çalıştırmasından alınır. Veri ve doküman örneğiyle yapılan masa başı çalışma açıkça belirtilir.

### Codex için başlangıç metni

Bu paketteki ortak Handoff raporunu uygula. Hedefim Portfolyo/29.10.2026 altında beş küçük gösterim çalışması için sade bir alan. Önce mevcut dizin ve repo durumunu incele, ardından H00-H06 görevlerini sırayla tamamla. Üç ana klasörü kullan; Projelerim düzeyindeki araştırma ve doküman arşivini kurma. On bir raporu ve Girdi\_Ekleri içeriğini uygun yerlere yerleştir. Mevcut dosyaları koru; ikinci çalıştırmada çoğaltma. Sonuçta gerçek yolları, eksik girdileri ve hangi repo için hangi ilk görevin hazır olduğunu bildir. Uygulama testleri henüz yapılmadıysa NOT\_RUN yaz.

### Kaynak sahipliği ve müşteri anlatımı

Kart veya sürücünün kaynak sahibini ve bizim eklediğimiz testi/otomasyonu ayrı yaz. OLIMEX donanım, yazılım ve dokümantasyon lisansları ayrı değerlendirilir. Diğer kaynakların lisansları da alınan revizyonda kontrol edilir. Kaynaklı dosyalar paylaşılacaksa ilgili lisans metinleri teslimde korunur.

Müşteriye gönderilecek kısa metin şu beş soruyu yanıtlar: sorun neydi, hangi girdi kullanıldı, bizim katkımız ne, hangi sonuç gerçekten doğrulandı, hangi bölüm hâlâ doğrulanmadı. İlanların genel kapsamını üstlenebilme iddiası yalnız bu portföyün varlığına dayandırılmaz.
