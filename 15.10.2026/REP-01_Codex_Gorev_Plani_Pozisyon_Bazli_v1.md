REP-01 Codex Görev Planlaması

ESP32-S3 24V Interface Board için pozisyon bazlı yürütme planı

Kaynak: REP-01 İş Akışı Raporu v1.0  |  Tarih: 16.10.2026  |  Repo: 002-Proje Çıktıları/esp32-s3-24v-interface-board

# Amaç ve yürütme kararı

Bu rapor, kaynak iş akışındaki WF01–WF06 iş paketlerini Codex’in yürütebileceği sıralı görevlere ve kopyalanıp kullanılabilir promptlara dönüştürür. İş paketleri ayrı pozisyonlar olarak ele alınmıştır; yeni pozisyon eklenmemiştir. İlk yürütme WF01 envanter ve gereksinim taslaklarıdır. WF02–WF04 yalnız araştırma, karşılaştırma ve sentetik taslak seviyesinde yürütülebilir. G1 halen BLOCKED olduğundan WF05 şematik üretimi, G3 PASS olmadan da WF06 PCB yerleşimi başlamaz.

Planlama ilkesi: Codex önce salt okunur envanter yapar; her çıktı müşteri şartını, türetilmiş sonucu, mühendislik kuralını ve bilinmeyeni ayırır. Kaynak raporda bulunmayan kararlar varsayılmaz; eksik bilgi UNKNOWN veya BLOCKED olarak kaydedilir.

# Durum ve kritik sınırlar

| **Durum / karar** | **Plan üzerindeki etkisi** |
| --- | --- |
| G0 PASS | Sürüm ve girdilerin güvenli başlangıç koşulları doğrulandı kabul edilir. |
| G1 BLOCKED | Kaynak hash/lisans/provenans ve müşteri gereksinimleri tamamlanmadan tasarım girdileri kilitlenemez; WF05 başlatılamaz. |
| G2–G6 NOT\_STARTED | Mimari, şema, PCB ve üretim paket kapıları sırayla kanıtlanmalıdır. |
| CF-09 / Q-C07 açık | Beş TTL TX + beş RS-232 TX kanalının bağımsızlığı ve frozen GPIO doğrulanana kadar 10 TX topolojisi BLOCKED. |
| G3 PASS şartı | WF06 layout ancak G3 kanıt paketi kabul edildikten sonra başlar. |
| Ayrı onay gereken işlemler | Git init/commit/push, paket kurulumu, Drive yazma, silme ve mevcut dosyanın üzerine yazma için kullanıcı onayı gerekir. |

# Pozisyon haritası ve bağımlılık

| **Pozisyon** | **İş kapsamı** | **Başlama koşulu** |
| --- | --- | --- |
| WF01 | Requirement & Interface Contract | G0; repo ve kaynaklar salt okunur envantere alınır. |
| WF02 | Power Entry Architecture | WF01 REQ-001 taslağı; Q-C01…03 ve CF-01/06 kayıtları. |
| WF03 | Power Tree & Regulator Design | REQ-002 ve WF02 seçenek taslakları; Q-C04…06. |
| WF04 | MCU & Peripheral Integration | REQ-003/004, CLAUDE-03 ve GPIO/arayüz kayıtları. |
| WF05 | Schematic & Pre-layout Verification | G1 PASS + G2 mimari kabulü + WF01–04 girdileri. |
| WF06 | PCB & Manufacturing Package | G3 PASS + WF05 çıktıları. |

WF02–WF04 araştırma görevleri bilgi toplama bakımından paralel yürütülebilir; kararları kilitleyen kabul sırası WF01 → WF02/WF03/WF04 → G1/G2 → WF05 → G3 → WF06 şeklindedir.

# Ortak Codex çalışma sözleşmesi

- Çalışma ağacını önce incele; repo yoksa oluşturma. Mevcut dosyaları değiştirmeden önce listele ve planla.

- İlk çalışma salt okunur olsun. Dosya silme, üzerine yazma, Git başlatma/commit/push, paket kurma veya Drive’a yazma yapma; gerekiyorsa işlem ve etkiyi raporla, onay bekle.

