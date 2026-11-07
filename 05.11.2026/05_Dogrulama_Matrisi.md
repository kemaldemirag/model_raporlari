# 05 — Doğrulama Matrisi | Portfolyo Ürün Çıkarımı

_Kabul ve kanıt matrisi • İlk durum NOT_RUN_

## Politika ve sınırlar

- Katkı Yaptığımız Repolar üzerindeki geliştirmeler yalnızca ilgili upstream reponun kapsamında geçerlidir.

- Sekiz ürün bağımsız, henüz oluşturulmamış repo adaylarıdır. Kaynak kod kopyalama lisans ve ayrıca onay gerektirir.

- Mevcut standart çalışma alanının klasör yapısı değiştirilmez; bu belgelerdeki dosya yolları yeni repo için öneridir.

- GitHub üzerinde repo oluşturma, fork, push, PR, klon veya kod değişikliği için ayrıca izin alınır.

- Bu belgelerde hiçbir testin çalıştırıldığı iddia edilmez; başlangıç durumu NOT_RUN.

## Kaynak ve kanıt düzeyi

- Ürün iskeletleri önceki sohbet araştırmasından türetilmiştir; önerilen yeni dosyalar henüz mevcut değildir.

- Referans repolar: GitHub üzerinde tespit edildi; her ürün için lisans, commit, dosya ve test kapsamı yeniden doğrulanmalıdır.

- Durumlar: VERIFIED (kaynak incelendi), INFERRED (çıkarım), UNVERIFIED (henüz doğrulanmadı), NOT_RUN (test yürütülmedi), BLOCKED (bağımlılık/izin).

## Ürün bazlı kabul matrisi

**ecad-quality-gate:** Test: tests/test_exit_codes.py; girdi: fixtures/cli_cases/; hedef artefakt: outputs/cli_classification.json; kabul senaryoları: KiCad sürüm/komut/flag matrisi; exit=0 fakat ihlal içeren rapor; exit=5; eksik binary; timeout; exit=0 fakat rapor yok; durum: NOT_RUN

**i2c-register-validation-bench:** Test: tests/test_register_read_write.py; girdi: fixtures/register_maps/; hedef artefakt: outputs/register_transactions.json; kabul senaryoları: Reset varsayılanları; RW/RO davranışı; bilinmeyen adres; art arda yazma; oracle eşleşmesi; durum: NOT_RUN

**motor-control-verification:** Test: tests/test_pwm.py; girdi: fixtures/pwm_cases/; hedef artefakt: outputs/pwm_trace.csv; kabul senaryoları: 0%, 50%, 100%; negatif ve >100% girdiler; periyot sınırları; yükselen/düşen kenar olayları; durum: NOT_RUN

**numerical-dynamics-validation:** Test: tests/test_solver_accuracy.py; girdi: fixtures/oscillator_cases/; hedef artefakt: outputs/error_vs_timestep.csv; kabul senaryoları: Sabit parametreler; h ve h/2; RMS ve maksimum hata; yakınsama eğimi; kritik/aşırı sönüm kapsam dışı; durum: NOT_RUN

**engineering-evidence-kit:** Test: tests/test_hash_integrity.py; girdi: fixtures/evidence_cases/; hedef artefakt: outputs/validation_summary.json; kabul senaryoları: Geçerli paket; bozuk JSON; eksik artefakt; hash uyuşmazlığı; beklenmeyen alan; deterministik karar; durum: NOT_RUN

**analog-audio-afe-reference:** Test: tests/test_gain.py; girdi: simulation/models/; hedef artefakt: outputs/frequency_response.csv; kabul senaryoları: Sabit netlist; AC sweep; kazanç referansı; -3 dB frekansı; simülatör sürümü ve model checksum; durum: NOT_RUN

**sensor-estimation-pipeline:** Test: tests/test_calibration.py; girdi: fixtures/synthetic_sensor/; hedef artefakt: outputs/calibration_metrics.json; kabul senaryoları: Bilinen gerçek parametreler; train/test ayrımı; bias ve RMSE; tekrarlanabilir seed; sıfır varyans sınırı; durum: NOT_RUN

**embedded-protocol-conformance:** Test: tests/test_encoding.py; girdi: protocols/example_protocol/golden_vectors.json; hedef artefakt: outputs/vector_results.json; kabul senaryoları: Golden vector; CRC/checksum seçimi; truncated frame; yanlış uzunluk; round-trip; yanlış checksum; durum: NOT_RUN

## Genel kapanış ölçütleri

- Bağımsız oracle ile expected/actual ayrımı.

- En az bir negatif test, bir sınır testi ve deterministik tekrar koşusu.

- Kaynak commit, komut, sürüm, ortam, log ve SHA-256 manifesti.

- Çalıştırılmayan test için PASS yazılmaz; donanım gerektiren iddialar açıkça sınırlandırılır.
