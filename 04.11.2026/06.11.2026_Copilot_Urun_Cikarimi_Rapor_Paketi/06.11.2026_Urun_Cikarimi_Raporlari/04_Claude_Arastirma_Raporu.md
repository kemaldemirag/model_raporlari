# 06.11.2026 — Copilot Ürün Çıkarımı

**Kapsam:** Araştırma ve planlama. Bu raporların hazırlanması geliştirme, repo/fork/PR oluşturma, upstream değişiklik veya sprint yürütme onayı değildir.

**Kanıt sınıfları:** VERIFIED (kaynak üzerinden incelendi), UNVERIFIED (henüz doğrulanmadı), BLOCKED (bağımlılık/izin engeli), NOT_RUN (test çalıştırılmadı), PROPOSED (taslak).

**Önemli sınır:** Repo iç iskeletleri taslaktır; kullanıcının standart yerel çalışma alanı klasör yapısı doğrulanmadan yerleştirme yolu atanmaz. Kaynak kodu yeniden kullanımı için lisans incelemesi gerekir. GitHub statik incelemesi çalışma testi yerine geçmez.

## Bağımsız araştırma soruları
1. L3G4200D BLE ve BDU bitleri hangi register ve hangi okuma düzenini gerektiriyor? Birden fazla I²C işlemi sırasında tutarlılık ne zaman korunur?
2. INT1_THS_XH/XL gibi alanlar ayrı high/low eşikleri mi, yoksa tek çok baytlı eşik değerinin parçaları mı? API adlandırması datasheet ile uyumlu mu?
3. `INT1_SRC` okumasının latch temizleme yan etkisi nedir? Olay kaydı nasıl tasarlanmalı?
4. `I2Cdev::readBytes` hangi I²C auto-increment semantiğini uygular? Platformlar arasında fark var mı?
5. PF-D KiCad CLI ERC/DRC JSON şemalarının sürüm farkları ve çıkış kodları nelerdir?
6. Gerçek ilanlarda firmware test, ECAD evidence ve MQTT replay için talep edilen teslimatlar hangileridir?

## Beklenen araştırma teslimi
Her iddia için birincil URL, commit veya doküman sürümü, satır/sayfa, karşı görüş, kanıt durumu, uygulanabilir test önerisi. Gri literatür bulguları üretici dokümanı yerine geçirilmez.