- Her iddiayı SOURCE\_EXPLICIT, DERIVED, ENGINEERING\_RULE, HANDOFF\_ONLY veya UNKNOWN olarak etiketle. Kaynakta yoksa sayı, eşik, parça numarası ya da müşteri kararı uydurma.

- Başarı raporunda PASS / BLOCKED / UNKNOWN / NOT\_RUN kullan. Araç yoksa ERC, DRC, simülasyon veya fiziksel doğrulama yapılmış gibi gösterme.

- Kaynakların revizyonu, dosya yolu, SHA-256 ve sayfa/bölümü kaydet. Birincil kaynakla doğrulanmayan iddiaları UNKNOWN bırak.

- Her görev sonunda değişen dosyaları, değişmeyen dosyaları, kanıtları, açık soruları ve sonraki güvenli adımı listele.

# WF01 — Requirement & Interface Contract

Amaç: Müşteri şartlarını ve iç tasarım kurallarını izlenebilir gereksinimlere, arayüz sözleşmesine ve açık karar listesine çevirmek.

**Girdiler:** REQ-001…005; CLAUDE-06/07; müşteri ilanları; repo dosyalarının ve PKG-A/B kayıtlarının salt okunur envanteri.

**Beklenen teslimatlar:** requirements-traceability.csv; interface-contract.md; gpio-budget.csv; open-decisions.md.

## Görev adımları

1. Repo ve çalışma ağacını salt okunur incele; dosya envanteri, Git durumu ve CLAUDE-06/09/10 kayıtlarını raporla.

2. Kaynakların hash, lisans, sürüm ve provenans durumunu G1 girdileri olarak ayır.

3. Müşteri kaynaklı şartları iç kurallardan ayırarak REQ-001…005 izlenebilirlik tablosunu taslakla.

4. USB, TTL, RS-232, LCD, CAN ve debug arayüzleri için yön, gerilim, güç alanı, power-off hali, kaynak ve doğrulama yöntemini tanımla.

5. GPIO bütçesi ve CF-09 alternatiflerini yalnız bilgi toplama seviyesinde çıkar; belirsizlikleri Q-C07 ve diğer açık kararlarla bağla.

## Bloklayıcılar ve sınırlar

G1 halen BLOCKED; gereksinim dosyalarının ana sürümünü onaysız değiştirme. CF-09 (10 TX bağımsızlığı/frozen GPIO) çözülmeden seri mimariyi kilitleme. Belge kaynakları yoksa eksikliği kaydet, varsayım ile doldurma.

## Pozisyon kabul ölçütü

Her gereksinim kaynak ve kanıt sınıfına bağlıdır; tüm arayüzlerde gerekli sözleşme alanları mevcut veya açık blocker’dır; taslak ile onaylı gereksinim ayrımı görünürdür.

## Codex prompt zinciri

### Prompt 1 — Repo ve kanıt envanteri

Bu repoda salt okunur başlangıç envanteri yap. Repo kökü: 002-Proje Çıktıları/esp32-s3-24v-interface-board. Dosyaları, mevcut Git durumunu, CLAUDE-06/09/10 ve PKG-A/B ile ilişkili kayıtları bul; hiçbir dosyayı değiştirme. G0 durumunu, kaynak hash/lisans/provenans durumunu ve G1 için eksik kanıtları tablo halinde raporla. Dosya bulunamazsa UNKNOWN yaz. Git init, commit, push, kurulum veya Drive yazma yapma.

### Prompt 2 — Gereksinim ve arayüz taslakları

Yalnız doğrulanmış kaynakları kullanarak requirements-traceability.csv, interface-contract.md, gpio-budget.csv ve open-decisions.md taslaklarını çalışma alanında hazırla. Müşteri şartı ile iç mühendislik kuralını ayrı kaydet. Her satıra kaynak, revizyon/sayfa-bölüm, iddia sınıfı ve doğrulama yöntemi ekle. USB, 5 TTL TX, 5 RS-232 TX + 1 RX, 8x2 HD44780, iki buton, Wi-Fi, CAN/TWAI, debug ve DNP arayüzlerini kapsa. Kaynakta olmayan pin, akım veya eşik uydurma. Mevcut dosyaların üzerine yazma; çakışma varsa yeni taslak adı kullan ve listele.

