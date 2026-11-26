**ÜRÜN 4 · 26 KASIM 2026**

# ROS 2 Kontrol Regresyon Tezgâhı Ürün Raporu

Sabit bir simülasyon senaryosunda kontrol değişikliklerini karşılaştıran yerel kanıt tezgâhı

**Ürün kararı**

**ROS 2 ve Gazebo sürümlerini, dünya dosyasını, robot modelini, başlangıç durumunu, seed değerini ve kontrolcü ayarlarını sabitleyen; baseline ile aday commit'i ayrı kapalı çevrim koşularında çalıştıran; komut, odometri, simülasyon gerçek durumu, temas ve zaman verisini MCAP içinde toplayan; farkı görev ve kontrol metrikleriyle raporlayan yerel regresyon ürünü.**

**Birincil kullanıcı:** Küçük robotik ekipleri, ROS 2 entegratörleri ve kontrol yazılımı danışmanları

**İlk teslim:** Tek robot, tek dünya, tek kontrolcü ve iki commit için ücretli regresyon pilotu

**Kanıt düzeyi:** Kapalı çevrim simülasyon — Gerçek robot güvenliği veya sim to real kanıtı değildir

**Sosyal veri:** Reddit, YouTube, GitHub Issues ve topluluk forumları — Pazar büyüklüğü kanıtı değil

**Drive işlemi:** Yerel çalışma — Drive'a yazılmadı

## 1 Yönetici kararı

Ürün 4 olarak ROS 2 Kontrol Regresyon Tezgâhı seçilmiştir. 21.11.2026 kapsamındaki kayıtlar bu adayı FPGA doğrulamasından sonra en yüksek donanımsız uygulanabilirliğe sahip fırsat olarak sıralıyordu. Raspimouse2, raspimouse sim, ros2 control demos, ros2 tracing ve ROS 2 test kaynakları; dünya hash'i, seed, komut, odometri, simülasyon gerçek durumu ve MCAP ile kapalı çevrim kıyaslama yolunu zaten tarif ediyordu.

Güncel sosyal medya taraması kararı güçlendirdi. Kullanıcılar ROS dağıtımı, Gazebo sürümü, plugin sınıfı, controller manager, bridge, simülasyon zamanı ve reset davranışlarında tekrar eden kurulum ve regresyon sorunları bildiriyor. Ayrı bir geliştirici, her push'ta headless Gazebo çalıştıran bulut CI aracı için erken kullanıcı arıyor. Bu örnekler sorunun ürünleştirilebilir görüldüğünü gösterir; ödeme niyeti veya pazar büyüklüğü kanıtlamaz.

İlk sürüm genel amaçlı simülasyon platformu değildir. Tek robot, tek dünya ve tek kontrolcü ailesinde baseline ile aday commit'i karşılaştırır. Her senaryoyu yeni süreçte başlatır, ön kontrolleri yapar, kapalı çevrim telemetriyi kaydeder ve eşikleri önceden belirlenmiş metriklerle PASS, FAIL, BLOCK veya FLAKY BASELINE kararı üretir.

## 2 Sosyal medya sinyalleri

| **Tarih ve yüzey** | **Gözlenen sorun** | **Ürün etkisi** |
| --- | --- | --- |
| Nisan 2026 Reddit | ROS 2 ve Gazebo için her push'ta headless simülasyon çalıştıran beta CI aracı erken kullanıcı arıyor; gönderide kullanıcı yanıtı yok | Doğrudan rakip yönü; yerel çalışma ve alan metrikleriyle ayrış |
| 2026 Reddit | Ekipler simülasyonu edge case, CI, bag inceleme ve güvenli failure testi için kullanıyor; timing ve paket kaybı sim to real boşluğu olarak anılıyor | Gecikme ve drop fixture'ı ekle; HIL sınırını açık tut |
| Temmuz 2026 Reddit | Sürüm, dokümantasyon, latency ve ros2 control ile Gazebo entegrasyonu temel darboğazlar olarak bildiriliyor | Pinlenmiş ortam, uyumluluk matrisi ve preflight zorunlu |
| 2025 Reddit kayıtları | Plugin sınıfı, cmd vel bridge, controller yükleme, reset ve kararsız simülasyon sorunları tekrar ediyor | Plugin topic message QoS ve clock sözleşme testleri |
| 2024 ve 2025 YouTube | Gazebo geçişi ve Jazzy ros2 control eğitimleri görünür izlenme topluyor | Migration sonrası baseline kıyaslama ihtiyacı; izlenme satın alma kanıtı değil |
| Ekim 2026 GitHub | ros2 control issue 3683 Gazebo ROS clock gecikmesi ve kontrol hatasını tekrarlı koşularla ölçüyor | Clock gap controller time ve timestamp kalitesi birinci sınıf metrik |

