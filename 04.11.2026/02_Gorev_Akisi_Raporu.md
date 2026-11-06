# 06.11.2026 — Copilot Ürün Çıkarımı

**Kapsam:** Araştırma ve planlama. Bu raporların hazırlanması geliştirme, repo/fork/PR oluşturma, upstream değişiklik veya sprint yürütme onayı değildir.

**Kanıt sınıfları:** VERIFIED (kaynak üzerinden incelendi), UNVERIFIED (henüz doğrulanmadı), BLOCKED (bağımlılık/izin engeli), NOT_RUN (test çalıştırılmadı), PROPOSED (taslak).

**Önemli sınır:** Repo iç iskeletleri taslaktır; kullanıcının standart yerel çalışma alanı klasör yapısı doğrulanmadan yerleştirme yolu atanmaz. Kaynak kodu yeniden kullanımı için lisans incelemesi gerekir. GitHub statik incelemesi çalışma testi yerine geçmez.

## Araştırma görev kuyruğu
| ID | Görev | Kabul kanıtı | Durum |
|---|---|---|---|
| T-01 | L3G4200D BLE/BDU ve readBytes semantiğini datasheet ile karşılaştır | kaynak-satır matrisi | OPEN |
| T-02 | INT1 high/low eşik, latch ve WAIT semantiği | register orakılı | OPEN |
| T-03 | PF-D şema ve çıkış kodu kontratını sabitle | örnek geçerli/geçersiz raporlar | OPEN |
| T-04 | PF-A yanlış alarm ve gecikme kriterlerini tanımla | ölçülebilir kabul koşulları | OPEN |
| T-05 | MQTT replay sıra/kayıp modelini belirle | deterministik fixture taslağı | OPEN |
| T-06 | Lisans ve kaynak dosya kullanımı | kaynak bazlı izin matrisi | OPEN |
| T-07 | İş ilanlarını özgün kaynağından doğrula | ilan URL, tarih, bütçe, durum | OPEN |

## Bağımlılık
T-01/T-02 → PF-E; T-03 → PF-D; PF-E → PF-A; PF-A + T-05 → IoT; tüm hatlar → PF-D kanıt paketi.