### Prompt 3 — CF-09 ve G1 durum değerlendirmesi

Taslakları denetle. CF-09 için bağımsız TX akışları ve frozen GPIO kanıtı mevcut mu? Yoksa UART topolojisini BLOCKED olarak bırak. G1 kriterlerini (kaynak hash, lisans/provenans, müşteri gereksinimleri, birincil kaynak eşlemesi) tek tek PASS/BLOCKED/UNKNOWN olarak değerlendir. Eksiklerin sahibi müşteri/araştırma/Codex olarak öneri düzeyinde atanabilir; kararı verilmiş gibi gösterme. Yalnız bulguları raporla ve hangi revizyonun G1 onayı için gerekli olduğunu açıkla.

# WF02 — Power Entry Architecture

Amaç: 24 V giriş için koruma mimarisi alternatiflerini kanıt sınıflarıyla kıyaslamak; koşullu seçenekleri sunup kesin parça seçimini ertelemek.

**Girdiler:** REQ-001; Q-C01…03; CF-01/06; varsa doğrulanmış OEM transient profili ve ilgili kaynaklar.

**Beklenen teslimatlar:** power-entry-options.md; protection-comparison.csv; transient-scenarios.json.

## Görev adımları

1. WF01 gereksinim taslağındaki 24 V VIN aralığı ve koruma şartlarını al; eksikleri kaydet.

2. M1 TVS + uygun buck, M2 yüksek gerilim sınıfı buck + koruma ve M3 aktif OV kesme/sınırlama + TVS seçeneklerini karşılaştır.

3. Her seçeneği transient, SOA, termal, alan ve maliyet eksenlerinde kaynaklı değerlendir.

4. Sentetik transient senaryoları oluşturup girdilerin müşteri profili olmadığını açıkça işaretle.

5. G1’in kaynak doğrulaması ihtiyacına bağlanan karar kaydı üret.

## Bloklayıcılar ve sınırlar

OEM darbe profili, VIN min/max, jump-start ve ters polarite koşulları olmadan exact-part seçme veya PASS verme. Koruma elemanlarının değerlerini varsayımla nihai tasarıma aktarma. Simülasyon yapılmadıysa plan veya sentetik senaryo olarak adlandır.

## Pozisyon kabul ölçütü

Üç mimari seçenek aynı ölçütlerle kıyaslanmıştır; her sonuç kaynak/varsayım sınıfını taşır; eksik araç ya da profil BLOCKED/UNKNOWN olarak belirtilir; exact part kararı verilmez.

## Codex prompt zinciri

### Prompt 1 — Girdi uygunluğu

WF02 için kaynak ve gereksinimleri incele. Q-C01…03 (VIN min/max, OEM/ISO transient profili, jump-start, ters polarite) cevapları ve birincil belge revizyonları mevcut mu kontrol et. Mevcut bilgiyle karar verilebilen ve verilemeyenleri ayır. Salt okunur kal; önceki dosyaları değiştirme.

### Prompt 2 — Alternatiflerin parametrik karşılaştırması

M1 TVS+uygun buck, M2 yüksek gerilim sınıfı buck+koruma ve M3 aktif OV kesme/sınırlama+TVS seçenekleri için protection-comparison.csv ve power-entry-options.md taslaklarını üret. Transient dayanımı, SOA, termal, alan ve maliyet alanları olsun. Her satır SOURCE\_EXPLICIT / DERIVED / ENGINEERING\_RULE / UNKNOWN olarak işaretlensin; kanıt yoksa UNKNOWN yaz. OEM profili olmadan kesin parça numarası, PASS veya güvenlik iddiası verme.

### Prompt 3 — Sentetik senaryo ve karar kapısı

