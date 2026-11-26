# Ürün 3 ROBOTIS MuJoCo Menagerie Yeniden Üretim Planı

21 Kasım 2026 Kaynak Kesimi İçin Yerel ve Doğrulanabilir Uygulama Planı

**Kapsam kesim tarihi:** 21.11.2026

**Kaynak depo:** ROBOTIS-GIT robotis\_mujoco\_menagerie

**Sabit revizyon:** d8344c0dbe7a00208d0301111523dde65efc174a

**Önerilen program:** 6 takvim haftası ve 48 kişi gün

**Karar Önerisi**

Yeniden üretim, STL varlıklarını yeniden çizmek olarak değil; sabit committeki 188 dosyayı hash düzeyinde koruyan ve dokuz modeli seçilmiş MuJoCo sürümlerinde otomatik olarak doğrulayan deterministik bir paket üretmek olarak tanımlanmalıdır. Bu yaklaşım kaynak arşivinin eşliğini ve simülasyon davranışının ölçülebilir eşliğini ayrı kanıtlarla gösterir.

Tarihsel sadakat hattı için MuJoCo 3.4.0, 21.11.2026 kesimindeki güncel uyumluluk hattı için MuJoCo 4.0.1 kullanılmalıdır. MuJoCo 3.15.0 ile değişen dampratio hesabı nedeniyle iki hattın dinamik sonuçları aynı sayısal baseline olarak kabul edilmemeli, her hat kendi imzası ve toleransıyla saklanmalıdır \[8\].

İlk on iş gününde kaynak manifesti, iki sürümlü ortam, tüm XML dosyaları için derleme testi ve iki pilot model tamamlanır. Pilot sonuçları sayısal toleransları ve kalan iş tahminini kesinleştirir.

## 1 Yönetici Özeti

ROBOTIS MuJoCo Menagerie, çalıştırılabilir bir uygulamadan çok MJCF model tanımları ile STL geometrilerini bir araya getiren bir kaynak koleksiyonudur. Sabitlenen revizyon altı model ailesinde dokuz model ve bunlara ait dokuz sahne sunar. Depoda bağımlılık kilidi, kurulum betiği, test paketi ve CI iş akışı bulunmadığı için yalnızca klonlama işlemi yeniden üretilebilir bir teslimat oluşturmaz \[1\]\[3\].

- Kaynak tabanı main dalındaki d8344c0dbe7a00208d0301111523dde65efc174a commitidir. Commit 2 Şubat 2026 tarihinde gripper eşitlik yönlerini ve ilgili kararlılık ayarlarını düzeltmiştir \[2\]\[4\].

- Yerel statik inceleme 188 dosya, 18 XML, 149 STL, 9 PNG ve 1 GIF bulmuştur. Toplam çalışma ağacı yaklaşık 193,3 MB, STL toplamı yaklaşık 180,1 MB'dir.

- On sekiz XML dosyasının tümü iyi biçimlidir. Dokuz sahne include referansının ve model XML'lerindeki tüm mesh yollarının yüzde 100'ü mevcut dosyalara çözülmüştür.

- Kök ve altı model dizinindeki yedi LICENSE dosyasının tamamı Apache License 2.0 metnidir. Dağıtımda lisanslar, atıflar ve değiştirilen dosya bildirimleri korunmalıdır \[3\]\[7\].

- Model XML'lerinde sensor öğesi yoktur. Kamera, IMU veya LiDAR eklemek mevcut kaynakların bire bir yeniden üretimi değil, ayrı kapsam değişikliğidir.

- Önerilen çalışma 48 kişi gündür. İki çekirdek mühendis, kısmi zamanlı teknik lider ve sınırlı lisans incelemesiyle altı takvim haftası planlanmıştır.

Başarı ölçüsü, tüm dosyaların kaynak manifestiyle eşleşmesi ve 18 XML girişinin iki sürüm hattında derlenmesidir. Yapısal imzalar değişmemeli; dinamik testler NaN veya sonsuz değer üretmemeli; gripper eşitlikleri, mobil taban yönleri ve model bazlı temas davranışı onaylanmış toleranslarda kalmalıdır.