### Sinyallerin sınırı

Sosyal gönderiler anekdottur. Başlangıç seviyesindeki kullanıcıları fazla temsil edebilir, aynı kişinin birden fazla gönderisi tekrar sayılabilir ve etkileşim satın alma yetkisini göstermez. Bu tarama ürün sorununu ve kullanılabilir dili keskinleştirir; müşteri görüşmesi, ücretli pilot ve yeniden satın alma ölçümünün yerine geçmez.

## 3 Müşteri ve kullanım sınırı

### Birincil müşteri

- Kontrolcü veya robot tanımı değiştiğinde hangi davranışın bozulduğunu düzenli olarak ölçemeyen küçük ROS 2 ekipleri

- Gazebo Classic'ten yeni Gazebo'ya veya Humble tabanından Jazzy tabanına geçiş yapan entegratörler

- Müşteriye simülasyon sonucu, ham kayıt ve yeniden çalıştırma paketi teslim etmek isteyen robotik danışmanları

### Kullanıcının tamamlamak istediği iş

Bir kontrol yazılımı, model, dünya veya bağımlılık değişikliğinin robot davranışını bozup bozmadığını aynı başlangıç koşullarında görmek; başarısızlığı commit, seed, süreç logu ve MCAP ile tekrar üretmek; aday sürümü baseline'a göre ölçülebilir bir kabul kararına bağlamak.

### Kapsam dışı iddialar

- Gerçek robot güvenliği, saha kararlılığı veya sertifikalı kontrol doğrulaması

- Rosbag içindeki aktüatör komutlarını oynatmayı kapalı çevrim deney tekrarı sayma

- Farklı fizik motorları ve bilgisayarlarda bit düzeyinde aynı sonuç garantisi

- Her ROS dağıtımı, her Gazebo sürümü ve her robot paketini otomatik destekleme

- Müşteri onayı olmadan gerçek robot topic'lerine veya deployment hattına erişme

## 4 Ürün mimarisi

| **Katman** | **Bileşen ve işlev** | **Çıktı** |
| --- | --- | --- |
| Ortam sözleşmesi | Container digest; ROS 2, Gazebo, ros2 control, plugin ve RMW sürümü; bağımlılık kilidi | Pinlenmiş çalışma ortamı |
| Senaryo sözleşmesi | Dünya, model, mesh ve parametre hash'i; başlangıç pozu; seed; clock ve physics ayarı; komut profili | Sürümlü senaryo manifesti |
| Ön kontrol | Plugin discovery, controller durumu, topic message QoS, TF, bridge, use sim time, clock ve timeout kontrolü | Koşudan önce PASS veya BLOCK |
| Koşu yöneticisi | Her senaryoda yeni ROS domain ve yeni süreç; world spawn, controller activation, komut yürütme ve kontrollü kapanış | İzole kapalı çevrim koşu |
| Telemetri | cmd vel, odom, joint state, ground truth, TF, contacts, controller lifecycle, clock ve süreç logları | MCAP ve ham günlükler |
| Metrik motoru | Görev başarısı, poz ve yaw hatası, yol RMSE, overshoot, settling, collision, timeout, clock gap ve timestamp kalitesi | Makinece okunur sonuçlar |
| Karşılaştırma | Baseline dağılımı, aday dağılımı, ön kayıtlı tolerans ve kasıtlı bozuk kontrolcü | PASS FAIL FLAKY BASELINE |
| Kanıt paketi | Commit, container, dünya hash'i, seed, komut, exit code, JUnit, JSON, MCAP ve checksum | Bağımsız yeniden koşu paketi |

### Kanıt sınıfları

| **Sınıf** | **Ne gösterir** | **Ne göstermez** |
| --- | --- | --- |
| SIMULATION | Pinlenmiş kapalı çevrim senaryosunda ölçülen davranış | Gerçek robot güvenliği ve fiziksel performans |
| REPLAY QC | MCAP içeriği, topic, timestamp ve veri işleme regresyonu | Aynı kontrol döngüsü veya aynı yörünge |
| HIL | Ayrı fixture içinde gerçek işlemci veya arayüz davranışı | Tam saha ve güvenlik kabulü |
| HARDWARE | Gerçek robotta tanımlı test sonucu | Başka robot ve saha koşullarına genelleme |