transient-scenarios.json içine yalnız test planı amacıyla parametrik/sentetik senaryo tanımla; bunları otomotiv standardı profili veya doğrulanmış saha girdisi gibi sunma. Her senaryo için gerekli dalga şekli, beklenen gözlem ve eksik müşteri girdisini belirt. Q-C01…03 tamamlanmadan hangi kararların BLOCKED kaldığını ve G1/G2’ye taşınacak kanıtları raporla.

# WF03 — Power Tree & Regulator Design

Amaç: 5 V ve 3,3 V güç ağacının yük, kayıp, derating ve sıralama bütçesini hesaplanabilir taslaklara dönüştürmek.

**Girdiler:** REQ-002; WF02 taslakları; Q-C04…06 (USB akımı, LCD yükleri, sıcaklık); doğrulanmış yük kaynakları.

**Beklenen teslimatlar:** rail-budget.csv; regulator-candidates.csv; sequencing.csv; thermal-budget.md; simulation-plan.md.

## Görev adımları

1. Her güç rayı için minimum/tipik/tepe yükleri ve kaynaklarını çıkar.

2. Inrush, verim, kayıp, derating, kapasitör ve zamanlama hesabının parametrelerini oluştur.

3. Regülatör adaylarını yalnız kanıt ve çalışma aralığı kıstasıyla listele; kesin seçimi ertele.

4. PGOOD/RESET davranışını ve supervisor seçeneklerini zamanlama kanıtından ayrı kaydet.

5. Kararlılık, transient ve startup senaryolarını ayrı doğrulama kalemleri yap.

6. Ngspice veya başka araç varsa sürüm ve model provenansını doğrula; yoksa NOT\_RUN de.

## Bloklayıcılar ve sınırlar

PKG-A içindeki CM5 yükünü JOB-01’e kopyalama. Yük akımları eksikse hesap sonucu UNKNOWN. Regülatör kararlılığını yalnız transient hesabından kanıtlamış sayma. Ngspice yoksa simülasyon çalıştırıldığını yazma.

## Pozisyon kabul ölçütü

Worst-case hesap izi ve varsayımlar görünürdür; yük girdileri kaynaklıdır veya UNKNOWN’dur; PGOOD/RESET, kararlılık ve transient kanıtları birbirine karıştırılmamıştır; exact-part kararı verilmemiştir.

## Codex prompt zinciri

### Prompt 1 — Ray yük modeli

WF03 girdilerinden 5 V ve 3,3 V yüklerini ayıkla. rail-budget.csv taslağında min/tipik/tepe akım, inrush varsayımı, görev durumu, kaynak ve kanıt sınıfı alanları bulunsun. USB-A VBUS, LCD/arka ışık, transceiver, level shifter ve ESP32-S3 yüklerini ayrı göster. Kaynak olmayan yükleri UNKNOWN bırak; PKG-A CM5 yükünü kullanma.

### Prompt 2 — Regülatör ve termal hesap planı

Yalnız doğrulanmış girişlerle regulator-candidates.csv ve thermal-budget.md taslağı üret. Verim, kayıp, derating, kondansatör ve çalışma sıcaklığı hesaplarının formüllerini ve gerekli parametrelerini göster. Eksik veri varsa sembolik/parametrik bırak; sayısal sonuç uydurma ve exact-part seçme. Q-C04…06 ve WF02 varsayımlarına bağlantı kur.

### Prompt 3 — Sıralama ve simülasyon planı

sequencing.csv ve simulation-plan.md taslaklarında VIN\_VALID → 5 V → 3,3 V → RESET\_N açılışını; brownout, timeout, PGOOD ve power-off durumlarını tanımla. Regülatör kararlılığı kanıtını transientten ayrı tut. Simülatör ve model mevcutsa sürüm/provenans şartlarını belirt; yoksa simülasyonu NOT\_RUN işaretle. Hangi kanıtın G2’ye, hangisinin G3’e gireceğini ayır.

# WF04 — MCU & Peripheral Integration

Amaç: ESP32-S3-WROOM-1-N8 pinleri, boot/strapping, USB host, seri kanallar ve güç alanları için çakışma ve durum matrisi kurmak.

**Girdiler:** REQ-003/004; CLAUDE-03; WF01 GPIO ve arayüz taslakları; ESP32-S3 TRM/datasheet birincil kaynakları.