## 2 Kaynak Tabanı ve Doğrulanmış Envanter

### 2 1 Kaynak Kilidi

| **Alan** | **Doğrulanmış değer** | **Plan kararı** |
| --- | --- | --- |
| Kesim tarihi | 21.11.2026 | Bu tarihten sonraki upstream değişiklikler kapsam dışıdır |
| Dal | main | Dal adı yalnızca izleme için kullanılır |
| Commit | d8344c0dbe7a00208d0301111523dde65efc174a | Tüm indirme ve manifest işlemleri tam SHA ile sabitlenir |
| Commit tarihi | 02.02.2026 09:01:25 UTC | Kesim tarihinden önceki son kaynak tabanı |
| Sürüm kaydı | CHANGELOG 1.1.0 | Etiket yerine commit SHA esas alınır |
| GitHub Releases | En yeni yayımlanmış kayıt 0.2.0 | Release kaydı ile changelog farkı karar günlüğüne yazılır |
| Alt depo ve LFS | Git submodule ve Git LFS yapılandırması yok | Büyük STL blobları doğrudan manifest kapsamındadır |

GitHub Releases sayfası 0.1.0 ve 0.2.0 kayıtlarını gösterirken changelog 1.1.0'a kadar ilerler \[4\]\[5\]. Bu uyumsuzluk nedeniyle yeniden üretim kimliği sürüm etiketi değil, tam commit SHA olmalıdır.

### 2 2 Model ve Varlık Envanteri

| **Model ailesi** | **Varyantlar** | **XML** | **STL** | **Kritik yapı** |
| --- | --- | --- | --- | --- |
| robotis\_ffw | FFW BG2<br>FFW SG2<br>FFW SH5 | 6 | 66 | Sabit ve serbest tabanlar; konum ve hız aktüatörleri; mimic ve yoğun temas dışlamaları |
| robotis\_omx | OMX | 2 | 8 | Altı konum aktüatörü; bir gripper equality; home keyframe |
| robotis\_omy | OpenMANIPULATOR Y | 2 | 13 | Yedi konum aktüatörü; üç gripper equality; home keyframe |
| robotis\_op3 | OP3 | 2 | 48 | Serbest taban; 20 konum aktüatörü; ayrı sadeleştirilmiş convex çarpışma ağları |
| robotis\_open\_manipulator\_x | OpenMANIPULATOR X | 2 | 8 | Beş konum aktüatörü; bir gripper equality; home keyframe |
| robotis\_tb3 | TurtleBot3 Burger<br>TurtleBot3 Waffle Pi | 4 | 6 | Serbest taban; iki hız kontrollü tekerlek |

Bire bir kaynak tesliminde model XML'leri tarafından kullanılmayan ancak Git ile izlenen beş STL de korunur: FFW için üç steer link dosyası, OpenMANIPULATOR X için Plate.stl ve TurtleBot3 için waffle\_base.stl. Bu dosyaları temizlemek ayrı ve belgelenmiş bir türev çalışma olur.

### 2 3 Statik Doğrulama Bulguları

- On sekiz XML dosyası ayrıştırılmış ve tüm include ile mesh file referansları yerel dosyalara çözülmüştür. Eksik referans sayısı sıfırdır.

- Depoda requirements.txt, pyproject.toml, Dockerfile, Makefile, test klasörü veya GitHub Actions iş akışı yoktur.

- Model ve sahne ayrımı korunmalıdır. Sahne XML'leri aynı klasördeki model XML'ini include eder; model XML'leri göreli meshdir kullanır.

- Çalışma dizinini düzleştirmek veya asset klasörlerini yeniden adlandırmak yüklemeyi bozar. Linux üzerinde büyük küçük harf duyarlılığı ayrıca test edilmelidir.

- Bu raporun yerel doğrulaması statik kaynak incelemesidir. MuJoCo derleme ve simülasyon sonuçları Kapı 3 ile Kapı 5 kapsamında üretilecektir.

## 3 Yeniden Üretim Tanımı ve Kapsam

### 3 1 İki Kanıt Katmanı

