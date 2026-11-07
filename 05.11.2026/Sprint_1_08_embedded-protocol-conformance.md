# Sprint-1 | embedded-protocol-conformance

_Frame Codec Golden Vectors • Tek modül • Araştırma/uygulama hazırlığı_

## Hedef ve kapsam

**Repo:** embedded-protocol-conformance

**Tek modül:** Frame Codec Golden Vectors

**Amaç:** Özgün ve belgeli örnek çerçeve formatında encode/decode tutarlılığını ve negatif girdileri sınamak

**Durum:** NOT_RUN — bu rapor plan ve kabul sözleşmesidir

## Önerilen repo dosya iskeleti

**Uygulama:** src/protocol_conformance/frame_parser.py

**Test:** tests/test_encoding.py

**Fixture:** protocols/example_protocol/golden_vectors.json

**Çıktı:** outputs/vector_results.json

**Yeniden üretim:** README içindeki sürüm-sabitlenmiş tek komut; henüz belirlenmedi

## Teknik test matrisi

- Golden vector

- CRC/checksum seçimi

- truncated frame

- yanlış uzunluk

- round-trip

- yanlış checksum

## Sprint iş akışı

- S1-01: Kaynak referanslarının tam SHA, lisans ve dosya yollarını doğrula.

- S1-02: Beklenen davranış ve toleransları bağımsız oracle ile yaz.

- S1-03: Pozitif, negatif ve sınır fixture oluştur (geliştirme onayından sonra).

- S1-04: Baseline / red-green / regresyon testlerini çalıştır ve logları sakla.

- S1-05: Kanıt manifesti, hata/başarı raporu ve yeniden üretim komutunu teslim et.

## Kabul kanıtı

- Hedef artefakt: outputs/vector_results.json

- Tam commit SHA, sabit sürüm, fixture SHA-256, exact command, stdout/stderr, exit code, beklenen/gerçek sonuç.

- Tekrar koşusunda karar ve tolerans içi ölçüm tutarlı olmalı.

- Test çalışmadıysa NOT_RUN; varsayımsal başarı kaydı yok.

## Riskler ve sınırlar

- Gerçek standart protokole uyumluluk iddia edilmez; örnek format açıkça özgündür

- Referanslar: cocotb/cocotb; HypothesisWorks/hypothesis

- Katkı repo kodu ve geliştirmeleri yalnız kendi upstream kapsamlarında kalır.

- Repo oluşturma, dosya yazma, fork, push ve PR ayrı onay gerektirir.
