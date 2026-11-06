# Sensor Telemetry & Replay — Sprint-4

**Durum:** PLAN / NOT_RUN. **Yürütme yetkisi:** YOK; kullanıcıdan ayrı onay gerekli.

## Amaç
Metrik raporu, dashboard taslağı ve kanıt paketi.

## Girdiler ve bağımlılıklar
- Önceki sprintin kanıt ve kabul kriterleri tamamlanmış olmalı.
- Kaynak: Eclipse Mosquitto, MQTT, Python, JSON Schema.
- Ürün sözleşmesi: Olay zaman damgası; sıra numarası; kayıt/replay; kayıp/gecikme.

## Görevler
1. Kaynak/versiyon ve varsayımları envantere kaydet.
2. “Metrik raporu, dashboard taslağı ve kanıt paketi” için beklenen davranışı test edilebilir alt gereksinimlere ayır.
3. Pozitif, negatif ve sınır değer test vektörlerini tanımla.
4. Başarı/başarısızlık ve BLOCKED/NOT_RUN kanıtlarını raporla.
5. Lisans, donanım ve entegrasyon bağımlılıklarını güncelle.

## Kabul kriterleri
- Her gereksinimin bir kaynak dayanağı ve en az bir test orakılı bulunur.
- Her testin beklenen sonucu önceden tanımlanır; çalıştırılmadıysa NOT_RUN kalır.
- Kaynak commit ve araç sürümleri manifestte kayıtlıdır.
- Gerçek cihaz performansı, yalnız sentetik veriye dayanarak doğrulanmış sayılmaz.

## Öngörülen teslimatlar (henüz üretilmedi)
- `requirements-traceability.md`
- `test-vectors.json`
- `verification-report.md`
- `evidence-manifest.json`

## Riskler / blokajlar
- Referans sürüm değişebilir; SHA sabitlenmeli.
- Test orakılı ve lisans koşulları doğrulanmalı.
- Fiziksel doğrulama gerektiren sonuçlar donanımsız ortamda UNVERIFIED kalır.

## Codex devir notu
Bu belge uygulanabilir sprint planıdır, başlatma komutu değildir. Kullanıcı açık onay verene kadar yalnız salt-okunur araştırma yapılabilir.
