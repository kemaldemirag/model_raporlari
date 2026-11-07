# SPRINT-12 | FOC, Örnekleme ve Kontrol Zinciri

_03.11.2026  •  Araştırma ve uygulama hazırlığı  •  Salt okunur değerlendirme_

## 1. Hedef ve kapsam

Repository: https://github.com/simplefoc/Arduino-FOC<br>
Hedef: Bu repoda kanıtlanabilir test boşlukları, gerçek API sözleşmeleri ve akademik doğrulama senaryoları belirlemek. Bu belge yürütülmüş test veya onaylanmış upstream katkı raporu değildir.

## 2. Dosya yolu odaklı inceleme

- src/

- examples/

- docs/

Not: src/, examples/, docs/ başlangıç arama kökleridir; tekil uygulama dosyaları henüz doğrulanmadı.

## 3. Geliştirme iş paketleri

1. S12-01 — Güncel kaynak ağacında gerçek FOC, current-sense ve PWM dosyalarını saptayıp yol sabitle.

1. S12-02 — d/q akım hedeflerini, PI kontrolörlerini ve gerilim komutlarını ayrı sözleşmelerle modelle.

1. S12-03 — loopFOC() ve move() zamanlaması ile sensör okuma bağımlılığını araştır.

1. S12-04 — Gerilim sınırlaması, anti-windup, sensör yokluğu ve kalibrasyon için test yükümlülükleri çıkar.

## 4. Model–Test eşleştirmesi

| Test ekseni | Oracle / yöntem | Kanıt |
| --- | --- | --- |
| T-01 | Güncel kaynak ağacında gerçek FOC, current-sense ve PWM dosyalarını saptayıp yol sabitle. | Commit SHA + komut + log + beklenen/gerçekleşen |
| T-02 | d/q akım hedeflerini, PI kontrolörlerini ve gerilim komutlarını ayrı sözleşmelerle modelle. | Commit SHA + komut + log + beklenen/gerçekleşen |
| T-03 | loopFOC() ve move() zamanlaması ile sensör okuma bağımlılığını araştır. | Commit SHA + komut + log + beklenen/gerçekleşen |

## 5. Kabul kriterleri

- Sürüm ve dosya yolu kanıt matrisi

- FOC dönüşüm matematiği için referans testler

- Gecikme ve saturasyon senaryo seti

Ortak kalite kapısı: mevcut baseline kayıt altına alınır; yeni testler önce hatayı veya kapsam boşluğunu kanıtlar; düzeltme sonrası aynı komutla tekrar çalıştırılır. Derleme/test yapılmadıysa sonuç NOT RUN olarak işaretlenir.

## 6. Akademik ve teknik referanslar

- Skuric et al. (2022) — SimpleFOC, JOSS DOI 10.21105/joss.04232

- SimpleFOC resmî dokümantasyonu

- Imperix — discrete control delay

## 7. Risk, sınırlar ve karar kapısı

Kod adlarının ve işlevlerin bazıları önceki kaynak incelemelerinden alınmıştır; güncel commit, lisans, test altyapısı ve upstream katkı koşulları uygulama öncesi yeniden doğrulanmalıdır. Mock/host-test fiziksel güvenlik kanıtı değildir. Başka repo kodu lisans incelemesi olmadan kopyalanmamalıdır.

İzin sınırı: mevcut 02.11.2026 çalışma alanında yalnız IRremoteESP8266 doğrulanmıştır. Bu repo aday olarak kalır; yeni GitHub repo, fork, push veya PR onaysız yapılmaz.

## 8. Codex devir kontrol listesi

☐ Varsayılan dal ve tam commit SHA sabitlendi mi?

☐ Lisans ve test komutları doğrulandı mı?

☐ Mevcut testlerle tekrarlar ayıklandı mı?

☐ Red/Green kanıtı ve test günlükleri kaydedildi mi?

☐ Kod değişikliği ve upstream katkı için ayrıca izin alındı mı?
