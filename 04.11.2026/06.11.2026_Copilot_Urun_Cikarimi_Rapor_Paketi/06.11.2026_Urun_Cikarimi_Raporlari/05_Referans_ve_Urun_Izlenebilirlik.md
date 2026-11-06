# 06.11.2026 — Copilot Ürün Çıkarımı

**Kapsam:** Araştırma ve planlama. Bu raporların hazırlanması geliştirme, repo/fork/PR oluşturma, upstream değişiklik veya sprint yürütme onayı değildir.

**Kanıt sınıfları:** VERIFIED (kaynak üzerinden incelendi), UNVERIFIED (henüz doğrulanmadı), BLOCKED (bağımlılık/izin engeli), NOT_RUN (test çalıştırılmadı), PROPOSED (taslak).

**Önemli sınır:** Repo iç iskeletleri taslaktır; kullanıcının standart yerel çalışma alanı klasör yapısı doğrulanmadan yerleştirme yolu atanmaz. Kaynak kodu yeniden kullanımı için lisans incelemesi gerekir. GitHub statik incelemesi çalışma testi yerine geçmez.

## Ürün ve referans matrisi
| Ürün | Kaynak | Müşteri ihtiyacı hipotezi | İlk kanıt |
|---|---|---|---|
| PF-E | i2cdevlib / RIOT / ST datasheet | Register ve protokol uyumu | Mock I²C + test matrisi |
| PF-D | KiCad CLI / pytest / CTest | Tekrarlanabilir mühendislik doğrulaması | Manifest + CI artifact |
| PF-A | Jiroskop olay akışları | Güvenilir hareket alarmı | FSM + yanlış alarm testleri |
| IoT | Mosquitto / MQTT | Sensör veri kaydı ve replay | Deterministik replay raporu |

## Kanıt durumu
- i2cdevlib header/uygulama statik incelemesi: VERIFIED (okunan dosyalar bağlamında).
- Upstream test çalıştırma: NOT_RUN.
- Endian/BDU gerçek cihaz davranışı: UNVERIFIED.
- İş ilanlarının özgün şartları: UNVERIFIED.
- Lisans/yeniden dağıtım koşulları: ürün bazında tekrar doğrulanacak.

## Referans URL'leri
- https://github.com/biodynamical/i2cdevlib/tree/master/Arduino/L3G4200D
- https://github.com/jrowberg/i2cdevlib/tree/master/Arduino/L3G4200D
- https://github.com/RIOT-OS/RIOT/tree/master/drivers/l3gxxxx
- https://github.com/eclipse-mosquitto/mosquitto
- https://github.com/kemaldemirag/muh_cikti_arastirmalari/commit/df4174f49cb399120719baeb9b29e0bff0e92b18
