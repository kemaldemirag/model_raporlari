# SPRINT-08 | Stepper Komut Dizisi ve Zamanlama

_03.11.2026  •  Araştırma ve uygulama hazırlığı  •  Salt okunur değerlendirme_

## 1. Hedef ve kapsam

Repository: https://github.com/altste/altste.github.io<br>
Hedef: Bu repoda kanıtlanabilir test boşlukları, gerçek API sözleşmeleri ve akademik doğrulama senaryoları belirlemek. Bu belge yürütülmüş test veya onaylanmış upstream katkı raporu değildir.

## 2. Dosya yolu odaklı inceleme

- assets/ino/Lab04Arduino.ino

## 3. Geliştirme iş paketleri

1. S8-01 — millis() tabanlı yön değişimini bloklayan step() çağrısından ayır.

1. S8-02 — İlk yönün yorumla tutarlılığını incele.

1. S8-03 — 2048 adım ve 6 RPM nominal 10 saniye hareketi hesapla.

1. S8-04 — Komut gecikmesi ve iptal edilebilirlik için zamanlı FSM modeli tasarla.

## 4. Model–Test eşleştirmesi

| Test ekseni | Oracle / yöntem | Kanıt |
| --- | --- | --- |
| T-01 | millis() tabanlı yön değişimini bloklayan step() çağrısından ayır. | Commit SHA + komut + log + beklenen/gerçekleşen |
| T-02 | İlk yönün yorumla tutarlılığını incele. | Commit SHA + komut + log + beklenen/gerçekleşen |
| T-03 | 2048 adım ve 6 RPM nominal 10 saniye hareketi hesapla. | Commit SHA + komut + log + beklenen/gerçekleşen |

## 5. Kabul kriterleri

- Yön değiştirme olay izi

- Bloklama süresi ve deadline matrisi

- Kod–açıklama tutarlılık düzeltmesi

Ortak kalite kapısı: mevcut baseline kayıt altına alınır; yeni testler önce hatayı veya kapsam boşluğunu kanıtlar; düzeltme sonrası aynı komutla tekrar çalıştırılır. Derleme/test yapılmadıysa sonuç NOT RUN olarak işaretlenir.

## 6. Akademik ve teknik referanslar

- Liu & Layland (1973) — Scheduling Algorithms for Multiprogramming

- Arduino Stepper referansı

- Hespanha et al. (2007) — Networked Control Systems

## 7. Risk, sınırlar ve karar kapısı

Kod adlarının ve işlevlerin bazıları önceki kaynak incelemelerinden alınmıştır; güncel commit, lisans, test altyapısı ve upstream katkı koşulları uygulama öncesi yeniden doğrulanmalıdır. Mock/host-test fiziksel güvenlik kanıtı değildir. Başka repo kodu lisans incelemesi olmadan kopyalanmamalıdır.

İzin sınırı: mevcut 02.11.2026 çalışma alanında yalnız IRremoteESP8266 doğrulanmıştır. Bu repo aday olarak kalır; yeni GitHub repo, fork, push veya PR onaysız yapılmaz.

## 8. Codex devir kontrol listesi

☐ Varsayılan dal ve tam commit SHA sabitlendi mi?

☐ Lisans ve test komutları doğrulandı mı?

☐ Mevcut testlerle tekrarlar ayıklandı mı?

☐ Red/Green kanıtı ve test günlükleri kaydedildi mi?

☐ Kod değişikliği ve upstream katkı için ayrıca izin alındı mı?
