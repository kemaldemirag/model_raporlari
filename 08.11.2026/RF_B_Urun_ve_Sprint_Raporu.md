# RF-B | Wireless Fault Detection Benchmark

08.11.2026 • Ürün spesifikasyonu + Sprint 1–3 • Durum: PLAN / NOT\_RUN

### Ürün tanımı

- Önerilen repo: wireless-fault-detection-benchmark

- İlan bağlantısı / beceri eşleşmesi: UAV Sensor ML; arıza tespiti (tematik eşleşme)

- Referans teknolojiler: imbalanced-learn, scikit-learn, Roberts et al. 2017

- Ürün bağımsız tutulacak; ortak modül kullanımı açık arayüz ve sürüm sözleşmesi gerektirir.

### Önerilen repo iskeleti

- Teknik alt dizinler: data/, labels/, splits/, models/, metrics/, tests/, reports/

- Ortak belgeler: README.md, docs/requirements.md, docs/architecture.md, docs/verification-matrix.md, docs/source-traceability.md

- CI, bağımlılık kilitleme ve örnek veri dosyaları uygulama aşamasında eklenecek.

### Sprint 1 — Kurulum ve çekirdek

- Etiketli veri envanteri ve arıza taksonomisi

- Teslim: veri/arayüz sözleşmesi, en az bir çalışan baseline örneği, birim test taslakları.

### Sprint 2 — Doğrulama

- Group/Temporal CV, baseline ve eğitim içi dengeleme

- Teslim: otomatik testler, negatif test senaryoları, başarısızlık nedenlerinin kaydı.

### Sprint 3 — Mühendislik kanıtı

- PR-AUC, minority recall, yanlış alarm ve belirsizlik

- Önerilen çıktılar: fault\_metrics.csv; split\_audit.json; confusion\_matrix.png; benchmark\_report.pdf

### Kabul kapısı

- Test verisi yeniden örneklenmez; gruplar ayrılır; azınlık metrikleri raporlanır

- Tüm testlerin çalıştırılma komutu, paket sürümleri, seed ve ham çıktı kaydedilir.

- NOT\_RUN sonucu PASS olarak raporlanmaz.

### Riskler ve açık kararlar

- Gerçek arıza etiketi yoksa yalnız sentetik demonstrasyon

- Veri lisansı, kaynak repo lisansı, sürüm/commit ve ilan aktifliği uygulama öncesinde doğrulanacak.

- Donanım veya saha ölçümü yapılmadığı sürece fiziksel doğrulama iddiası kurulmayacak.

### Kaynak kayıtları

Arastirma RF: https://docs.google.com/document/d/1GkxPZPfsaspa20Sil9Qskj-ZKUuzQ-x1KhFwsyAOOsU/edit

Arastirma CV: https://docs.google.com/document/d/1iD9HCnpW51VkNZCm\_DsvJWDZbyhGB3lPf8i-lcmkrfQ/edit

Arastirma SCADA: https://docs.google.com/document/d/14s8FDGboFZmp3AD72m2Tdvuq9leIjCfDzdM1MMcklco/edit

Referans Listeler: https://docs.google.com/document/d/1B7zZrY9hStFuPgoG1e7ia9OY5CZBkm1X5oGl54fpgJo/edit

Is Ilanlari: https://docs.google.com/document/d/1-kVPmwgE0W5y0SRzQh0Xd2zCfEQmBs4kUXq-G9An0lg/edit

Kaynak listeleri arşiv bulgularıdır. İlan aktifliği, lisanslar, güncel commitler ve sürümler uygulama öncesinde yeniden doğrulanmalıdır.