**Beklenen teslimatlar:** pin-allocation.csv; usb-host-contract.md; serial-architecture.md; power-domains.md; boot-reset-checklist.md.

## Görev adımları

1. WROOM-1-N8 pin/strapping bilgilerini birincil dokümandan doğrula.

2. USB-A host VBUS, akım sınırlama/kapama ve ESD sözleşmesini müşteri onayına açık alanlarla yaz.

3. UART0 debug, TTL buffer, RS-232 transceiver, LCD, CAN DNP ve boot glitch risklerini çalışma durumlarına göre eşle.

4. Güç kapalı durumda USB/RS-232 geri besleme yollarını analiz et.

5. GPIO çakışmasını ve 10 TX / 3 UART kapasite sınırını CF-09 ile ilişkilendir.

## Bloklayıcılar ve sınırlar

CF-09 çözülmeden 10 TX topolojisi seçme. PHY/JTAG davranışını TRM ile doğrulamadan sabitleme. USB akım eşiği, baud/kablo/yük ve ESD hedefi müşteri onayı olmadan kesinleşmez.

## Pozisyon kabul ölçütü

Pin tahsisi kaynaklıdır veya açık blocker gösterir; boot, aktif, reset ve power-off durumları tanımlıdır; GPIO çakışmaları raporlanır; CF-09 belirsizliği saklanmaz.

## Codex prompt zinciri

### Prompt 1 — Pin ve boot kanıtı

ESP32-S3-WROOM-1-N8 için pin, strapping, USB OTG/USB Serial-JTAG ve UART davranışını birincil TRM/datasheet üzerinden doğrula. pin-allocation.csv ve boot-reset-checklist.md taslaklarını hazırla. Her pinin kaynağı ve bölüm/sayfa referansı olsun. Kaynak bulunamazsa UNKNOWN; GPIO ataması kesinleşmemişse taslak olarak açıkça belirt.

### Prompt 2 — USB ve seri mimari sözleşmesi

usb-host-contract.md ve serial-architecture.md taslaklarını üret. USB-A host VBUS güç anahtarı/akım limiti/ESD, UART0 debug, 5 adet 5 V TTL TX, 5 adet RS-232 TX ve 1 RX için yön, seviye, güç alanı, çalışma hali ve test alanlarını tanımla. CF-09 cevaplanmadıysa 10 TX topolojisini BLOCKED bırak; kanalların bağımsız veri akışı olup olmadığını varsayma.

### Prompt 3 — Güç alanı, geri besleme ve risk matrisi

power-domains.md içinde açılış, aktif, brownout, reset ve power-off senaryolarını çıkar. USB ve RS-232 üzerinden geri besleme, boot sırasında TX sahte start biti, strapping ve debug etkileşimlerini risk olarak kaydet. Kanıtlanan davranışla önerilen mühendislik kuralını ayır. Q-C07…10 ve kalan müşteri kararlarını güncelle; tasarım kilitleme kararı verme.

# WF05 — Schematic & Pre-layout Verification

Amaç: Yalnız G1 ve G2 kabulünden sonra onaylı gereksinimlerle şematik ve model provenanslı pre-layout kanıtlarını hazırlamak.

**Girdiler:** G1 PASS; G2 mimari kabulü; WF01–WF04 kabul edilmiş girdiler ve açık kararların kapatılmış/izin verilmiş listesi.

**Beklenen teslimatlar:** KiCad şematik; ERC raporu; BOM; simulation-evidence; prelayout-review.md; G3 kanıt paketi.

## Görev adımları

1. Başlangıçta G1/G2 ve WF01–WF04 onay durumunu kontrol et; herhangi biri geçmemişse STOP ve blocker raporu ver.

2. Kabul edilmiş mimariyi şematik bloklarına eşle ve değişiklik planını önce göster.

3. Onay varsa KiCad şematiği, sembol/footprint provenansı ve BOM’u üret.

4. ERC’yi mevcut KiCad sürümüyle çalıştır, raporu sakla; simülasyon modellerinin kaynak ve kapsamını kaydet.

