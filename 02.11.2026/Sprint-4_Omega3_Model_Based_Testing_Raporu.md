# SPRINT-4 | OMEGA3

Model-Based Testing · Disket sürücüsü emülasyonu · 03.11.2026

Hedef: h5n1xp/Omega3  |  Referans commit: c0e4fa18e53176f7a2af860d22d87d6746b8a563

Durum: Araştırma ve geliştirme planı. Test çalıştırılmadı; repo klonlanmadı, değiştirilmedi veya upstream PR gönderilmedi.

## 1. Amaç, sınırlar ve kanıt

ADF disk görüntüsü yaşam döngüsü, MFM veri kodlaması ve chipset register yansımaları için durum–olay–çıktı sözleşmeleri kurulacak. Omega3, 02.11.2026 çalışma alanında mevcut çıktı reposu değildir; çalışma alanına eklenmesi ayrıca onaylanmalıdır.

| Kanıt düzeyi | Açıklama |
| --- | --- |
| E0 — Kod okuması | Drives.hpp, Drives.cpp, Chipset.cpp ve main.cpp üzerinden çıkarım |
| E1 — Host test | Derlenip çalıştırılacak izole test; henüz yok |
| E2 — Entegrasyon | Emülatör ve chipset kayıtlarıyla senaryo testi; henüz yok |
| E3 — Donanım | Bu sprintin kapsamında değil |

## 2. Dosya yolu envanteri

| Dosya | İnceleme / olası değişiklik |
| --- | --- |
| Drives.hpp | Floppy/Drives durum alanları; sınır ve gözlem API’leri |
| Drives.cpp | InsertDisk, EjectDisk, EncodeMFMTrack, MFMChecksum; yaşam döngüsü |
| Chipset.cpp | getSelectedDisk, DiskPresent ve register bitleri |
| main.cpp | ADF başlangıç yüklemesi ve sürücü bağlantıları |
| Mevcut test/CI yolları | Önce ağaç taramasıyla doğrulanmalı; varsayılmamalı |

## 3. Somut kaynak kod bulguları

InsertDisk(path), ilk olarak EjectDisk() çağırıyor. Dosya açılamaz, boyut uyuşmaz veya kısa okuma gerçekleşirse önceki disk çıkarılmış kalıyor. Bu bir “atomic replace” garantisi olmadığını gösterir; hata sayılması ancak ürün sözleşmesine göre mümkündür.

ADF_SIZE = 901120 bayt (80 track × 2 side × 11 sector × 512 byte). EncodeMFMTrack, header, label, checksum ve sektör verilerini MFM kelimelerine dönüştürüyor. MFM checksum hesabı XOR ve 0x55555555 maskesi kullanıyor. Kodun referans Amiga formatıyla birebir uygunluğu bağımsız oracle ile doğrulanmalı.

Chipset.cpp, getSelectedDisk() ve DiskPresent() üzerinden diskin varlığını register bitine yansıtıyor. Dolayısıyla hem Floppy iç durumu hem dışarıdan gözlemlenen register birlikte test edilmeli.

## 4. İş paketleri ve kabul kriterleri

| ID | Senaryo / geliştirme | Kabul kanıtı |
| --- | --- | --- |
| S4-01 | Build, lisans, test/CI ve mevcut fixture envanteri | Commit + build komutu + log; lisans doğrulaması |
| S4-02 | Insert → Eject → Insert; geçiş modeli | Her olayda disk varlığı/change ve tampon durumu |
| S4-03 | Geçersiz yol, yanlış boyut, kısa okuma | Hata politikası belgeli; bellek/sızıntı kontrolü |
| S4-04 | MFM header/data checksum ve bilinen vektörler | Bağımsız oracle ile byte/word karşılaştırması |
| S4-05 | Track 0/79, side 0/1, step sınırları | Sınır dışı erişim yok; tanımlı sonuç |
| S4-06 | Disk varlığı → chipset register bitleri | İç durum ile dış gözlem tutarlı |
| S4-07 | Regresyon, negatif test ve kanıt paketleme | Red/green sonuçları ve yeniden üretim adımları |

