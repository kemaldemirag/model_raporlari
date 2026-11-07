# Sprint-1 | sensor-estimation-pipeline

_Synthetic Sensor Calibration • Tek modül • Araştırma/uygulama hazırlığı_

## Hedef ve kapsam

**Repo:** sensor-estimation-pipeline

**Tek modül:** Synthetic Sensor Calibration

**Amaç:** Sabit tohumlu sentetik sensör verisinde ofset ve ölçek kalibrasyonu gerçekleştirmek

**Durum:** NOT_RUN — bu rapor plan ve kabul sözleşmesidir

## Önerilen repo dosya iskeleti

**Uygulama:** src/sensor_pipeline/calibration.py

**Test:** tests/test_calibration.py

**Fixture:** fixtures/synthetic_sensor/

**Çıktı:** outputs/calibration_metrics.json

**Yeniden üretim:** README içindeki sürüm-sabitlenmiş tek komut; henüz belirlenmedi

## Teknik test matrisi

- Bilinen gerçek parametreler

- train/test ayrımı

- bias ve RMSE

- tekrarlanabilir seed

- sıfır varyans sınırı

## Sprint iş akışı

- S1-01: Kaynak referanslarının tam SHA, lisans ve dosya yollarını doğrula.

- S1-02: Beklenen davranış ve toleransları bağımsız oracle ile yaz.

- S1-03: Pozitif, negatif ve sınır fixture oluştur (geliştirme onayından sonra).

- S1-04: Baseline / red-green / regresyon testlerini çalıştır ve logları sakla.

- S1-05: Kanıt manifesti, hata/başarı raporu ve yeniden üretim komutunu teslim et.

## Kabul kanıtı

- Hedef artefakt: outputs/calibration_metrics.json

- Tam commit SHA, sabit sürüm, fixture SHA-256, exact command, stdout/stderr, exit code, beklenen/gerçek sonuç.

- Tekrar koşusunda karar ve tolerans içi ölçüm tutarlı olmalı.

- Test çalışmadıysa NOT_RUN; varsayımsal başarı kaydı yok.

## Riskler ve sınırlar

- Kalman filtresi, gerçek sensör ve arıza tespiti sonraki sprint

- Referanslar: scipy/scipy; python-control/python-control

- Katkı repo kodu ve geliştirmeleri yalnız kendi upstream kapsamlarında kalır.

- Repo oluşturma, dosya yazma, fork, push ve PR ayrı onay gerektirir.
