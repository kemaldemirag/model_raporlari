# 03 — Görev Planlama | Portfolyo Ürün Çıkarımı

_Sprint-1 her ürün için yalnız bir alt modül_

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

## Sprint iş paketleri

**ecad-quality-gate:** S1-01 kaynak/lisans; S1-02 CLI Exit-Code Classification sözleşmesi; S1-03 fixture; S1-04 tests/test_exit_codes.py negatif/pozitif test; S1-05 outputs/cli_classification.json ve kanıt raporu

**i2c-register-validation-bench:** S1-01 kaynak/lisans; S1-02 Register Read/Write Model sözleşmesi; S1-03 fixture; S1-04 tests/test_register_read_write.py negatif/pozitif test; S1-05 outputs/register_transactions.json ve kanıt raporu

**motor-control-verification:** S1-01 kaynak/lisans; S1-02 PWM Duty-Cycle Verification sözleşmesi; S1-03 fixture; S1-04 tests/test_pwm.py negatif/pozitif test; S1-05 outputs/pwm_trace.csv ve kanıt raporu

**numerical-dynamics-validation:** S1-01 kaynak/lisans; S1-02 Damped Oscillator Euler/RK4 sözleşmesi; S1-03 fixture; S1-04 tests/test_solver_accuracy.py negatif/pozitif test; S1-05 outputs/error_vs_timestep.csv ve kanıt raporu

**engineering-evidence-kit:** S1-01 kaynak/lisans; S1-02 Evidence Schema & Integrity sözleşmesi; S1-03 fixture; S1-04 tests/test_hash_integrity.py negatif/pozitif test; S1-05 outputs/validation_summary.json ve kanıt raporu

**analog-audio-afe-reference:** S1-01 kaynak/lisans; S1-02 AC Gain Reference Simulation sözleşmesi; S1-03 fixture; S1-04 tests/test_gain.py negatif/pozitif test; S1-05 outputs/frequency_response.csv ve kanıt raporu

**sensor-estimation-pipeline:** S1-01 kaynak/lisans; S1-02 Synthetic Sensor Calibration sözleşmesi; S1-03 fixture; S1-04 tests/test_calibration.py negatif/pozitif test; S1-05 outputs/calibration_metrics.json ve kanıt raporu

**embedded-protocol-conformance:** S1-01 kaynak/lisans; S1-02 Frame Codec Golden Vectors sözleşmesi; S1-03 fixture; S1-04 tests/test_encoding.py negatif/pozitif test; S1-05 outputs/vector_results.json ve kanıt raporu

## Bağımlılık ve sıralama

- Öncelik: numerical-dynamics-validation ve engineering-evidence-kit; sonra ECAD ve I²C; ardından kontrol/sensör ve uygulama örnekleri.

- Bu sıralama öneridir; bağımsız geliştirmeye engel oluşturan zorunlu bağımlılık yoktur.

- Her sprint için kullanıcıdan geliştirme ve yazma onayı ayrı alınır.
