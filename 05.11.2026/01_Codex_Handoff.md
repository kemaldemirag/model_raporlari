# 01 — Codex Handoff | Portfolyo Ürün Çıkarımı

_8 bağımsız ürün • Geliştirme başlamadan önce teknik devir_

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

## Devir girdileri

- Her ürün için ayrı tam SHA, lisans, modül sözleşmesi, fixture ve test komutu kaydı tutulur.

- Sprint-1 sınırı tek modüldür; diğer modüller backlog olarak kalır.

- Kaynak repo kodları yeni ürünlere otomatik taşınmaz.

## Ürün bazlı handoff

**ecad-quality-gate:** Kapsam: CLI Exit-Code Classification; önerilen kaynak: src/ecad_gate/runner.py; test: tests/test_exit_codes.py; hedef: outputs/cli_classification.json

**i2c-register-validation-bench:** Kapsam: Register Read/Write Model; önerilen kaynak: src/i2c_bench/register_map.py; test: tests/test_register_read_write.py; hedef: outputs/register_transactions.json

**motor-control-verification:** Kapsam: PWM Duty-Cycle Verification; önerilen kaynak: src/pwm/generator.py; test: tests/test_pwm.py; hedef: outputs/pwm_trace.csv

**numerical-dynamics-validation:** Kapsam: Damped Oscillator Euler/RK4; önerilen kaynak: src/numerical_validation/models/damped_oscillator.py; test: tests/test_solver_accuracy.py; hedef: outputs/error_vs_timestep.csv

**engineering-evidence-kit:** Kapsam: Evidence Schema & Integrity; önerilen kaynak: src/evidence_kit/validator.py; test: tests/test_hash_integrity.py; hedef: outputs/validation_summary.json

**analog-audio-afe-reference:** Kapsam: AC Gain Reference Simulation; önerilen kaynak: simulation/ac_analysis.cir; test: tests/test_gain.py; hedef: outputs/frequency_response.csv

**sensor-estimation-pipeline:** Kapsam: Synthetic Sensor Calibration; önerilen kaynak: src/sensor_pipeline/calibration.py; test: tests/test_calibration.py; hedef: outputs/calibration_metrics.json

**embedded-protocol-conformance:** Kapsam: Frame Codec Golden Vectors; önerilen kaynak: src/protocol_conformance/frame_parser.py; test: tests/test_encoding.py; hedef: outputs/vector_results.json

## Codex GO/HOLD

- GO: Kaynaklar, lisans, bağımlılıklar, kabul sözleşmesi ve hedef yol doğrulandıktan sonra ayrı geliştirme onayı alınır.

- HOLD: API, fixture, bağımsız oracle veya araç sürümü belirsiz.

- NO-GO: Fiziksel güvenlik/sertifikasyon iddiası yalnız simülasyonla kuruluyor.