## 5. Model-Based Testing sözleşmesi

Soyut durum: S = {EMPTY, LOADED, INVALID_ATTEMPT}; olaylar: {INSERT_VALID, INSERT_INVALID, EJECT, STEP, SELECT}. Bu durum kümesi bir araştırma modelidir; Omega3’ün gerçek enum tanımı değildir.

| Başlangıç | Olay | Beklenen gözlem / araştırma sorusu |
| --- | --- | --- |
| EMPTY | INSERT_VALID | DiskPresent true; görüntü boyutu geçerli |
| LOADED | EJECT | DiskPresent false; kaynak serbest |
| LOADED | INSERT_INVALID | Önceki disk korunuyor mu? Kod EjectDisk ile önce çıkarıyor |
| EMPTY | STEP | Track sınırları ve disk yokluğu semantiği |
| LOADED | SELECT + register read | Chipset diskin varlığını doğru yansıtıyor mu? |

## 6. Akademik yöntemler

| Yöntem | Kaynak | Uygulama |
| --- | --- | --- |
| Labelled Transition Systems / ioco | Tretmans (2008), DOI:10.1007/978-3-540-78917-8_1 | Olay–gözlem geçişleri; tam ioco kanıtı iddia edilmez |
| Contract-Based Design | Benveniste et al. (2018), DOI:10.1145/3182014 | Geçerli ADF ve bellek varsayımları / register garantileri |
| Metamorphic Testing | Segura et al. (2016), DOI:10.1109/TSE.2016.2532875 | Tekrarlı Eject ve yeniden yükleme ilişkileri |
| Mutation Testing | Jia & Harman (2011), DOI:10.1109/TSE.2010.62 | Sınır, checksum ve durum mutasyonları |
| MC/DC | NASA/TM-2001-210876 | Dosya/geçerlilik kararları için koşul kapsamı |

## 7. Red/Green uygulama sırası

RED: Önce testlerin gerçekten eksik davranışı yakaladığını kanıtla. GREEN: En küçük düzeltmeyi uygula. REFACTOR: Davranışı koruyarak sadeleştir. Hiçbir test geçmediği halde PASS raporlanmaz. Önce mevcut testlerle çakışma ve lisans durumu doğrulanır.

Kanıt kayıtları: repo URL, commit SHA, dosya yolu, test adı, senaryo kimliği, test komutu, ortam, beklenen/gerçek sonuç, log yolu, oracle kaynağı, belirsizlik, lisans durumu.

## 8. Öncelikli kararlar ve riskler

P0: ADF yaşam döngüsü ve negatif yükleme; MFM bilinen vektörleri. P1: Chipset register entegrasyonu ve track sınırları. Riskler: Amiga MFM spesifikasyonunun yanlış yorumlanması, yanlış oracle, emülatör bağımlılıkları, test ortamı kurulumu ve kapsam dışı refactoring.

Çıkış kapısı: 1) lisans ve baseline doğrulandı; 2) test boşluğu kanıtlandı; 3) yeni testler çalıştı; 4) davranış değişikliği belgeli; 5) upstream PR yalnız ayrı kullanıcı onayıyla.

## 9. Doğrudan kaynaklar

https://github.com/h5n1xp/Omega3/blob/c0e4fa18e53176f7a2af860d22d87d6746b8a563/Drives.hpp

https://github.com/h5n1xp/Omega3/blob/c0e4fa18e53176f7a2af860d22d87d6746b8a563/Drives.cpp

https://github.com/h5n1xp/Omega3/blob/c0e4fa18e53176f7a2af860d22d87d6746b8a563/Chipset.cpp

https://github.com/h5n1xp/Omega3/blob/c0e4fa18e53176f7a2af860d22d87d6746b8a563/main.cpp

https://doi.org/10.1007/978-3-540-78917-8_1

https://doi.org/10.1145/3182014

https://doi.org/10.1109/TSE.2016.2532875

https://ntrs.nasa.gov/citations/20010057789

## Belge durumu

> SPRINT-4 · Araştırma taslağı · Değişiklik yapılmadı