## 5 İlk ürün kapsamı

### MVP teslimleri

- Tek pinlenmiş ROS 2 ve Gazebo çifti; önerilen ilk hedef Jazzy ve Harmonic

- Tek diferansiyel sürüşlü robot fixture'ı, tek dünya ve tek ros2 control kontrolcü ailesi

- Baseline commit ile aday commit için ayrı ve yeni süreçte kapalı çevrim koşu

- Başlangıç ve controller activation, düz sürüş, dönüş ve kasıtlı bozuk kontrolcü senaryoları

- Plugin, bridge, clock, topic, message type, QoS, TF ve controller lifecycle ön kontrolleri

- MCAP kayıtları, süreç logları, JUnit, metrik JSON, bulgu raporu ve evidence manifest

- Yerel tek komutlu koşu ve isteğe bağlı müşteri CI adaptörü

### MVP dışında

- Çok robotlu namespace ve görev planlama

- Nav2 benchmark platformu veya genel bulut simülasyon hizmeti

- GPU kamera ve lidar algı kalitesinin kapsamlı doğrulaması

- HIL, gerçek robot ve simülasyonu tek kabul seviyesinde birleştirme

- Simülasyondan gerçek robota otomatik deployment

## 6 Doğrulama planı

Aşağıdaki sayılar ölçülmüş ürün sonucu değildir. K1 başlamadan önce sabitlenecek önerilen kabul hedefleridir. Yol ve kontrol eşikleri fixture'a göre tanımlanır; aday commit görülmeden önce baseline toleransı kilitlenir.

| **Aşama** | **Test** | **Önerilen kabul hedefi** |
| --- | --- | --- |
| K1 ortam | Temiz container kurulumu | Pinlenmiş digest ve tek komutla build launch; eksik veya uyumsuz paket varsa BLOCK |
| K1 ön kontrol | Plugin topic QoS TF clock | Zorunlu sözleşme alanlarının tamamı bulunur; yanlış plugin ve message fixture'ı koşudan önce yakalanır |
| K1 baseline | Üç bağımsız tekrar | Karar aynı; ölçülen ana metriklerin değişimi ön kayıtlı fixture toleransı içinde |
| K1 zaman | Clock ve timestamp kalitesi | Geri giden timestamp 0; clock gap eşiği physics step oranına göre önceden sabitlenir |
| K1 görev | Düz sürüş ve dönüş | Görev tamamlanır; collision 0; poz hata RMSE ve settling hedefi önceden kilitli |
| K1 negatif | Bozuk gain gecikme veya bridge | Her kayıtlı mutasyon beklenen metrik veya preflight kontrolünde FAIL |
| K1 izolasyon | Ardışık koşular | ROS domain süreç ve world durumu sızıntısı 0; her senaryo yeni süreçte |
| K1 kanıt | Manifest bütünlüğü | Commit container dünya seed komut exit code metrik MCAP ve hash alanlarının tamamı dolu |
| K2 holdout | Görülmemiş komut ve başlangıç pozu | Aday görülmeden kilitlenen senaryolarda beklenen PASS ve FAIL sınıfları doğru |
| K2 yük | Clock gecikmesi ve paket kaybı | Enjekte edilen bozulma görünür bulgu üretir; gizli yeniden deneme yok |
| K2 geçiş | Baseline ve migration adayı | Aynı görev sözleşmesi altında metrik farkı ve uyumsuzluk kök nedeni raporlanır |
| K3 pilot | Müşteri repo ve CI | Müşteri temiz ortamında tek komutla aynı karar ve kabul edilen kanıt paketi |

## 7 On haftalık geliştirme planı