Arşiv eşliği, sabit committeki dizin yapısını, XML dosyalarını, binary assetleri, önizleme görsellerini ve lisansları SHA256 manifestiyle korur. İşlevsel eşlik ise aynı kaynakların sabitlenmiş ortamda derlenmesini ve yapısal ile dinamik ölçümlerde kayıtlı baselinelara uymasını gösterir.

Tamamlanmış yeniden üretim her iki katmanı da gerektirir. Görsel benzerlik tek başına yeterli değildir; çarpışma ağları, ataletler, eklem limitleri, aktüatörler, equality kısıtları ve temas dışlamaları da kaynakla eşleşmelidir.

### 3 2 Kapsam İçinde

- Dokuz model XML'i, dokuz sahne XML'i, 149 STL, 9 PNG, 1 GIF ve tüm lisans dosyaları

- Commit, dosya boyutu ve SHA256 içeren kaynak manifesti

- MuJoCo 3.4.0 sadakat hattı ve MuJoCo 4.0.1 güncel uyumluluk hattı

- Sabit Python sürümü, paket hashleri, kanonik Linux container ve Windows PowerShell başlatıcısı

- Statik XML ve asset denetimi, derleme testi, yapısal imza, dinamik regresyon ve sabit kamera görsel kontrolü

- Yerel tek komutlu doğrulama, CI tanımı, model kataloğu, lisans matrisi ve bilinen sınırlamalar

### 3 3 Kapsam Dışında

- STL geometrilerini CAD kaynaklarından yeniden çizmek veya tersine mühendislik yapmak

- LeRobot eğitim ve veri toplama akışını yeniden üretmek

- Gerçek robot üzerinde sistem tanımlama, motor kalibrasyonu veya sim to real garantisi vermek

- Kaynakta bulunmayan kamera, IMU, LiDAR, denetleyici veya görev ortamı eklemek

- ROBOTIS ya da başka bir upstream depoya değişiklik göndermek

- Yeni upstream commitleri otomatik olarak bu kaynak tabanına almak

### 3 4 Plan Varsayımları

- Kaynak binary assetler değiştirilmeden kullanılır ve hashleri teslim boyunca sabit kalır.

- Donanım erişimi gerekmez; kabul yazılım ve simülasyon eşliğini ölçer.

- Kanonik koşum Linux container içinde yapılır; Windows kullanımı aynı kilit dosyasına bağlı başlatıcıyla desteklenir.

- Model sayısı dokuzda, kaynak kesimi 21.11.2026 tarihinde sabit kalır.

- Pilot testler sayısal toleransları ve efor tahminini yüzde 25 belirsizlik aralığından daha dar bir aralığa indirir.

## 4 MuJoCo Sürüm Stratejisi

OP3 README dosyası yalnız OP3 modeli için MuJoCo 2.2.2 veya sonrasını belirtir \[6\]. Ancak koleksiyonun diğer XML dosyaları implicitfast, inheritrange ve dampratio kullanır. Bu öğeler sırasıyla 2.3.3, 3.1.3 ve 3.1.6 sürümlerinde eklenmiştir. Bu nedenle tam koleksiyon için sözdizimsel alt sınır 3.1.6'dır \[8\].

| **Sürüm** | **Rol** | **Gerekçe** | **Kabul yaklaşımı** |
| --- | --- | --- | --- |
| 3.1.6 | Sözdizimsel alt sınır | dampratio bu sürümde eklendi | Yalnız geriye dönük derleme araştırması; üretim baseline değil |
| 3.4.0 | Tarihsel sadakat adayı | Repo HEAD tarihinde yayımlanmış en yeni MuJoCo sürümü | Birincil model imzaları ve golden dinamik koşular burada üretilir |
| 4.0.1 | 21.11.2026 güncel uyumluluk hattı | 11 Kasım 2026 sürümü; 4.0.0 solver hatasını düzeltir | Aynı testler çalışır; farklar ayrı baseline ve değişim raporuyla saklanır |
| 4.0.0 | Hariç | Seyrek Cholesky hatası yanlış ivme ve sapma üretebilir | CI ve kabul ortamında kullanılmaz |

