# Portfolyo Çıktı Rapor Paketi — 06.11.2026

8 bağımsız ürün adayı; 5 standart rapor + 8 tek-modül Sprint-1 raporu.

## Politikalar
- Katkı Yaptığımız Repolar üzerindeki geliştirmeler yalnızca ilgili upstream reponun kapsamında geçerlidir.
- Sekiz ürün bağımsız, henüz oluşturulmamış repo adaylarıdır. Kaynak kod kopyalama lisans ve ayrıca onay gerektirir.
- Mevcut standart çalışma alanının klasör yapısı değiştirilmez; bu belgelerdeki dosya yolları yeni repo için öneridir.
- GitHub üzerinde repo oluşturma, fork, push, PR, klon veya kod değişikliği için ayrıca izin alınır.
- Bu belgelerde hiçbir testin çalıştırıldığı iddia edilmez; başlangıç durumu NOT_RUN.

## Sprint-1 kapsamları
- `ecad-quality-gate` — CLI Exit-Code Classification | `src/ecad_gate/runner.py` → `outputs/cli_classification.json`
- `i2c-register-validation-bench` — Register Read/Write Model | `src/i2c_bench/register_map.py` → `outputs/register_transactions.json`
- `motor-control-verification` — PWM Duty-Cycle Verification | `src/pwm/generator.py` → `outputs/pwm_trace.csv`
- `numerical-dynamics-validation` — Damped Oscillator Euler/RK4 | `src/numerical_validation/models/damped_oscillator.py` → `outputs/error_vs_timestep.csv`
- `engineering-evidence-kit` — Evidence Schema & Integrity | `src/evidence_kit/validator.py` → `outputs/validation_summary.json`
- `analog-audio-afe-reference` — AC Gain Reference Simulation | `simulation/ac_analysis.cir` → `outputs/frequency_response.csv`
- `sensor-estimation-pipeline` — Synthetic Sensor Calibration | `src/sensor_pipeline/calibration.py` → `outputs/calibration_metrics.json`
- `embedded-protocol-conformance` — Frame Codec Golden Vectors | `src/protocol_conformance/frame_parser.py` → `outputs/vector_results.json`

## Durum
Tüm Sprint-1 raporları planlama aşamasındadır: NOT_RUN. Hiçbir repo oluşturulmadı, kod yazılmadı veya test yürütülmedi.

## İçerik

### Standart raporlar

- [01 Codex Handoff](01_Codex_Handoff.md)
- [02 İş Akışı](02_Is_Akisi.md)
- [03 Görev Planlama](03_Gorev_Planlama.md)
- [04 Claude Araştırma](04_Claude_Arastirma.md)
- [05 Doğrulama Matrisi](05_Dogrulama_Matrisi.md)

### Sprint 1 raporları

- [01 ecad quality gate](Sprint_1_01_ecad-quality-gate.md)
- [02 i2c register validation bench](Sprint_1_02_i2c-register-validation-bench.md)
- [03 motor control verification](Sprint_1_03_motor-control-verification.md)
- [04 numerical dynamics validation](Sprint_1_04_numerical-dynamics-validation.md)
- [05 engineering evidence kit](Sprint_1_05_engineering-evidence-kit.md)
- [06 analog audio afe reference](Sprint_1_06_analog-audio-afe-reference.md)
- [07 sensor estimation pipeline](Sprint_1_07_sensor-estimation-pipeline.md)
- [08 embedded protocol conformance](Sprint_1_08_embedded-protocol-conformance.md)

Özgün DOCX dosyaları aynı klasörde korunmuştur.
