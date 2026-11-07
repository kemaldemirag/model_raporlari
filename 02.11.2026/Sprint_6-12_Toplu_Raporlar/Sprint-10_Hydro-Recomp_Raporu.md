# SPRINT-10 | Dönüşüm Zinciri ve Quaternion Doğrulaması

_03.11.2026  •  Araştırma ve uygulama hazırlığı  •  Salt okunur değerlendirme_

## 1. Hedef ve kapsam

Repository: https://github.com/NextrixVFX/Hydro-Recomp<br>
Hedef: Bu repoda kanıtlanabilir test boşlukları, gerçek API sözleşmeleri ve akademik doğrulama senaryoları belirlemek. Bu belge yürütülmüş test veya onaylanmış upstream katkı raporu değildir.

## 2. Dosya yolu odaklı inceleme

- hydro/workspace/core/assets/skin.hxx

## 3. Geliştirme iş paketleri

1. S10-01 — Yerel ve dünya koordinat dönüşümü sırasını bağımsız referansla sınayacak testler tasarla.

1. S10-02 — Quaternion lerp ve normalize etme işlemlerinde uç değerleri incele.

1. S10-03 — Parent-child kemik zincirinde frame 0 ile bind pose eşleşmesini ölç.

1. S10-04 — Sıfır uzunluklu quaternion ve parent indeks sınırlarını değerlendir.

## 4. Model–Test eşleştirmesi

| Test ekseni | Oracle / yöntem | Kanıt |
| --- | --- | --- |
| T-01 | Yerel ve dünya koordinat dönüşümü sırasını bağımsız referansla sınayacak testler tasarla. | Commit SHA + komut + log + beklenen/gerçekleşen |
| T-02 | Quaternion lerp ve normalize etme işlemlerinde uç değerleri incele. | Commit SHA + komut + log + beklenen/gerçekleşen |
| T-03 | Parent-child kemik zincirinde frame 0 ile bind pose eşleşmesini ölç. | Commit SHA + komut + log + beklenen/gerçekleşen |

## 5. Kabul kriterleri

- Matris çarpım sırası için altın referans testleri

- Quaternion normu ve poz sapması raporu

- Animasyon görsel regresyon ölçütleri

Ortak kalite kapısı: mevcut baseline kayıt altına alınır; yeni testler önce hatayı veya kapsam boşluğunu kanıtlar; düzeltme sonrası aynı komutla tekrar çalıştırılır. Derleme/test yapılmadıysa sonuç NOT RUN olarak işaretlenir.

## 6. Akademik ve teknik referanslar

- Shoemake (1985) — Animating Rotation with Quaternion Curves

- Robotics: Modelling, Planning and Control — Siciliano et al.

- Numerical methods and floating-point error

## 7. Risk, sınırlar ve karar kapısı

Kod adlarının ve işlevlerin bazıları önceki kaynak incelemelerinden alınmıştır; güncel commit, lisans, test altyapısı ve upstream katkı koşulları uygulama öncesi yeniden doğrulanmalıdır. Mock/host-test fiziksel güvenlik kanıtı değildir. Başka repo kodu lisans incelemesi olmadan kopyalanmamalıdır.

İzin sınırı: mevcut 02.11.2026 çalışma alanında yalnız IRremoteESP8266 doğrulanmıştır. Bu repo aday olarak kalır; yeni GitHub repo, fork, push veya PR onaysız yapılmaz.

## 8. Codex devir kontrol listesi

☐ Varsayılan dal ve tam commit SHA sabitlendi mi?

☐ Lisans ve test komutları doğrulandı mı?

☐ Mevcut testlerle tekrarlar ayıklandı mı?

☐ Red/Green kanıtı ve test günlükleri kaydedildi mi?

☐ Kod değişikliği ve upstream katkı için ayrıca izin alındı mı?
