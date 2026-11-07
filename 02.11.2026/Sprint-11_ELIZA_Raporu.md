# SPRINT-11 | Olay Tabanlı Timeout ve Runtime Monitoring

_03.11.2026  •  Araştırma ve uygulama hazırlığı  •  Salt okunur değerlendirme_

## 1. Hedef ve kapsam

Repository: https://github.com/critical-code-studies/ELIZA<br>
Hedef: Bu repoda kanıtlanabilir test boşlukları, gerçek API sözleşmeleri ve akademik doğrulama senaryoları belirlemek. Bu belge yürütülmüş test veya onaylanmış upstream katkı raporu değildir.

## 2. Dosya yolu odaklı inceleme

- assets/term.js

## 3. Geliştirme iş paketleri

1. S11-01 — motor() ses işlevinin gerçek fiziksel motor sürücüsü olmadığını kaydet.

1. S11-02 — 60 saniyelik idle timeout, yeniden tetikleme ve visibilitychange olaylarını modelle.

1. S11-03 — Sound.hum(false) ve Sound.quiet() çağrılarının olay sırasını incele.

1. S11-04 — Sahte saat ile deterministik UI zamanlayıcı testleri tasarla.

## 4. Model–Test eşleştirmesi

| Test ekseni | Oracle / yöntem | Kanıt |
| --- | --- | --- |
| T-01 | motor() ses işlevinin gerçek fiziksel motor sürücüsü olmadığını kaydet. | Commit SHA + komut + log + beklenen/gerçekleşen |
| T-02 | 60 saniyelik idle timeout, yeniden tetikleme ve visibilitychange olaylarını modelle. | Commit SHA + komut + log + beklenen/gerçekleşen |
| T-03 | Sound.hum(false) ve Sound.quiet() çağrılarının olay sırasını incele. | Commit SHA + komut + log + beklenen/gerçekleşen |

## 5. Kabul kriterleri

- Timeout FSM geçiş matrisi

- Fake timer test planı

- Sekme görünürlüğü ve yeniden etkinleştirme olay izleri

Ortak kalite kapısı: mevcut baseline kayıt altına alınır; yeni testler önce hatayı veya kapsam boşluğunu kanıtlar; düzeltme sonrası aynı komutla tekrar çalıştırılır. Derleme/test yapılmadıysa sonuç NOT RUN olarak işaretlenir.

## 6. Akademik ve teknik referanslar

- Leucker & Schallhart (2009) — Runtime Verification

- Bauer et al. (2011) — Three-valued Runtime Verification

- Finite state machine testing

## 7. Risk, sınırlar ve karar kapısı

Kod adlarının ve işlevlerin bazıları önceki kaynak incelemelerinden alınmıştır; güncel commit, lisans, test altyapısı ve upstream katkı koşulları uygulama öncesi yeniden doğrulanmalıdır. Mock/host-test fiziksel güvenlik kanıtı değildir. Başka repo kodu lisans incelemesi olmadan kopyalanmamalıdır.

İzin sınırı: mevcut 02.11.2026 çalışma alanında yalnız IRremoteESP8266 doğrulanmıştır. Bu repo aday olarak kalır; yeni GitHub repo, fork, push veya PR onaysız yapılmaz.

## 8. Codex devir kontrol listesi

☐ Varsayılan dal ve tam commit SHA sabitlendi mi?

☐ Lisans ve test komutları doğrulandı mı?

☐ Mevcut testlerle tekrarlar ayıklandı mı?

☐ Red/Green kanıtı ve test günlükleri kaydedildi mi?

☐ Kod değişikliği ve upstream katkı için ayrıca izin alındı mı?
