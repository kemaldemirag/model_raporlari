# SPRINT-06 | Sayısal Dinamikler ve Mod Geçişleri

_03.11.2026  •  Araştırma ve uygulama hazırlığı  •  Salt okunur değerlendirme_

## 1. Hedef ve kapsam

Repository: https://github.com/prosysim-xr/gimble-motor-spring<br>
Hedef: Bu repoda kanıtlanabilir test boşlukları, gerçek API sözleşmeleri ve akademik doğrulama senaryoları belirlemek. Bu belge yürütülmüş test veya onaylanmış upstream katkı raporu değildir.

## 2. Dosya yolu odaklı inceleme

- Assets/_project/Scripts/Motor.cs

## 3. Geliştirme iş paketleri

1. S6-01 — İkinci dereceden sönümlü yay modelini analitik çözümle karşılaştır.

1. S6-02 — Yarı örtük Euler adımında dt duyarlılığı ve kararlılık bölgesini ölç.

1. S6-03 — Aktif/pasif mod değişiminde springVelocity sıfırlamasını ve süreksizliği kaydet.

1. S6-04 — maxAngle=0 ve büyük dt için parametre geçerliliğini değerlendir.

## 4. Model–Test eşleştirmesi

| Test ekseni | Oracle / yöntem | Kanıt |
| --- | --- | --- |
| T-01 | İkinci dereceden sönümlü yay modelini analitik çözümle karşılaştır. | Commit SHA + komut + log + beklenen/gerçekleşen |
| T-02 | Yarı örtük Euler adımında dt duyarlılığı ve kararlılık bölgesini ölç. | Commit SHA + komut + log + beklenen/gerçekleşen |
| T-03 | Aktif/pasif mod değişiminde springVelocity sıfırlamasını ve süreksizliği kaydet. | Commit SHA + komut + log + beklenen/gerçekleşen |

## 5. Kabul kriterleri

- Farklı dt değerlerinde hata eğrileri ve analitik referans

- Mod geçişi olay izleri

- Parametre sınırlarında kontrollü başarısızlık veya koruma

Ortak kalite kapısı: mevcut baseline kayıt altına alınır; yeni testler önce hatayı veya kapsam boşluğunu kanıtlar; düzeltme sonrası aynı komutla tekrar çalıştırılır. Derleme/test yapılmadıysa sonuç NOT RUN olarak işaretlenir.

## 6. Akademik ve teknik referanslar

- Kontrol teorisi: ikinci dereceden sistemler

- Hairer, Nørsett & Wanner — Solving Ordinary Differential Equations I, Springer

- NIST — Verification, Validation and Uncertainty Quantification

## 7. Risk, sınırlar ve karar kapısı

Kod adlarının ve işlevlerin bazıları önceki kaynak incelemelerinden alınmıştır; güncel commit, lisans, test altyapısı ve upstream katkı koşulları uygulama öncesi yeniden doğrulanmalıdır. Mock/host-test fiziksel güvenlik kanıtı değildir. Başka repo kodu lisans incelemesi olmadan kopyalanmamalıdır.

İzin sınırı: mevcut 02.11.2026 çalışma alanında yalnız IRremoteESP8266 doğrulanmıştır. Bu repo aday olarak kalır; yeni GitHub repo, fork, push veya PR onaysız yapılmaz.

## 8. Codex devir kontrol listesi

☐ Varsayılan dal ve tam commit SHA sabitlendi mi?

☐ Lisans ve test komutları doğrulandı mı?

☐ Mevcut testlerle tekrarlar ayıklandı mı?

☐ Red/Green kanıtı ve test günlükleri kaydedildi mi?

☐ Kod değişikliği ve upstream katkı için ayrıca izin alındı mı?
