# 06.11.2026 — Copilot Ürün Çıkarımı

**Kapsam:** Araştırma ve planlama. Bu raporların hazırlanması geliştirme, repo/fork/PR oluşturma, upstream değişiklik veya sprint yürütme onayı değildir.

**Kanıt sınıfları:** VERIFIED (kaynak üzerinden incelendi), UNVERIFIED (henüz doğrulanmadı), BLOCKED (bağımlılık/izin engeli), NOT_RUN (test çalıştırılmadı), PROPOSED (taslak).

**Önemli sınır:** Repo iç iskeletleri taslaktır; kullanıcının standart yerel çalışma alanı klasör yapısı doğrulanmadan yerleştirme yolu atanmaz. Kaynak kodu yeniden kullanımı için lisans incelemesi gerekir. GitHub statik incelemesi çalışma testi yerine geçmez.

## Araştırmadan geliştirmeye kapılar
1. G0 — Kaynak envanteri: tam commit SHA, yol, lisans, test/CI varlığı ve iş ilanı kaynağı.
2. G1 — Gereksinim: ürün sözleşmesi, müşteri teslimatı, başarı ölçütü; varsayımlar ayrı.
3. G2 — Test orakılı: beklenen sonuç, negatif/boundary senaryosu, kaynak dayanağı.
4. G3 — Onay: kullanıcı açıkça geliştirmeye izin verirse çalışma alanı doğrulaması, repo stratejisi ve sprint başlangıcı.
5. G4 — Uygulama: yalnız onaylı kapsam, bağımsız repo/upstream PR için ayrıca onay.
6. G5 — Teslim: test sonuçları, kaynak manifesti, NOT_RUN/BLOCKED listesi, tekrarlanabilir komutlar.

## Çalışma hatları
Portfolyo ürünleri, upstream katkılar, fork sprintleri ve Model Bağımsız Çalışmaları birbirine karıştırılmaz. Mevcut standart çalışma alanı okunup doğrulanmadan klasör yolu uydurulmaz.