MuJoCo 3.15.0 dampratio hesabını değiştirmiştir. Çok eklemli zincirlerde 3.4.0 ve 4.0.1 aynı XML'den farklı damping üretebilir. Bu fark hata olarak gizlenmemeli; sürüm kaynaklı davranış değişimi olarak ölçülüp iki hatta ayrı tolerans tanımlanmalıdır \[8\].

MuJoCo 4.0.0 ile Python dağıtımı mujoco-core ve ek paketlere ayrılmıştır \[9\]. Ortam kilidi, paket adı, tam sürüm, Python sürümü ve wheel hashlerini birlikte kaydetmelidir. Headless koşum ile masaüstü viewer bağımlılıkları ayrı katmanlarda tutulmalıdır.

## 5 Teknik Yeniden Üretim Akışı

**1 Kaynak tabanını sabitle:** Depoyu tam commit SHA ile indir, detached HEAD kullan ve kaynak kilidini makinece okunur dosyaya yaz.

**2 Arşiv manifestini üret:** İzlenen 188 dosya için göreli yol, byte boyutu ve SHA256 kaydet; yedi lisans dosyasını ayrıca sınıflandır.

**3 Ortamları kilitle:** MuJoCo 3.4.0 ve 4.0.1 için aynı doğrulama arayüzünü kullanan iki paket kilidi üret; container ve Windows başlatıcısını buna bağla.

**4 Statik denetimi çalıştır:** XML iyi biçimliliğini, include ve mesh yollarını, dosya adı büyük küçük harf eşliğini ve STL okunabilirliğini kontrol et.

**5 Derlenmiş model imzasını çıkar:** MjModel.from\_xml\_path ile her model ve sahneyi derle; nq, nv, nu, nbody, njnt, ngeom, neq, nkey, timestep ve ad listelerini JSON olarak sakla \[10\].

**6 Dinamik ve görsel regresyonu kur:** Sabit başlangıç durumları ile kontrol dizilerini çalıştır; sayısal sağlık, equality residual, hareket yönü ve temas metriklerini kaydet; görüntüyü ikincil kanıt olarak kullan.

**7 Temiz makine teslimini doğrula:** Yeni bir ortamda tek komutla bootstrap, test, rapor ve kanıt paketini üret; yerel ve CI çıktılarının aynı manifesti verdiğini kontrol et.

### 5 1 Evrensel Test Katmanları

| **Katman** | **Kontrol** | **Üretilen kanıt** |
| --- | --- | --- |
| Kaynak | Commit, dosya sayısı, SHA256, temiz çalışma ağacı | SOURCE\_LOCK ve SHA256SUMS |
| Statik | XML, include, meshdir, yol case, STL başlık ve geometri okunabilirliği | static\_audit.json |
| Derleme | 18 XML için parse ve compile; uyarı allowlist | compile\_report.json ve loglar |
| Yapısal | Gövde, eklem, aktüatör, equality, keyframe, mesh ve parametreler | model\_signatures dizini |
| Dinamik | Sabit kontrol dizileri, NaN ve inf, limit, kuvvet, residual ve temas | trajectory ve metrics dosyaları |
| Görsel | Sabit kamera, çözünürlük ve renderer | Dokuz golden görüntü ve fark raporu |
| Paket | Temiz makine ve tek komut | Kabul raporu ve yeniden üretim kılavuzu |

### 5 2 Ortam ve Yol Dayanıklılığı

- Model yüklemesi repo kökünden ve repo dışındaki bir çalışma dizininden çalıştırılmalıdır.

- Linux container testi büyük küçük harf hatalarını, Windows testi boşluk ve Türkçe karakter içeren mutlak yol kullanımını yakalamalıdır.

- XML dosyalarında açık timestep bulunmadığı için derlenmiş timestep her sürüm hattında imzaya yazılmalıdır.

- Görsel test için tek bir headless backend ve sabit renderer seçilmelidir; backend farkı fizik regresyonu olarak yorumlanmamalıdır.

## 6 Model Bazlı Doğrulama Planı

