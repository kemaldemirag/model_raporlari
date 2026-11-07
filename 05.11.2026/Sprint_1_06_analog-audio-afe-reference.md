# Sprint-1 | analog-audio-afe-reference

_AC Gain Reference Simulation • Tek modül • Araştırma/uygulama hazırlığı_

## Hedef ve kapsam

**Repo:** analog-audio-afe-reference

**Tek modül:** AC Gain Reference Simulation

**Amaç:** Basit ve özgün bir analog ön uç devresinin AC kazanç ve bant genişliğini SPICE ile ölçmek

**Durum:** NOT_RUN — bu rapor plan ve kabul sözleşmesidir

## Önerilen repo dosya iskeleti

**Uygulama:** simulation/ac_analysis.cir

**Test:** tests/test_gain.py

**Fixture:** simulation/models/

**Çıktı:** outputs/frequency_response.csv

**Yeniden üretim:** README içindeki sürüm-sabitlenmiş tek komut; henüz belirlenmedi

## Teknik test matrisi

- Sabit netlist

- AC sweep

- kazanç referansı

- -3 dB frekansı

- simülatör sürümü ve model checksum

## Sprint iş akışı

- S1-01: Kaynak referanslarının tam SHA, lisans ve dosya yollarını doğrula.

- S1-02: Beklenen davranış ve toleransları bağımsız oracle ile yaz.

- S1-03: Pozitif, negatif ve sınır fixture oluştur (geliştirme onayından sonra).

- S1-04: Baseline / red-green / regresyon testlerini çalıştır ve logları sakla.

- S1-05: Kanıt manifesti, hata/başarı raporu ve yeniden üretim komutunu teslim et.

## Kabul kanıtı

- Hedef artefakt: outputs/frequency_response.csv

- Tam commit SHA, sabit sürüm, fixture SHA-256, exact command, stdout/stderr, exit code, beklenen/gerçek sonuç.

- Tekrar koşusunda karar ve tolerans içi ölçüm tutarlı olmalı.

- Test çalışmadıysa NOT_RUN; varsayımsal başarı kaydı yok.

## Riskler ve sınırlar

- KiCad PCB, gürültü ve fiziksel doğrulama bu sprintte yok

- Referanslar: PySpice-org/PySpice; ngspice

- Katkı repo kodu ve geliştirmeleri yalnız kendi upstream kapsamlarında kalır.

- Repo oluşturma, dosya yazma, fork, push ve PR ayrı onay gerektirir.
