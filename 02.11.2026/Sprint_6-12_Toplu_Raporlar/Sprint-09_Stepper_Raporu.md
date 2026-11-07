# SPRINT-09 | Stepper Kütüphanesi Yürütme Doğrulaması

_03.11.2026  •  Araştırma ve uygulama hazırlığı  •  Salt okunur değerlendirme_

## 1. Hedef ve kapsam

Repository: https://github.com/arduino-libraries/Stepper<br>
Hedef: Bu repoda kanıtlanabilir test boşlukları, gerçek API sözleşmeleri ve akademik doğrulama senaryoları belirlemek. Bu belge yürütülmüş test veya onaylanmış upstream katkı raporu değildir.

## 2. Dosya yolu odaklı inceleme

- src/Stepper.cpp

- src/Stepper.h

## 3. Geliştirme iş paketleri

1. S9-01 — setSpeed(0) ve uygunsuz adım parametrelerini analiz et.

1. S9-02 — step() içindeki while ve yield() ile bloklayan yürütmeyi belge.

1. S9-03 — micros() taşması, step_delay ve yön çevrimini incele.

1. S9-04 — Mock micros()/stepMotor() ile tekrarlanabilir host-test fizibilitesini değerlendir.

## 4. Model–Test eşleştirmesi

| Test ekseni | Oracle / yöntem | Kanıt |
| --- | --- | --- |
| T-01 | setSpeed(0) ve uygunsuz adım parametrelerini analiz et. | Commit SHA + komut + log + beklenen/gerçekleşen |
| T-02 | step() içindeki while ve yield() ile bloklayan yürütmeyi belge. | Commit SHA + komut + log + beklenen/gerçekleşen |
| T-03 | micros() taşması, step_delay ve yön çevrimini incele. | Commit SHA + komut + log + beklenen/gerçekleşen |

## 5. Kabul kriterleri

- Zamanlama sınır testleri

- İleri/geri adım dizisi kayıtları

- API uyumluluğu ve regresyon kanıtı

Ortak kalite kapısı: mevcut baseline kayıt altına alınır; yeni testler önce hatayı veya kapsam boşluğunu kanıtlar; düzeltme sonrası aynı komutla tekrar çalıştırılır. Derleme/test yapılmadıysa sonuç NOT RUN olarak işaretlenir.

## 6. Akademik ve teknik referanslar

- Real-time scheduling — Liu & Layland

- Arduino Stepper resmî kaynak kodu

- Property-based testing — Claessen & Hughes (2000)

## 7. Risk, sınırlar ve karar kapısı

Kod adlarının ve işlevlerin bazıları önceki kaynak incelemelerinden alınmıştır; güncel commit, lisans, test altyapısı ve upstream katkı koşulları uygulama öncesi yeniden doğrulanmalıdır. Mock/host-test fiziksel güvenlik kanıtı değildir. Başka repo kodu lisans incelemesi olmadan kopyalanmamalıdır.

İzin sınırı: mevcut 02.11.2026 çalışma alanında yalnız IRremoteESP8266 doğrulanmıştır. Bu repo aday olarak kalır; yeni GitHub repo, fork, push veya PR onaysız yapılmaz.

## 8. Codex devir kontrol listesi

☐ Varsayılan dal ve tam commit SHA sabitlendi mi?

☐ Lisans ve test komutları doğrulandı mı?

☐ Mevcut testlerle tekrarlar ayıklandı mı?

☐ Red/Green kanıtı ve test günlükleri kaydedildi mi?

☐ Kod değişikliği ve upstream katkı için ayrıca izin alındı mı?