| **Model** | **Taban ve aktüatör** | **Kritik yapı** | **Hedef test** |
| --- | --- | --- | --- |
| FFW BG2 | Sabit taban<br>19 konum | 6 mimic equality<br>21 contact exclude | Master ve follower oranları; baş ve gripper hareketi; temas dışlama regresyonu |
| FFW SG2 | Serbest taban<br>25 konum ve hız | 6 mimic equality<br>26 contact exclude | Sürüş ve direksiyon ayrımı; ileri ve dönüş yönü; gripper oranları |
| FFW SH5 | Serbest taban<br>63 konum ve hız | İki 5 parmaklı el<br>52 contact exclude | Her parmak ekleminin limit ve yönü; el çarpışma kararlılığı; mobil taban |
| OMX | Sabit taban<br>6 konum | 1 equality<br>home keyframe | Gripper\_mimic ekleminin Gripper eklemini eksi bir oranla izlemesi |
| OMY | Sabit taban<br>7 konum | 3 equality<br>home keyframe | rh\_l1 ve rh\_r2 için eksi bir; rh\_l2 için artı bir oran; damping ve residual |
| OpenMANIPULATOR X | Sabit taban<br>5 konum | 1 equality<br>home keyframe | Gripper senkronizasyonu; home yerleşimi; izlenen Plate.stl dosyasının korunması |
| OP3 | Serbest taban<br>20 konum | 48 mesh<br>convex çarpışma seti | Ayak teması; düşme sonrası sayısal kararlılık; görsel ve çarpışma ağlarının ayrı doğrulanması |
| TurtleBot3 Burger | Serbest taban<br>2 hız | Diferansiyel sürüş | Eş teker komutunda ileri hareket; ters işaretli komutta dönüş; zemin teması |
| TurtleBot3 Waffle Pi | Serbest taban<br>2 hız | Diferansiyel sürüş | Burger ile aynı yön testleri; waffle\_base.stl dosyasının kaynak manifestinde korunması |

Equality residual eşikleri, eklem ve kontrol toleransları ilk iki pilotta ölçülmelidir. Platformlar arasında kanıtlanmadan keyfi bir mutlak eşik seçilmemelidir. 3.4.0 hattı kanonik dinamik baseline, 4.0.1 hattı ise sürüm geçişi sonucu olarak ayrı değerlendirilir.

## 7 Fazlar ve İş Paketleri

| **İş paketi** | **İçerik** | **Efor** | **Çıkış kapısı** |
| --- | --- | --- | --- |
| WP1 Kaynak ve hukuk tabanı | Commit kilidi, dosya ve hash manifesti, lisans matrisi, kapsam sözleşmesi | 3 kişi gün | Kaynak tabanı onaylı |
| WP2 Deterministik ortam | İki MuJoCo hattı, Python kilidi, container, Windows başlatıcısı, headless GL | 6 kişi gün | 18 XML iki hatta derleniyor |
| WP3 Küçük ve orta modeller | OMX, OMY, OpenMANIPULATOR X ve iki TurtleBot3 modeli | 10 kişi gün | Beş model tüm hedef testleri geçiyor |
| WP4 Karmaşık modeller | OP3 ile FFW BG2, SG2 ve SH5; temas ve equality testleri | 15 kişi gün | Dört karmaşık model kabul edildi |
| WP5 Otomasyon ve CI | Statik, derleme, yapısal, dinamik ve görsel regresyon | 9 kişi gün | Yerel ve CI kanıtları aynı |
| WP6 Belgeler ve sürüm | Kurulum, katalog, lisans raporu, kabul özeti ve temiz makine paketi | 5 kişi gün | Teslim paketi yeniden üretildi |

Toplam ön tahmin 48 kişi gündür. Pilot kapısından sonra kalan efor yeniden tahmin edilir.

### 7 1 Altı Haftalık Takvim

| **Hafta** | **Ana çalışma** | **Hafta sonu sonucu** |
| --- | --- | --- |
| 1 | Kaynak kilidi, lisans, iki ortam ve statik denetim | 18 XML için ilk derleme raporu |
| 2 | OpenMANIPULATOR X ve TurtleBot3 Burger pilotları | Toleranslar, test şablonu ve revize tahmin |
| 3 | OMX, OMY, Waffle Pi ve OP3 | Dört ek modelin yapısal ve dinamik kabulü |
| 4 | FFW BG2, SG2 ve SH5 | Equality, mobil taban, parmak ve temas testleri |
| 5 | CI, Windows yol testi ve görsel regresyon | İki sürümlü otomatik kanıt paketi |
| 6 | Temiz makine, belgeler, lisans ve sürüm kapısı | Onaylanmış yerel teslim |

