# Sprint-1 | i2c-register-validation-bench

_Register Read/Write Model • Tek modül • Araştırma/uygulama hazırlığı_

## Hedef ve kapsam

**Repo:** i2c-register-validation-bench

**Tek modül:** Register Read/Write Model

**Amaç:** Sentetik register haritasında reset, okuma, yazma, read-only ve geçersiz adres sözleşmesini test etmek

**Durum:** NOT_RUN — bu rapor plan ve kabul sözleşmesidir

## Önerilen repo dosya iskeleti

**Uygulama:** src/i2c_bench/register_map.py

**Test:** tests/test_register_read_write.py

**Fixture:** fixtures/register_maps/

**Çıktı:** outputs/register_transactions.json

**Yeniden üretim:** README içindeki sürüm-sabitlenmiş tek komut; henüz belirlenmedi

## Teknik test matrisi

- Reset varsayılanları

- RW/RO davranışı

- bilinmeyen adres

- art arda yazma

- oracle eşleşmesi

## Sprint iş akışı

- S1-01: Kaynak referanslarının tam SHA, lisans ve dosya yollarını doğrula.

- S1-02: Beklenen davranış ve toleransları bağımsız oracle ile yaz.

- S1-03: Pozitif, negatif ve sınır fixture oluştur (geliştirme onayından sonra).

- S1-04: Baseline / red-green / regresyon testlerini çalıştır ve logları sakla.

- S1-05: Kanıt manifesti, hata/başarı raporu ve yeniden üretim komutunu teslim et.

## Kabul kanıtı

- Hedef artefakt: outputs/register_transactions.json

- Tam commit SHA, sabit sürüm, fixture SHA-256, exact command, stdout/stderr, exit code, beklenen/gerçek sonuç.

- Tekrar koşusunda karar ve tolerans içi ölçüm tutarlı olmalı.

- Test çalışmadıysa NOT_RUN; varsayımsal başarı kaydı yok.

## Riskler ve sınırlar

- İlk sprint yazılım register modeli; HDL/cocotb entegrasyonu bu sprintte yok

- Referanslar: cocotb/cocotb; alexforencich/cocotbext-i2c

- Katkı repo kodu ve geliştirmeleri yalnız kendi upstream kapsamlarında kalır.

- Repo oluşturma, dosya yazma, fork, push ve PR ayrı onay gerektirir.
