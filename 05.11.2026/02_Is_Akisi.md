# 02 — İş Akışı | Portfolyo Ürün Çıkarımı

_Araştırma → doğrulama → geliştirme → kanıt → kabul_

## Politika ve sınırlar

- Katkı Yaptığımız Repolar üzerindeki geliştirmeler yalnızca ilgili upstream reponun kapsamında geçerlidir.

- Sekiz ürün bağımsız, henüz oluşturulmamış repo adaylarıdır. Kaynak kod kopyalama lisans ve ayrıca onay gerektirir.

- Mevcut standart çalışma alanının klasör yapısı değiştirilmez; bu belgelerdeki dosya yolları yeni repo için öneridir.

- GitHub üzerinde repo oluşturma, fork, push, PR, klon veya kod değişikliği için ayrıca izin alınır.

- Bu belgelerde hiçbir testin çalıştırıldığı iddia edilmez; başlangıç durumu NOT_RUN.

## Kaynak ve kanıt düzeyi

- Ürün iskeletleri önceki sohbet araştırmasından türetilmiştir; önerilen yeni dosyalar henüz mevcut değildir.

- Referans repolar: GitHub üzerinde tespit edildi; her ürün için lisans, commit, dosya ve test kapsamı yeniden doğrulanmalıdır.

- Durumlar: VERIFIED (kaynak incelendi), INFERRED (çıkarım), UNVERIFIED (henüz doğrulanmadı), NOT_RUN (test yürütülmedi), BLOCKED (bağımlılık/izin).

## Aşama kapıları

- A0 — Referans repo ve kaynak dosya araştırması (salt-okunur).

- A1 — Lisans, tam commit SHA, araç zinciri ve mevcut testlerin denetimi.

- A2 — Tek modüllü Sprint-1 sözleşmesi ve negatif testlerin tasarımı.

- A3 — Ayrı onayla yerel geliştirme; red/green ve regresyon.

- A4 — Komut, ortam, fixture, oracle, log, artefakt hash ve sonuç paketleme.

- A5 — Portfolyo değerlendirmesi; GitHub yayınlama ayrı onay.

## Yeniden üretilebilirlik standardı

- Bir çalıştırma komutu; sabitlenmiş bağımlılıklar; fixture hash; beklenen/gözlenen sonuç; tolerans; test exit code.

- Windows/Linux farkları ve KiCad/HDL/SPICE sürüm farkları açıkça belgelenir.

- Çıktılar artefakt olarak saklanır, kaynak repo içine otomatik commit edilmez.