5. Hot-loop, dönüş yolu, decoupling, anten keepout, termal ve EMC riskleri için pre-layout incelemesi yap.

6. G3 kanıtlarını ve bağımsız kritik bulgu kontrol listesini hazırla.

## Bloklayıcılar ve sınırlar

G1 BLOCKED iken şematik üretme; G2 kabulü olmadan mimariyi uygulama. ERC başarısı EMC veya loop stability kanıtı değildir. Araç ya da model yoksa NOT\_RUN. Kaynak tasarım dosyasını izinsiz üzerine yazma.

## Pozisyon kabul ölçütü

G1/G2 geçiş kanıtları mevcut; ERC ve simülasyon gerçek çalıştırma durumuyla raporlanmış; model kökeni izlenebilir; bağımsız kritik bulgular kapatılmış veya açık blocker’dır.

## Codex prompt zinciri

### Prompt 1 — Giriş kapısı denetimi

WF05 başlatmadan önce G1 PASS, G2 kabulü ve WF01–WF04 çıktılarının kabul durumunu denetle. Her kapının kanıt dosyasını ve sürümünü listele. Herhangi biri eksik/BLOCKED ise tasarım dosyası oluşturma veya düzenleme; yalnız blocker, gerekli kanıt ve güvenli sonraki adımı raporla.

### Prompt 2 — Onaylı şematik çalışma planı

Yalnız önceki kapılar PASS ise kabul edilmiş arayüz ve güç mimarisini KiCad şematik bloklarına eşle. Kaynak dosyaları değiştirmeden önce planı ve dokunulacak dosyaları listele. Gerekli sembol/footprint/model provenansını ve BOM kaynaklandırmasını göster. Kullanıcı onayı gerektiren değişiklikleri bekle; Git, kurulum, Drive veya overwrite işlemi yapma.

### Prompt 3 — ERC, simülasyon ve G3 kanıtı

Onaylanmış değişiklikler uygulandıktan sonra ERC’yi kurulu KiCad sürümüyle çalıştır; komut/sürüm, rapor ve açık ihlalleri kaydet. Simülasyonda model provenansını ve kapsamını belirt; araç/model yoksa NOT\_RUN. ERC’yi EMC veya loop stability kanıtı sayma. prelayout-review.md içinde hot-loop, dönüş yolu, decoupling, anten keepout, termal ve EMC risklerini ayır. G3 için eksik kanıtları ve kritik bulgu listesini raporla.

# WF06 — PCB & Manufacturing Package

Amaç: G3 PASS sonrasında 4 katmanlı PCB’yi ve üretim paketini yeniden üretilebilir kontrollerle hazırlamak.

**Girdiler:** G3 PASS; onaylı WF05 şematik ve bileşen kaynakları; üretici stackup/üretim sınıfı ve anten kısıtları.

**Beklenen teslimatlar:** KiCad PCB; DRC; Gerber; NC drill; CPL; BOM; STEP; release checksums; DFM ve maliyet değerlendirmesi.

## Görev adımları

1. G3 PASS ve girdilerin değişmediğini kontrol et; değilse STOP. Üretici stackup ve 4 katman kısıtlarını al; ≤64×65 mm mekanik sınırı ve anten keepout’u bağla.

2. PCB yerleşim/routing kararlarını hot-loop, dönüş yolu, RF/anten, termal ve konektör erişimi açısından kaydet.

3. KiCad DRC çalıştır ve tüm hataları sınıflandır.

4. Gerber, drill, CPL, BOM ve STEP dosyalarını aynı release sürümünden üret.

5. Dosya bütünlüğü için checksum ve yeniden üretim talimatlarını ekle.

6. 100 adet PCBA için 10–15 USD hedefini güncel kaynaklarla karşılaştır; fizibilite onayı olarak sunma.

## Bloklayıcılar ve sınırlar

G3 olmadan layout yok. Üretici stackup, assembly class veya maliyet teklifi yoksa ilgili sonuç UNKNOWN. Açıklanmamış DRC hatasıyla üretim release’i verme. Fiziksel test veya otomotiv sertifikası iddia etme.