| **Hafta** | **Çıktı** | **Çıkış kapısı** |
| --- | --- | --- |
| 1 | Tek robot ve sürüm çifti seçimi; lisans ve gizlilik matrisi | Genel platform iddiası yok; tüm girdiler yazılı |
| 2 | Pinlenmiş container ve ortam ön kontrolü | Temiz ortam build launch ve controller activation |
| 3 | Senaryo manifesti, world model seed ve başlangıç pozu | Aynı senaryo yeni süreçte tekrar kuruluyor |
| 4 | MCAP recorder ve süreç log toplayıcı | Zorunlu topic ve timestamp kalite raporu |
| 5 | Düz sürüş ve dönüş metrikleri | RMSE overshoot settling ve görev kararı |
| 6 | Clock gap reset timeout ve bridge negatif fixture'ları | Kasıtlı kusurların tamamı beklenen FAIL |
| 7 | Baseline aday karşılaştırıcı ve flaky baseline kararı | Üç tekrar ve ön kayıtlı tolerans |
| 8 | JUnit JSON bulgu raporu ve kanıt manifesti | İki temiz koşu aynı normalize kararı üretir |
| 9 | Kilitli holdout senaryosu ve bağımsız tekrar | Test yazarının görmediği başlangıç ve komut profili |
| 10 | Tek robotlu ücretli pilot SOW ve müşteri CI devir paketi | NDA veri saklama kabul ve devam koşulları yazılı |

## 8 Ticari pilot

İlk satış biçimi bulut aboneliği değil, tek robot ve iki commit için sabit kapsamlı yerel regresyon pilotudur. Müşteri repo erişimini, baseline ve aday commit'i, hedef görevi ve kabul edeceği metrikleri sağlar. Teslim; pinlenmiş runner, senaryo manifesti, dört koşu ailesi, MCAP kayıtları, metrik karşılaştırması, bulgu raporu ve müşteri CI yönergesidir.

### Sosyal rakipten ayrışma

- Yalnız colcon test pass fail yerine kapalı çevrim görev ve kontrol metrikleri

- Cloud zorunluluğu olmadan yerel ve müşteri CI ortamında çalışma

- Kasıtlı bozuk kontrolcü ile test gücünü gösterme

- Clock reset plugin bridge ve timestamp sorunlarını ayrı bulgu sınıflarında raporlama

- MCAP ve hash ile kabul kararını yeniden çalıştırılabilir pakete bağlama

### Doğrulanacak ticari varsayımlar

- Ekip aynı regresyon paketini ikinci commit veya ikinci release için tekrar satın alır

- Müşteri genel danışmanlık yerine tek görev ve ölçütlü pilotu kabul eder

- Yerel runner ve kaynakların dışarı çıkmaması güvenlik satın alma engelini azaltır

- Flaky baseline kararı başarısızlığı gizlemek yerine müşteriye değer sağlar

- Pilot sonrası yeni senaryo veya ikinci robot için ücretli genişleme isteği oluşur

## 9 Riskler ve durdurma kapıları

| **Risk** | **Kontrol** | **Durdurma veya yeniden tasarım kapısı** |
| --- | --- | --- |
| Sosyal medya yanlılığı | Gönderileri sorun dili için kullan; ücretli pilot ve tekrar ödeme ayrı ölç | İki hedef ekip yazılı metrik vermezse ürün kararı yükselmez |
| Flaky simülasyon | Üç baseline tekrar, toleranslı metrik ve FLAKY BASELINE kararı | Baseline kararlı değilse aday için PASS verme |
| Sürüm çeşitliliği | Tek resmi uyumlu ROS Gazebo çifti ve container digest | Her müşteri yeni dağıtım isterse ürün değil danışmanlık |
| Bag replay yanılgısı | REPLAY QC ve SIMULATION kanıt sınıflarını ayır | Komut replay'i deney tekrarı sayılıyorsa kapsamı reddet |
| Ground truth eksikliği | Simülatör pose ve contact kaynağını fixture içinde doğrula | Referans veri yoksa yörünge doğruluğu iddiası yok |
| Gerçek robot yanlış güveni | Simülasyon sınırı, HIL ve hardware için ayrı kabul planı | Güvenlik veya otomatik deployment bekleniyorsa pilotu durdur |
| IP ve hassas bag | Yerel runner, veri minimizasyonu, saklama süresi ve silme kaydı | Yazılı hak olmadan repo bag veya mesh saklama yok |
| Rakip baskısı | Genel cloud CI yerine alan metriği ve kanıt paketi | Müşteri yalnız ucuz genel CI istiyorsa dar migration audit'e pivot |

### Go no go kararı

Go: pinlenmiş ortam temiz makinede kurulursa; üç baseline tekrar aynı kararı verirse; kasıtlı plugin, clock ve controller kusurları beklenen biçimde yakalanırsa; müşteri metrikleri aday commit görülmeden kilitlenirse ve tek robotlu ücretli pilot müşteri CI ortamında yeniden çalışırsa ürün ilerler.