### 7 2 Kritik Yol

Kaynak manifesti ve ortam kilidi tamamlanmadan dinamik baseline üretilemez. Pilot modeller kabul edilmeden karmaşık FFW toleransları sabitlenmemelidir. CI ve temiz makine kabulü, tüm model imzaları donduktan sonra yapılmalıdır.

## 8 Kabul Kapıları

| **Kapı** | **Ölçüt** | **Zorunlu kanıt** |
| --- | --- | --- |
| Kapı 0 Kaynak bütünlüğü | Commit tam SHA ile eşleşir; 188 dosyanın yolu, boyutu ve SHA256 değeri kayıtlıdır; kaynak dışı asset sessizce eklenmez | SOURCE\_LOCK, SHA256SUMS ve temiz git çıktısı |
| Kapı 1 Lisans ve atıf | Yedi LICENSE dosyası pakettedir; Apache 2.0 yükümlülükleri, OP3 kökeni ve marka sınırı belgelenir | Lisans matrisi ve dağıtım kontrol listesi |
| Kapı 2 Statik geçerlilik | 18 XML parse edilir; 149 STL hash ve okunabilirlik kontrolünü geçer; include ve mesh referans hatası sıfırdır | static\_audit.json ve case sensitive Linux koşusu |
| Kapı 3 MuJoCo derleme | 18 XML hem 3.4.0 hem 4.0.1 hattında derlenir; uyarılar sıfır veya onaylı allowlist içindedir | compile\_report, loglar ve sürüm kilitleri |
| Kapı 4 Yapısal eşlik | Gövde, eklem, aktüatör, equality, keyframe, mesh, limit, kütle ve atalet imzaları kaynak baseline ile eşleşir | Model başına JSON imzası ve fark raporu |
| Kapı 5 Dinamik eşlik | NaN veya inf yoktur; mimic oranları, teker yönleri, FFW hareketi ve OP3 temas davranışı onaylı toleranslardadır | Sabit kontrol dizileri, trajectory ve metrik raporu |
| Kapı 6 Temiz makine | Yeni checkout tek komutla kurulur ve aynı model kataloğu ile test özetini üretir; gizli yerel duruma bağlı değildir | Temiz makine koşu kaydı ve kabul raporu |

Bir kapı başarısızsa sürüm adayı yayımlanmaz. Bilinçli sapma için neden, etkilenen model, ölçüm, sahip ve onay kaydı aynı değişim belgesinde bulunmalıdır.

## 9 İlk On İş Günü

| **Gün** | **Çalışma** | **Gün sonu çıktısı** |
| --- | --- | --- |
| 1 | Commit ve kapsam sözleşmesini sabitle | Kaynak kilidi ve kabul tanımı |
| 2 | Dosya ağacı, SHA256 ve lisans envanterini üret | Kaynak ve lisans manifesti |
| 3 | 3.4.0 ve 4.0.1 ortam adaylarını kur | İki sürümlü uyumluluk matrisi |
| 4 | Kanonik container ve Windows başlatıcısını bağla | Tekrarlanabilir çalışma ortamı |
| 5 | XML, include, mesh ve STL statik denetleyicisini tamamla | 18 XML ve 149 STL denetim raporu |
| 6 | Tüm model ve sahnelerde compile smoke testi yap | İlk model imzaları ve uyarı listesi |
| 7 | OpenMANIPULATOR X pilotunu tamamla | Manipülatör golden baseline |
| 8 | TurtleBot3 Burger pilotunu tamamla | Mobil taban golden baseline |
| 9 | Dinamik ve görsel regresyon altyapısını bağla | Sabit koşular ve dokuz kamera tanımı |
| 10 | Pilot kapı incelemesini yap ve tahmini güncelle | Onaylı toleranslar ve revize takvim |

## 10 Rol ve Efor Planı

