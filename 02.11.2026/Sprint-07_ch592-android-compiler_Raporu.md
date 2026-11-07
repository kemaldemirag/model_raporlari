# SPRINT-07 | Servo API ve PWM Sözleşmeleri

_03.11.2026  •  Araştırma ve uygulama hazırlığı  •  Salt okunur değerlendirme_

## 1. Hedef ve kapsam

Repository: https://github.com/gouthamsk98/ch592-android-compiler<br>
Hedef: Bu repoda kanıtlanabilir test boşlukları, gerçek API sözleşmeleri ve akademik doğrulama senaryoları belirlemek. Bu belge yürütülmüş test veya onaylanmış upstream katkı raporu değildir.

## 2. Dosya yolu odaklı inceleme

- app/app/src/main/assets/project/main/inc/Servo.h

## 3. Geliştirme iş paketleri

1. S7-01 — attach/detach açıklamaları ile void fonksiyon imzalarını karşılaştır.

1. S7-02 — İlgili Servo uygulama dosyasını ve XMC/PWM bağımlılıklarını bul.

1. S7-03 — write ve writeMicroseconds sınırlarını ve açı/pulse dönüşümünü incele.

1. S7-04 — attached() ve read() çıktılarının gerçek fiziksel geri besleme olmadığını açıkça belgele.

## 4. Model–Test eşleştirmesi

| Test ekseni | Oracle / yöntem | Kanıt |
| --- | --- | --- |
| T-01 | attach/detach açıklamaları ile void fonksiyon imzalarını karşılaştır. | Commit SHA + komut + log + beklenen/gerçekleşen |
| T-02 | İlgili Servo uygulama dosyasını ve XMC/PWM bağımlılıklarını bul. | Commit SHA + komut + log + beklenen/gerçekleşen |
| T-03 | write ve writeMicroseconds sınırlarını ve açı/pulse dönüşümünü incele. | Commit SHA + komut + log + beklenen/gerçekleşen |

## 5. Kabul kriterleri

- Başlık–uygulama uyumluluk tablosu

- PWM kanal tahsisi ve hata davranışı testleri

- API dokümantasyon düzeltme önerisi

Ortak kalite kapısı: mevcut baseline kayıt altına alınır; yeni testler önce hatayı veya kapsam boşluğunu kanıtlar; düzeltme sonrası aynı komutla tekrar çalıştırılır. Derleme/test yapılmadıysa sonuç NOT RUN olarak işaretlenir.

## 6. Akademik ve teknik referanslar

- Design by Contract — Meyer

- NASA MC/DC tutorial

- Arduino Servo resmî API belgeleri

## 7. Risk, sınırlar ve karar kapısı

Kod adlarının ve işlevlerin bazıları önceki kaynak incelemelerinden alınmıştır; güncel commit, lisans, test altyapısı ve upstream katkı koşulları uygulama öncesi yeniden doğrulanmalıdır. Mock/host-test fiziksel güvenlik kanıtı değildir. Başka repo kodu lisans incelemesi olmadan kopyalanmamalıdır.

İzin sınırı: mevcut 02.11.2026 çalışma alanında yalnız IRremoteESP8266 doğrulanmıştır. Bu repo aday olarak kalır; yeni GitHub repo, fork, push veya PR onaysız yapılmaz.

## 8. Codex devir kontrol listesi

☐ Varsayılan dal ve tam commit SHA sabitlendi mi?

☐ Lisans ve test komutları doğrulandı mı?

☐ Mevcut testlerle tekrarlar ayıklandı mı?

☐ Red/Green kanıtı ve test günlükleri kaydedildi mi?

☐ Kod değişikliği ve upstream katkı için ayrıca izin alındı mı?
