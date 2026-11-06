# Register & Protocol Validation — Sprint-4

**Durum:** PLAN / NOT_RUN. **Yürütme yetkisi:** YOK; kullanıcıdan ayrı onay gerekli.

## Amaç
Üç sürücü için uyumluluk raporu ve PF-D kanıt adaptörü.

## Girdiler ve bağımlılıklar
- Önceki sprintin kanıt ve kabul kriterleri tamamlanmış olmalı.
- Kaynak: biodynamical/i2cdevlib, jrowberg/i2cdevlib, RIOT-OS/RIOT.
- Ürün sözleşmesi: L3G4200D register haritası; INT1; BLE/BDU; I2C okuma.

## Görevler
1. Kaynak/versiyon ve varsayımları envantere kaydet.
2. “Üç sürücü için uyumluluk raporu ve PF-D kanıt adaptörü” için beklenen davranışı test edilebilir alt gereksinimlere ayır.
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