| **Rol** | **Sorumluluk** | **Efor** |
| --- | --- | --- |
| Robotik ve simülasyon mühendisi | Model imzaları, dinamik testler, model ailesi kabulü | 24 kişi gün |
| Test ve otomasyon mühendisi | Manifest, test koşucusu, CI, yol ve temiz makine testleri | 10 kişi gün |
| Teknik lider | Kaynak kararı, sürüm stratejisi, toleranslar, kapı onayı ve risk yönetimi | 12 kişi gün |
| Lisans ve yayın incelemesi | Lisans matrisi, atıf, değişiklik bildirimi ve paket kontrolü | 2 kişi gün |
| Toplam | Altı haftalık plan | 48 kişi gün |

İki çekirdek mühendis ve kısmi zamanlı teknik liderle altı takvim haftası gerçekçidir. Tek tam zamanlı mühendisle aynı kapsam yaklaşık dokuz ile on hafta sürer. Donanım doğrulaması eklenirse ayrı efor ve ekipman planı gerekir.

## 11 Riskler ve Önlemler

| **Risk** | **Olasılık** | **Etki** | **Kontrol** |
| --- | --- | --- | --- |
| main dalının değişmesi | Yüksek | Yüksek | Tam commit SHA, detached checkout ve hash manifesti |
| Changelog ile GitHub release kaydının uyuşmaması | Yüksek | Orta | Sürüm etiketi yerine commit kimliği ve karar günlüğü |
| MuJoCo semantiğinin sürümler arasında değişmesi | Yüksek | Yüksek | 3.4.0 ve 4.0.1 için ayrı baseline; dampratio fark raporu |
| Test ve CI eksikliği | Yüksek | Yüksek | Statik, derleme, yapısal, dinamik ve görsel test katmanları |
| Göreli yol ve dosya adı case sorunu | Orta | Yüksek | Linux case sensitive test, repo dışı çalışma dizini ve Windows Unicode yol testi |
| Büyük STL dosyalarının normal Git blobu olması | Orta | Orta | İndirme bütünlüğü, SHA256 ve temiz cache davranışı |
| FFW equality ve contact exclude regresyonu | Orta | Yüksek | Hedefli master follower ve temas testleri |
| Fiziksel doğruluk iddiasının kaynakta kanıtlanmaması | Yüksek | Yüksek | Yazılım eşliği ile gerçek robot doğrulamasını teslimde açıkça ayır |
| Headless renderer farkları | Orta | Orta | Tek backend; görsel testi ikincil kanıt olarak kullan |
| Lisans veya marka atfının eksik kalması | Düşük | Yüksek | Yedi lisans dosyası, OP3 kaynak atfı ve yayın kontrol listesi |
| Kapsamın CAD veya sensör geliştirmeye kayması | Orta | Yüksek | Kapı 0 kapsam sözleşmesi ve ayrı değişiklik tahmini |

## 12 Teslimatlar ve Hedef Klasör Yapısı

- SOURCE\_LOCK.json ile tam commit, kaynak URL ve kesim tarihi

- SHA256SUMS ile 188 dosyanın göreli yolu, boyutu ve hash değeri

- 3.4.0 ve 4.0.1 için ayrı ortam kilitleri ve container tanımı

- Statik denetleyici, derleme koşucusu, yapısal imza ve dinamik regresyon testleri

- Model başına JSON imzaları, metrikler, golden görüntüler ve fark raporları

- CI tanımı, Windows başlatıcısı ve temiz makine kabul kaydı

- REPRODUCING.md, model kataloğu, lisans matrisi, risk ve bilinen sınırlamalar

Aşağıdaki yapı uygulama aşamasında oluşturulacak hedef teslim düzenidir. Bu plan dosyası yalnız yerel Ürün 3 klasöründe tutulur; uygulama klasörleri iş başlangıcında açılır.

urun-3/<br>source/<br>manifest/<br>SOURCE\_LOCK.json<br>SHA256SUMS<br>env/<br>mujoco-3.4.0/<br>mujoco-4.0.1/<br>validation/<br>evidence/{model-signatures,trajectories,renders}/<br>docs/

## 13 Başlatma Kararı

