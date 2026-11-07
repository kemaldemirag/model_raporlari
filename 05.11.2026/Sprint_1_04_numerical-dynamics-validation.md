# Sprint-1 | numerical-dynamics-validation

_Damped Oscillator Euler/RK4 • Tek modül • Araştırma/uygulama hazırlığı_

## Hedef ve kapsam

**Repo:** numerical-dynamics-validation

**Tek modül:** Damped Oscillator Euler/RK4

**Amaç:** Sönümlü ikinci dereceden sistem için Euler ve RK4 çözümlerini bağımsız analitik çözümle karşılaştırmak

**Durum:** NOT_RUN — bu rapor plan ve kabul sözleşmesidir

## Önerilen repo dosya iskeleti

**Uygulama:** src/numerical_validation/models/damped_oscillator.py

**Test:** tests/test_solver_accuracy.py

**Fixture:** fixtures/oscillator_cases/

**Çıktı:** outputs/error_vs_timestep.csv

**Yeniden üretim:** README içindeki sürüm-sabitlenmiş tek komut; henüz belirlenmedi

## Teknik test matrisi

- Sabit parametreler

- h ve h/2

- RMS ve maksimum hata

- yakınsama eğimi

- kritik/aşırı sönüm kapsam dışı

## Sprint iş akışı

- S1-01: Kaynak referanslarının tam SHA, lisans ve dosya yollarını doğrula.

- S1-02: Beklenen davranış ve toleransları bağımsız oracle ile yaz.

- S1-03: Pozitif, negatif ve sınır fixture oluştur (geliştirme onayından sonra).

- S1-04: Baseline / red-green / regresyon testlerini çalıştır ve logları sakla.

- S1-05: Kanıt manifesti, hata/başarı raporu ve yeniden üretim komutunu teslim et.

## Kabul kanıtı

- Hedef artefakt: outputs/error_vs_timestep.csv

- Tam commit SHA, sabit sürüm, fixture SHA-256, exact command, stdout/stderr, exit code, beklenen/gerçek sonuç.

- Tekrar koşusunda karar ve tolerans içi ölçüm tutarlı olmalı.

- Test çalışmadıysa NOT_RUN; varsayımsal başarı kaydı yok.

## Riskler ve sınırlar

- Analitik çözüm koşulları, tolerans ve kayan nokta hassasiyeti belgelenmeli

- Referanslar: scipy/scipy; python-control/python-control

- Katkı repo kodu ve geliştirmeleri yalnız kendi upstream kapsamlarında kalır.

- Repo oluşturma, dosya yazma, fork, push ve PR ayrı onay gerektirir.
