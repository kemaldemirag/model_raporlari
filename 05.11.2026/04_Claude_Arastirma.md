# 04 — Claude Araştırma | Portfolyo Ürün Çıkarımı

_Karşılaştırmalı literatür ve kaynak kod inceleme brifi_

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

## Araştırma soruları

**ecad-quality-gate:** KiCad CLI ERC/DRC süreç çıkış kodlarını, bayrakları ve rapor varlığını birlikte sınıflandırmak; araştırılacak referanslar: INTI-CMNB/KiBot; productize/kicad-automation-scripts

**i2c-register-validation-bench:** Sentetik register haritasında reset, okuma, yazma, read-only ve geçersiz adres sözleşmesini test etmek; araştırılacak referanslar: cocotb/cocotb; alexforencich/cocotbext-i2c

**motor-control-verification:** Sentetik PWM üreticisinin görev çevrimi, periyot ve sınır girişlerini deterministik sanal saatle doğrulamak; araştırılacak referanslar: python-control/python-control

**numerical-dynamics-validation:** Sönümlü ikinci dereceden sistem için Euler ve RK4 çözümlerini bağımsız analitik çözümle karşılaştırmak; araştırılacak referanslar: scipy/scipy; python-control/python-control

**engineering-evidence-kit:** Test kanıtının JSON Schema, artefakt varlığı ve SHA-256 bütünlüğünü doğrulamak; araştırılacak referanslar: HypothesisWorks/hypothesis

**analog-audio-afe-reference:** Basit ve özgün bir analog ön uç devresinin AC kazanç ve bant genişliğini SPICE ile ölçmek; araştırılacak referanslar: PySpice-org/PySpice; ngspice

**sensor-estimation-pipeline:** Sabit tohumlu sentetik sensör verisinde ofset ve ölçek kalibrasyonu gerçekleştirmek; araştırılacak referanslar: scipy/scipy; python-control/python-control

**embedded-protocol-conformance:** Özgün ve belgeli örnek çerçeve formatında encode/decode tutarlılığını ve negatif girdileri sınamak; araştırılacak referanslar: cocotb/cocotb; HypothesisWorks/hypothesis

## Doğrulama beklentileri

- Her referans için URL, varsayılan dal, tam commit, lisans, gerçek kaynak ve test dosyası bağlantıları.

- İlgili algoritmanın teknik oracle kaynağı: resmi dokümantasyon, veri sayfası, standart veya analitik çözüm.

- Mevcut açık kaynak çözümlerle farkı belirt; KiBot gibi olgun ürünlerin zaten yaptığı işleri yeniden icat etme.

- Kaynak bulgusu ile hipotezi ayrı yaz; issue varlığını doğrulanmış bug olarak sunma.
