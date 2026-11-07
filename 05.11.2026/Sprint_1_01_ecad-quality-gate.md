# Sprint-1 | ecad-quality-gate

_CLI Exit-Code Classification • Tek modül • Araştırma/uygulama hazırlığı_

## Hedef ve kapsam

**Repo:** ecad-quality-gate

**Tek modül:** CLI Exit-Code Classification

**Amaç:** KiCad CLI ERC/DRC süreç çıkış kodlarını, bayrakları ve rapor varlığını birlikte sınıflandırmak

**Durum:** NOT_RUN — bu rapor plan ve kabul sözleşmesidir

## Önerilen repo dosya iskeleti

**Uygulama:** src/ecad_gate/runner.py

**Test:** tests/test_exit_codes.py

**Fixture:** fixtures/cli_cases/

**Çıktı:** outputs/cli_classification.json

**Yeniden üretim:** README içindeki sürüm-sabitlenmiş tek komut; henüz belirlenmedi

## Teknik test matrisi

- KiCad sürüm/komut/flag matrisi

- exit=0 fakat ihlal içeren rapor

- exit=5

- eksik binary

- timeout

- exit=0 fakat rapor yok

## Sprint iş akışı

- S1-01: Kaynak referanslarının tam SHA, lisans ve dosya yollarını doğrula.

- S1-02: Beklenen davranış ve toleransları bağımsız oracle ile yaz.

- S1-03: Pozitif, negatif ve sınır fixture oluştur (geliştirme onayından sonra).

- S1-04: Baseline / red-green / regresyon testlerini çalıştır ve logları sakla.

- S1-05: Kanıt manifesti, hata/başarı raporu ve yeniden üretim komutunu teslim et.

## Kabul kanıtı

- Hedef artefakt: outputs/cli_classification.json

- Tam commit SHA, sabit sürüm, fixture SHA-256, exact command, stdout/stderr, exit code, beklenen/gerçek sonuç.

- Tekrar koşusunda karar ve tolerans içi ölçüm tutarlı olmalı.

- Test çalışmadıysa NOT_RUN; varsayımsal başarı kaydı yok.

## Riskler ve sınırlar

- KiCad sürüm farkları ve --exit-code-violations kullanımı; gerçek fixture üretimi

- Referanslar: INTI-CMNB/KiBot; productize/kicad-automation-scripts

- Katkı repo kodu ve geliştirmeleri yalnız kendi upstream kapsamlarında kalır.

- Repo oluşturma, dosya yazma, fork, push ve PR ayrı onay gerektirir.
