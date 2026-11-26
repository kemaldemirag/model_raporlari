# 08.11.2026 | Codex Handoff

Çalışma alanı keşfi, güvenli oluşturma manifestosu ve uygulama kapıları

### Aşama 0 — Salt okunur keşif

- Mevcut standart çalışma alanının gerçek kökünü, klasör ağacını, git durumunu ve senkronizasyon durumunu doğrula.

- Mevcut altı ürünün isim veya içerik çakışması olup olmadığını kontrol et; varsayılan yeni üst klasör uydurma.

- Korunacak dosyaları, değiştirilecek dosyaları ve önerilen hedefleri manifestoda göster.

- Kaynakların sürüm, lisans ve erişim durumunu doğrula; kullanıcı onayı olmadan yazma yapma.

### Aşama 1 — Onaydan sonra

- Altı ürünü bağımsız repo olarak tasarla; önce RF-C Sprint 1 ve SCADA-01 Sprint 1'i uygula.

- Her repo için README, gereksinim, mimari, kaynak izlenebilirliği, test matrisi ve sprint kabul raporu oluştur.

- Yalnız testleri gerçekten çalıştırdıktan sonra PASS/FAIL/NOT\_RUN yaz.

- GitHub'a push, yeni repo açma, PR veya mevcut veriyi silme için ayrı onay iste.

### Teslim manifestosu

- RF-C → spatial-validation-audit → Sprint 1/2/3; kabul: Grup çakışması sıfır; tampon sınırı doğrulanmış; fold başına örnek sayısı ve worst-zone hata raporlanmış

- RF-01 → indoor-coverage-prediction → Sprint 1/2/3; kabul: Sabit girdide deterministik sonuç; fiziksel varsayımlar belgeli; sentetik/gerçek ölçüm ayrımı

- RF-02 → ap-placement-optimizer → Sprint 1/2/3; kabul: Yerleşimler kısıtları sağlıyor; baseline ile aynı koşullarda karşılaştırma var

- RF-03 → warehouse-radio-digital-twin → Sprint 1/2/3; kabul: Sabit seed/sürüm/sahneyle tekrar çalıştırılabilir; simülasyon ölçüm diye sunulmaz

- RF-B → wireless-fault-detection-benchmark → Sprint 1/2/3; kabul: Test verisi yeniden örneklenmez; gruplar ayrılır; azınlık metrikleri raporlanır

- SCADA-01 → industrial-telemetry-validation → Sprint 1/2/3; kabul: Kesinti ve yeniden bağlantı testleri çalıştırılmış; alarm ve stale-data ölçümleri kayıtlı

### Kaynak kayıtları

Arastirma RF: https://docs.google.com/document/d/1GkxPZPfsaspa20Sil9Qskj-ZKUuzQ-x1KhFwsyAOOsU/edit

Arastirma CV: https://docs.google.com/document/d/1iD9HCnpW51VkNZCm\_DsvJWDZbyhGB3lPf8i-lcmkrfQ/edit

Arastirma SCADA: https://docs.google.com/document/d/14s8FDGboFZmp3AD72m2Tdvuq9leIjCfDzdM1MMcklco/edit

Referans Listeler: https://docs.google.com/document/d/1B7zZrY9hStFuPgoG1e7ia9OY5CZBkm1X5oGl54fpgJo/edit

Is Ilanlari: https://docs.google.com/document/d/1-kVPmwgE0W5y0SRzQh0Xd2zCfEQmBs4kUXq-G9An0lg/edit

Kaynak listeleri arşiv bulgularıdır. İlan aktifliği, lisanslar, güncel commitler ve sürümler uygulama öncesinde yeniden doğrulanmalıdır.
