# Sprint-1 | motor-control-verification

_PWM Duty-Cycle Verification • Tek modül • Araştırma/uygulama hazırlığı_

## Hedef ve kapsam

**Repo:** motor-control-verification

**Tek modül:** PWM Duty-Cycle Verification

**Amaç:** Sentetik PWM üreticisinin görev çevrimi, periyot ve sınır girişlerini deterministik sanal saatle doğrulamak

**Durum:** NOT_RUN — bu rapor plan ve kabul sözleşmesidir

## Önerilen repo dosya iskeleti

**Uygulama:** src/pwm/generator.py

**Test:** tests/test_pwm.py

**Fixture:** fixtures/pwm_cases/

**Çıktı:** outputs/pwm_trace.csv

**Yeniden üretim:** README içindeki sürüm-sabitlenmiş tek komut; henüz belirlenmedi

## Teknik test matrisi

- 0%, 50%, 100%

- negatif ve >100% girdiler

- periyot sınırları

- yükselen/düşen kenar olayları

## Sprint iş akışı

- S1-01: Kaynak referanslarının tam SHA, lisans ve dosya yollarını doğrula.

- S1-02: Beklenen davranış ve toleransları bağımsız oracle ile yaz.

- S1-03: Pozitif, negatif ve sınır fixture oluştur (geliştirme onayından sonra).

- S1-04: Baseline / red-green / regresyon testlerini çalıştır ve logları sakla.

- S1-05: Kanıt manifesti, hata/başarı raporu ve yeniden üretim komutunu teslim et.

## Kabul kanıtı

- Hedef artefakt: outputs/pwm_trace.csv

- Tam commit SHA, sabit sürüm, fixture SHA-256, exact command, stdout/stderr, exit code, beklenen/gerçek sonuç.

- Tekrar koşusunda karar ve tolerans içi ölçüm tutarlı olmalı.

- Test çalışmadıysa NOT_RUN; varsayımsal başarı kaydı yok.

## Riskler ve sınırlar

- Fiziksel GPIO ve motor akımı ölçülmez; PI/PID sonraki sprint

- Referanslar: python-control/python-control

- Katkı repo kodu ve geliştirmeleri yalnız kendi upstream kapsamlarında kalır.

- Repo oluşturma, dosya yazma, fork, push ve PR ayrı onay gerektirir.