No go veya pivot: baseline kararlı değilse, ground truth alınamıyorsa, her proje yeni simülatör ve adaptör gerektiriyorsa, müşteri bag komut replay'ini fiziksel tekrar sayıyorsa veya simülasyon sonucundan gerçek robot güvenliği bekliyorsa genel ürün durdurulur. Çıktılar sürüm geçişi ve regresyon denetimi hizmeti olarak korunur.

## 10 Kaynaklar

**K1:** [24 Kasım Referans Listeler PCB Makine Görüsü ROS 2 ve FPGA](https://docs.google.com/document/d/1oAJTYGt4XvYokdU1XCrVnFKb6zCa2ptOWQjd1sC2n5s/edit) — Raspimouse ros2 control ve tracing kaynakları

**K2:** [24 Kasım Araştırma Kayıtları Güncel Sentezi](https://docs.google.com/document/d/1dcz3_32Uf0tsr9-vKUdqMf60GC-qXrS3NpROj8Uhu4s/edit) — rt control bench eşlemesi ve kanıt sınırı

**S1:** [ROS 2 simulation testing CI early testers](https://www.reddit.com/r/robotics/comments/1scewdn/looking_for_early_testers_for_a_ros_2_simulation/) — Bulut CI rakip yönü; kullanıcı yanıtı yok

**S2:** [Anyone using simulation before real robots](https://www.reddit.com/r/ROS/comments/1s5dotx/anyone_here_using_simulation_before_working_with/) — CI edge case bag ve sim to real tartışması

**S3:** [ROS 2 bottlenecks](https://www.reddit.com/r/AskRobotics/comments/1up5v5m/most_frustrated_or_biggest_bottleneck_when/) — Sürüm dokümantasyon latency ve entegrasyon

**S4:** [Error loading ros2 control plugin](https://www.reddit.com/r/ROS/comments/1ogvh1k/error_loading_ros2_control_plugin/) — Plugin sınıfı ve sürüm uyuşmazlığı

**S5:** [Gazebo new community critique](https://www.reddit.com/r/ROS/comments/1jqn8wb/rant_gazebo_new_is_a_terrible_piece_of_software/) — Sürüm adlandırma reset ve dokümantasyon sorunları

**S6:** [How to upgrade your ROS project to the New Gazebo](https://www.youtube.com/watch?v=fH4gkIFZ6W8) — Migration eğitimi ve topluluk ilgisi

**S7:** [ROS 2 Jazzy ros2 control in Gazebo](https://www.youtube.com/watch?v=_F8wVuiEmww) — Kurulum ve kontrol eğitimi

**S8:** [ROS 2 bag replay of teleop commands causes vehicle divergence](https://robotics.stackexchange.com/questions/118241/ros-2-bag-replay-of-teleop-commands-speed-cmd-steer-angle-cmd-causes-vehicl) — Komut replay'i ile kapalı çevrim deney tekrarı arasındaki sınır

**T1:** [ros2 control issue 3683](https://github.com/ros-controls/ros2_control/issues/3683) — Clock gecikmesi ve kontrol hatası ölçümleri

**T2:** [gz ros2 control reset crash issue 636](https://github.com/ros-controls/gz_ros2_control/issues/636) — Reset sonrası süreç hatası örneği

**T3:** [Gazebo Classic migration](https://gazebosim.org/docs/latest/gazebo_classic_migration/) — Resmî geçiş rehberi

**T4:** [gz ros2 control Jazzy documentation](https://control.ros.org/jazzy/doc/gz_ros2_control/doc/index.html) — Jazzy Harmonic ve headless Docker örneği

**T5:** [ROS 2 integration testing](https://github.com/ros2/ros2_documentation/blob/rolling/source/Developer-Tools/Debugging/Testing/Integration.rst) — launch testing benzersiz domain ve XUnit

**T6:** [rosbag2](https://github.com/ros2/rosbag2) — Kayıt oynatma MCAP ve sim time sınırları

### Kaynak kullanım sınırı

Sosyal gönderiler ve video etkileşimleri sorun keşfi içindir; müşteri sayısı, pazar büyüklüğü veya ödeme niyeti kanıtlamaz. GitHub issue sonuçları yalnız bildirilen ortam ve sürümlere aittir. Resmî belgeler araç sözleşmesini açıklar; bu ürünün ölçülmüş performansı değildir. Rapordaki kabul değerleri önerilen ön kayıt hedefleridir.