Çalışma, commit SHA ve iki katmanlı yeniden üretim tanımı onaylandığında başlatılabilir. İlk iki gün kaynak ile lisans tabanını dondurur; gün 3 ile gün 6 ortam ve derleme riskini ortadan kaldırır; gün 7 ile gün 10 iki pilotla toleransları sabitler.

Başlangıçta alınması gereken dört karar vardır: kanonik CI platformu, iki MuJoCo hattında ortak Python sürümü, headless renderer ve kanıtların saklanacağı yerel sürümleme düzeni. Bunlar kaynak kapsamını değiştirmez; yalnız uygulama ayrıntılarını belirler.

Lisans incelemesinde yeni bir kısıt çıkarsa, 18 XML'den herhangi biri iki sürüm hattında da derlenmezse veya kapsam CAD ile donanım doğrulamasını içerecek biçimde genişlerse plan yeniden tahmin edilmelidir. Diğer durumlarda önerilen altı haftalık program uygulanabilir.

## 14 Kaynaklar

Kaynak bağlantıları 21.11.2026 kesimine göre seçilmiş, repo bağlantıları sabit commit SHA ile verilmiştir. MuJoCo değişiklik günlüğü sürüm tarihleri ve davranış değişiklikleri için birincil kaynaktır.

**\[1\] ROBOTIS MuJoCo Menagerie sabit kaynak ağacı:** [Kaynağı aç](https://github.com/ROBOTIS-GIT/robotis_mujoco_menagerie/tree/d8344c0dbe7a00208d0301111523dde65efc174a)

**\[2\] Sabitlenen commit ve equality bug fix:** [Kaynağı aç](https://github.com/ROBOTIS-GIT/robotis_mujoco_menagerie/commit/d8344c0dbe7a00208d0301111523dde65efc174a)

**\[3\] Kök README ve model ile lisans özeti:** [Kaynağı aç](https://github.com/ROBOTIS-GIT/robotis_mujoco_menagerie/blob/d8344c0dbe7a00208d0301111523dde65efc174a/README.md)

**\[4\] ROBOTIS MuJoCo Menagerie değişiklik günlüğü:** [Kaynağı aç](https://github.com/ROBOTIS-GIT/robotis_mujoco_menagerie/blob/d8344c0dbe7a00208d0301111523dde65efc174a/CHANGELOG.rst)

**\[5\] GitHub Releases kayıtları:** [Kaynağı aç](https://github.com/ROBOTIS-GIT/robotis_mujoco_menagerie/releases)

**\[6\] OP3 model notları ve MuJoCo alt sınırı:** [Kaynağı aç](https://github.com/ROBOTIS-GIT/robotis_mujoco_menagerie/blob/d8344c0dbe7a00208d0301111523dde65efc174a/robotis_op3/README.md)

**\[7\] Kök Apache License 2.0 metni:** [Kaynağı aç](https://github.com/ROBOTIS-GIT/robotis_mujoco_menagerie/blob/d8344c0dbe7a00208d0301111523dde65efc174a/LICENSE)

**\[8\] MuJoCo resmî değişiklik günlüğü:** [Kaynağı aç](https://mujoco.readthedocs.io/en/latest/changelog.html)

**\[9\] MuJoCo resmî Python bağları ve paket yapısı:** [Kaynağı aç](https://mujoco.readthedocs.io/en/latest/python.html)

**\[10\] MuJoCo model ayrıştırma ve derleme akışı:** [Kaynağı aç](https://mujoco.readthedocs.io/en/latest/programming/modeledit.html)

**\[11\] MuJoCo MJCF XML başvuru kılavuzu:** [Kaynağı aç](https://mujoco.readthedocs.io/en/latest/XMLreference.html)

**\[12\] ROBOTIS OP3 kaynak URDF tanımı:** [Kaynağı aç](https://github.com/ROBOTIS-GIT/ROBOTIS-OP3-Common/tree/master/op3_description/urdf)

**\[13\] Google DeepMind MuJoCo Menagerie OP3 modeli:** [Kaynağı aç](https://github.com/google-deepmind/mujoco_menagerie/tree/main/robotis_op3)
