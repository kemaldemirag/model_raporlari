# 08.11.2026 | Model Raporları

Ürün çıkarımı • Karar ve izlenebilirlik raporu • Durum: PLAN / NOT\_RUN

Bu paket 08.11.2026 tarihli üç Araştırma Kayıtları belgesi, bir Referans Listeler ve bir İş İlanları belgesi esas alınarak hazırlanmıştır. Altı aday bağımsız ürünlerdir; mevcut PF kimlikleriyle eşitlenmez.

| Kod | Repo önerisi | İlan eşleşmesi |
| --- | --- | --- |
| RF-C | spatial-validation-audit | Python GIS / UAV Sensor ML / PostGIS |
| RF-01 | indoor-coverage-prediction | iBwave Wi-Fi Design Engineer (tematik eşleşme) |
| RF-02 | ap-placement-optimizer | Wi-Fi kapsama ve AP yerleşimi |
| RF-03 | warehouse-radio-digital-twin | Depo içi RF simülasyonu; doğrudan ilan eşleşmesi kanıtlanmadı |
| RF-B | wireless-fault-detection-benchmark | UAV Sensor ML; arıza tespiti (tematik eşleşme) |
| SCADA-01 | industrial-telemetry-validation | Ignition HMI, PLC/SCADA, PLC-to-MQTT gateway |

### Öncelik ve gerekçe

- 1. RF-C: Random/spatial CV farkı, veri sızıntısı ve dengesizlik için en somut bağımsız test çekirdeği.

- 2. SCADA-01: Donanımsız Modbus/OpenPLC/FUXA entegrasyonu ile farklı bir portfolyo hattı.

- 3. RF-01: RF-C doğrulamasını kullanabilecek kapsama modeli; saha verisi açık.

- 4. RF-02: RF-01 üzerine kurulu optimizasyon.

- 5. RF-B: Etiketli gerçek arıza verisine bağlı benchmark.

- 6. RF-03: Daha yüksek simülasyon/kurulum maliyetli 3D dijital ikiz.

### Çalışma alanı güvenlik sınırları

- Standart klasör yapısı canlı ortamdan okunmadan üst dizin oluşturulmayacak.

- Projelerim, Model Bağımsız Çalışmaları, Portfolyo ve Katkı Yaptığım Repolar ayrı hatlardır; hedef hat kullanıcı onayına tabidir.

- Yeni GitHub reposu, upstream PR, silme ve senkronizasyon değişikliği ayrı onay gerektirir.

- PASS / FAIL / NOT\_RUN ayrımı zorunludur; bu pakette hiçbir test çalıştırılmamıştır.

### Kaynak kayıtları

Arastirma RF: https://docs.google.com/document/d/1GkxPZPfsaspa20Sil9Qskj-ZKUuzQ-x1KhFwsyAOOsU/edit

Arastirma CV: https://docs.google.com/document/d/1iD9HCnpW51VkNZCm\_DsvJWDZbyhGB3lPf8i-lcmkrfQ/edit

Arastirma SCADA: https://docs.google.com/document/d/14s8FDGboFZmp3AD72m2Tdvuq9leIjCfDzdM1MMcklco/edit

Referans Listeler: https://docs.google.com/document/d/1B7zZrY9hStFuPgoG1e7ia9OY5CZBkm1X5oGl54fpgJo/edit

Is Ilanlari: https://docs.google.com/document/d/1-kVPmwgE0W5y0SRzQh0Xd2zCfEQmBs4kUXq-G9An0lg/edit

Kaynak listeleri arşiv bulgularıdır. İlan aktifliği, lisanslar, güncel commitler ve sürümler uygulama öncesinde yeniden doğrulanmalıdır.
