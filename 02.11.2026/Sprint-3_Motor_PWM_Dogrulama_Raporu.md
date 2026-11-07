# SPRINT-3 | Motor Sürücüleri ve PWM Doğrulaması

_03.11.2026  •  Katkı Yaptığım Repolar  •  Araştırma ve uygulama planı_

Hedef: tekdemo/Motor  |  https://github.com/tekdemo/Motor

## 1. Kapsam ve sınırlar

Amaç: Arduino motor sürücü soyutlamasının enable/disable, brake/coast, PWM ve yön durumlarını model tabanlı ve sözleşme tabanlı testlerle doğrulamak. GST ve IRremoteESP8266 metodolojik referanstır; bu sprintte onların kodu değiştirilmeyecek. Fiziksel motor güvenliği host-test ile kanıtlanamaz.

## 2. Kaynak koddan görülen bulgular

- DualPWM::disable(): _enabled=false atamasından sonra coast() çağrılır; coast(int) ilk satırda !_enabled ise döner. Bu nedenle bu çağrı PWM çıkışlarını değiştirmez.

- DualPWM, DualPWMEnable ve FourWire constructor’larında _mirrored, _enableCoastMode ve _val üyelerinin açık başlangıç değerleri yoktur. İlk okumalar için C++ belirsiz davranış riski vardır.

- FourWire::disable() önce enable pinini LOW yapar, ardından write(0) çağırır, sonra _enabled=false atar. Bunun fiziksel güvenlik semantiği sürücü veri sayfasına bağlıdır.

- VNH5019(int a,int c,int en,int pwm) → FourWire(a,c,en,pwm) aktarımı, FourWire(int a,int c,int pwm,int en) imzasına göre konumsal olarak tutarlıdır. Sırf bu çağrıdan hata çıkarılmamalıdır.

## 3. Sprint iş paketleri

| ID | Görev | Kabul kanıtı | Öncelik |
| --- | --- | --- | --- |
| S3-01 | Baseline ve lisans | README, LICENSE, örnekler, derleme komutları; commit SHA ve toolchain kaydı | P0 |
| S3-02 | Başlangıç durumu | Üyeleri deterministik başlatma öncesi/sonrası testleri; statik analiz | P0 |
| S3-03 | Disable regresyonu | enable → write(100) → disable; GPIO/PWM çağrı izi | P0 |
| S3-04 | PWM ve frenleme | -256,-255,-1,0,1,255,256; mirror ve brake/coast kombinasyonları | P0 |
| S3-05 | Sürücü sözleşmeleri | DualPWM/FourWire/VNH5019 pin haritası ve veri sayfası eşlemesi | P1 |
| S3-06 | Host test | Arduino.h mock: pinMode, digitalWrite, analogWrite, constrain | P1 |
| S3-07 | Kanıt paketi | Red/green logları, mutasyonlar, sınırlamalar, PR taslağı | P1 |

## 4. Test sözleşmesi ve deneyler

Örnek soyut durumlar: Disabled, EnabledBrake, EnabledCoast, Forward, Reverse. Bunlar fiziksel durumlar değil, test modeli durumlarıdır. Girdi: komut ve PWM değeri; gözlem: Arduino mock çağrıları, isEnabled(), read(). Her sürücü sınıfı için farklı pin doğruluk tablosu gerekebilir.

- TC-01: Yeni nesne + write(100) → başlangıç modu ve pin çağrıları deterministik olmalı.

- TC-02: DualPWM enable → write(100) → disable → izin verilen çıkış durumu açık sözleşmeyle karşılaştırılmalı.

- TC-03: mirror(true) ve mirror(false) için pozitif/negatif komut simetrisi sınanmalı; donanım topolojisine özgü istisnalar kaydedilmeli.

- TC-04: Sınır dışı komutlar constrain ile [-255,255] aralığına alınmalı.

- TC-05: FourWire enable/disable geçişinde çıkış çağrı sırası ve pin ataması incelenmeli.

## 5. Akademik ve üretici kaynakları

- Benveniste et al. (2018), Contracts for System Design — Varsayım–garanti sözleşmesi<br>
https://doi.org/10.1145/3182014

- Tretmans (2008), Model Based Testing with Labelled Transition Systems — Durum ve olay geçişleri<br>
https://doi.org/10.1007/978-3-540-78917-8_1

- Segura et al. (2016), A Survey on Metamorphic Testing — Dönüşüm ilişkileri<br>
https://doi.org/10.1109/TSE.2016.2532875

- Jia & Harman (2011), Mutation Testing Survey — Testlerin hata yakalama gücü<br>
https://doi.org/10.1109/TSE.2010.62

- TI DRV8872 datasheet — Sürücüye özgü brake/coast tablosu; VNH5019’a doğrudan uygulanmaz<br>
https://www.ti.com/lit/ds/symlink/drv8872.pdf

- ST VNH5019A-E datasheet — Pin ve hata durumları için donanım referansı<br>
https://www.st.com/resource/en/datasheet/vnh5019a-e.pdf

## 6. İş akışı ve çıkış ölçütleri

Salt-okunur baseline → API/lisans incelemesi → mock testleri → red test kanıtı → en küçük yerel düzeltme → green regresyon → sürücü veri sayfası kontrolü → kanıt paketi → ayrı onayla upstream PR. Başarılı test, yalnızca gerçek komut ve log ile raporlanır.

Kanıt dosyası şeması: repo, commit_sha, test_id, toolchain, command, expected, observed, status, limitations, artifact_path. Host-test yalnızca yazılımın pin çağrılarını kanıtlar; gerçek motor akımı, frenleme veya güvenli duruşu kanıtlamaz.

## 7. Karar kapıları

GO: Gerçek derleme/test ortamı kuruldu, ilk kusur yeniden üretildi, testler güvenilir ve lisans uygun. HOLD: API veya veri sayfası belirsiz. NO-GO: Fiziksel güvenlik iddiası yalnız mock sonuçlarına dayanıyor. Repo ekleme, fork, push ve PR işlemleri için ayrıca onay alınır.

## Belge durumu

> Sprint-3 | Araştırma taslağı — test yürütülmedi, dosya veya upstream değiştirilmedi.