## Pozisyon kabul ölçütü

G3 kanıtı PASS ve girdi sürümü sabittir; DRC’de açıklanmamış hata yok; üretim çıktıları aynı revizyondan ve checksum’lıdır; maliyet hedefi güncel kaynaklarla değerlendirilmiş veya UNKNOWN’dur.

## Codex prompt zinciri

### Prompt 1 — G3 ve release girdisi kapısı

WF06’ya başlamadan G3 PASS kanıtını, WF05 şematik revizyonunu, üretici stackup/anten keepout ve montaj sınıfı girdilerini doğrula. Her dosya/revizyon/hash için manifest hazırla. G3 PASS değilse veya girdiler değişmişse layout ve üretim dosyası üretme; yalnız blocker raporla.

### Prompt 2 — Layout ve DRC planı

Kapılar PASS ise 4 katmanlı, ≤64×65 mm PCB için mevcut onaylı şematiğe bağlı yerleşim/routing planı oluştur. Hot-loop, dönüş yolu, decoupling, RF/anten keepout, termal ve konektör erişim kararlarını kaydet. KiCad PCB üzerinde değişiklik yapmadan önce dokunulacak dosyaları bildir. DRC’yi çalıştırınca sürüm, ayarlar, rapor ve istisnaları sakla; açıklanmamış hataları release blocker yap.

### Prompt 3 — İmalat paketi ve maliyet kontrolü

Onaylı PCB revizyonundan Gerber, NC drill, CPL, BOM ve STEP çıktıları üret; hepsini manifest ve SHA-256 checksum ile eşleştir. DFM kontrollerini ve yeniden üretim adımlarını yaz. 100 adet PCBA 10–15 USD hedefini güncel tedarikçi/üretici girdileriyle kıyasla; fiyat verisi yoksa UNKNOWN yaz ve hedefi fizibilite onayı gibi sunma. Fiziksel test veya otomotiv sertifikası iddiası ekleme.

# Codex yürütme sırası

| **Sıra** | **Görev / durum** | **Çıkış koşulu** |
| --- | --- | --- |
| 0 | Salt okunur repo ve kaynak envanteri | Dosyalar, sürümler ve G0/G1 kanıt boşlukları kayıtlı. |
| 1 | WF01 gereksinim ve arayüz taslakları | İzlenebilirlik ve açık kararlar gözden geçirilebilir. |
| 2 | WF02–WF04 kanıtlı taslaklar | Parametrik araştırma yapılır; mimari kilitlenmez. |
| 3 | G1 ve G2 kabul değerlendirmesi | Kaynak/provenans ve mimari kararlar açıkça PASS olmadan devam edilmez. |
| 4 | WF05 şematik ve pre-layout | Yalnız G1 PASS + G2 kabulünden sonra. |
| 5 | G3 kabul değerlendirmesi | Şematik/ERC/model/pre-layout kanıt paketi incelenir. |
| 6 | WF06 PCB ve üretim paketi | Yalnız G3 PASS ve sabit girdilerle. |

# Kapanış raporu şablonu

Her Codex görevi sonunda aşağıdaki özeti iste:

- Görev ve iş paketi: WFxx / adım numarası

- Sonuç: PASS, BLOCKED, UNKNOWN veya NOT\_RUN

- Değiştirilen/oluşturulan dosyalar ve sürümleri

- Çalıştırılan araçlar, sürüm ve kanıt konumları

- Açık müşteri kararları ve teknik belirsizlikler

- Kapı etkisi: G1/G2/G3/G4/G6 durumu

- Bir sonraki güvenli görev; başlaması için gereken koşullar

# Kaynakla uyum notu

Plan, REP-01 İş Akışı Raporu v1.0’daki altı WF, teslimat adları, açık sorular ve kapı koşullarını korur. Bu görev planı müşteri onayı, kaynak doğrulaması, kesin bileşen seçimi, devre/PCB tasarımı veya fiziksel doğrulama yerine geçmez. Codex promptları taslak üretimi ve kanıt yönetimi içindir; kapı koşulları aşılmaz.
