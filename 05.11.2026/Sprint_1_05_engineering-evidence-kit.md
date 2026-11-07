# Sprint-1 | engineering-evidence-kit

_Evidence Schema & Integrity • Tek modül • Araştırma/uygulama hazırlığı_

## Hedef ve kapsam

**Repo:** engineering-evidence-kit

**Tek modül:** Evidence Schema & Integrity

**Amaç:** Test kanıtının JSON Schema, artefakt varlığı ve SHA-256 bütünlüğünü doğrulamak

**Durum:** NOT_RUN — bu rapor plan ve kabul sözleşmesidir

## Önerilen repo dosya iskeleti

**Uygulama:** src/evidence_kit/validator.py

**Test:** tests/test_hash_integrity.py

**Fixture:** fixtures/evidence_cases/

**Çıktı:** outputs/validation_summary.json

**Yeniden üretim:** README içindeki sürüm-sabitlenmiş tek komut; henüz belirlenmedi

## Teknik test matrisi

- Geçerli paket

- bozuk JSON

- eksik artefakt

- hash uyuşmazlığı

- beklenmeyen alan

- deterministik karar

## Sprint iş akışı

- S1-01: Kaynak referanslarının tam SHA, lisans ve dosya yollarını doğrula.

- S1-02: Beklenen davranış ve toleransları bağımsız oracle ile yaz.

- S1-03: Pozitif, negatif ve sınır fixture oluştur (geliştirme onayından sonra).

- S1-04: Baseline / red-green / regresyon testlerini çalıştır ve logları sakla.

- S1-05: Kanıt manifesti, hata/başarı raporu ve yeniden üretim komutunu teslim et.

## Kabul kanıtı

- Hedef artefakt: outputs/validation_summary.json

- Tam commit SHA, sabit sürüm, fixture SHA-256, exact command, stdout/stderr, exit code, beklenen/gerçek sonuç.

- Tekrar koşusunda karar ve tolerans içi ölçüm tutarlı olmalı.

- Test çalışmadıysa NOT_RUN; varsayımsal başarı kaydı yok.

## Riskler ve sınırlar

- İlk sprint yalnızca kanıt doğrulayıcı; rapor toplama/CI entegrasyonu sonra

- Referanslar: HypothesisWorks/hypothesis

- Katkı repo kodu ve geliştirmeleri yalnız kendi upstream kapsamlarında kalır.

- Repo oluşturma, dosya yazma, fork, push ve PR ayrı onay gerektirir.
