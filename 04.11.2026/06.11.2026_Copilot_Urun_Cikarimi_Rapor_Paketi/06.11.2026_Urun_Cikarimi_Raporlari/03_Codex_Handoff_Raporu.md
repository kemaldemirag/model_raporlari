# 06.11.2026 — Copilot Ürün Çıkarımı

**Kapsam:** Araştırma ve planlama. Bu raporların hazırlanması geliştirme, repo/fork/PR oluşturma, upstream değişiklik veya sprint yürütme onayı değildir.

**Kanıt sınıfları:** VERIFIED (kaynak üzerinden incelendi), UNVERIFIED (henüz doğrulanmadı), BLOCKED (bağımlılık/izin engeli), NOT_RUN (test çalıştırılmadı), PROPOSED (taslak).

**Önemli sınır:** Repo iç iskeletleri taslaktır; kullanıcının standart yerel çalışma alanı klasör yapısı doğrulanmadan yerleştirme yolu atanmaz. Kaynak kodu yeniden kullanımı için lisans incelemesi gerekir. GitHub statik incelemesi çalışma testi yerine geçmez.

## Codex devir sınırı
**Şimdilik araştırma görevi; hiçbir kod yazılmayacak.** Codex'e verilecek taslak talimat: “Yalnız salt-okunur incele; verilen upstream dosyalarının tam SHA ve yollarını doğrula. BLE/BDU, INT1 ve readBytes için sözleşme/uygulama farkı matrisi hazırla. Test çalıştırma, klonlama, dosya yazma, commit, branch, fork ve PR yok. Bulguları VERIFIED/UNVERIFIED/NOT_RUN/BLOCKED olarak işaretle.”

## Geliştirme için ayrı onay kapısı
Onay gelirse: standart çalışma alanı gerçek yapısını oku; hedef repo/çalışma hattı seçimini kullanıcıyla netleştir; yalnız onaylı Sprint-1 kapsamını uygula; otomatik upstream PR açma. Her sprintte diff, test logu, sürüm manifesti, risk ve geri alma notu sun.

## Kabul
Raporun kaynak commitleri ve kod satırları izlenebilir olmalı. Doğrulanmamış hataya “bug fixed” denmemeli.
