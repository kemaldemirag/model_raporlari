# Ürün 1 K1 Fixture ve Kabul Matrisi

_21 Kasım 2026 kapsamı. Durum ve kanıt alanları yürütme sırasında doldurulur._

## K1 özeti

| Ölçüt | Güncel değer | Kaynak formül |
| --- | ---: | --- |
| Toplam test | 12 | `=COUNTA(B11:B22)` |
| Geçti | 0 | `=COUNTIFS(J11:J22,"Geçti")` |
| Kaldı | 0 | `=COUNTIFS(J11:J22,"Kaldı")` |
| Engelli | 0 | `=COUNTIFS(J11:J22,"Engelli")` |
| Tamamlanma | 0% | `=IF(C5=0,0,(F5+I5+L5)/C5)` |
| K1 kararı | Açık | `=IF(I5>0,"Kaldı",IF(L5>0,"Engelli",IF(F5=C5,"Geçti","Açık")))` |

Durum alanlarında kullanılabilen değerler: `Planlandı`, `Çalışıyor`, `Geçti`, `Kaldı`, `Engelli`.

## Test matrisi

| Test kimliği | Sınıf | Fixture veya senaryo | Enjekte edilen durum | Beklenen karar | Beklenen kontrol | Zorunlu kanıt | Kabul ölçütü | Durum | Gerçek sonuç | Kanıt yolu | İnceleyen | Tarih |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| K1-00 | Temiz kontrol | Geçerli KiCad 10 kartı | Yok | PASS | ALL_REQUIRED | ERC DRC çıktı listesi manifest ve kaynak hashleri | Blocker yok zorunlu çıktı tam kaynak değişmedi | Planlandı |  |  |  |  |
| K1-01 | Kusur | Temiz kartın açıklık ihlalli kopyası | Bakır öğeler minimum açıklığın altında | BLOCK | PCB_DRC_CLEARANCE | DRC JSON katman koordinat ve HTML kaydı | Kusur doğru kart ve konumla raporlanır | Planlandı |  |  |  |  |
| K1-02 | Kusur | Temiz kartın ERC hatalı kopyası | Zorunlu güç pini veya uyumsuz pin tipi | BLOCK | SCH_ERC_REQUIRED | ERC JSON sayfa sembol ve HTML kaydı | Kusur doğru şematik konumuyla raporlanır | Planlandı |  |  |  |  |
| K1-03 | Kusur | Temiz kartın açık sınır içeren kopyası | Edge Cuts çevrimi kapalı değil veya belirsiz | BLOCK | PCB_OUTLINE_CLOSED | Geometri sonucu ve ilgili segmentler | Açıklık veya belirsiz çevrim raporlanır | Planlandı |  |  |  |  |
| K1-04 | Kusur | Zorunlu çıktıyı üretmeyen profil | Gerber veya NPTH drill eksik ya da sıfır bayt | BLOCK | FAB_OUTPUT_REQUIRED | Beklenen ve gerçekleşen çıktı manifesti | Eksik dosya ve profil gerekliliği gösterilir | Planlandı |  |  |  |  |
| K1-05 | Kusur | BOM ve CPL kümeleri uyuşmayan kopya | CPL fazlası veya yerleştirilecek BOM referansı eksik | BLOCK | BOM_CPL_REFERENCE_PARITY | BOM CPL kümeleri ve küme farkı | Uyuşmayan reference açıkça listelenir | Planlandı |  |  |  |  |
| K1-06 | Kusur | DNP sızıntısı içeren kopya | Takılmaması gereken bileşen CPL içinde | BLOCK | VARIANT_DNP_PLACEMENT | DNP kaydı CPL satırı ve varyant | Sızan reference ve varyant raporlanır | Planlandı |  |  |  |  |
| K1-07 | Bütünlük | Temiz fixture | Koşu öncesi sonrası kaynak karşılaştırması | PASS | SOURCE_IMMUTABILITY | İki SHA 256 listesi ve diff | Yeni değişen veya silinen kaynak yok | Planlandı |  |  |  |  |
| K1-08 | Tekrar | Temiz fixture iki yeni çalışma dizini | İki bağımsız koşu | PASS | NORMALIZED_REPRODUCIBILITY | İki manifest checksum listesi ve normalize diff | Değişken alanlar hariç sonuçlar özdeş | Planlandı |  |  |  |  |
| K1-09 | Yeniden çalışma | Temiz ortam ve boş cache | Önceden oluşmuş çıktı yok | PASS | ONE_COMMAND_REPRODUCE | Komut stdout stderr exit code ve manifest | Tek komut tam paketi aynı kararla üretir | Planlandı |  |  |  |  |
| K1-10 | Kanıt | Tüm fixture koşuları | Paket şema doğrulaması | PASS | EVIDENCE_PACKAGE_SCHEMA | Dosya listesi ve JSON şema sonucu | Her blocker ham kanıta ve HTML kaydına izlenir | Planlandı |  |  |  |  |
| K1-11 | İnceleme | Tamamlanmış kanıt paketi | Bağımsız karar incelemesi | PASS | INDEPENDENT_REVIEW | İnceleme kaydı ve imzalı karar | Tüm zorunlu ölçütler doğrulanmadan geçmez | Planlandı |  |  |  |  |

## Kaynaklar

- [KiCad 10 CLI](https://docs.kicad.org/10.0/en/cli/cli.html)
- [KiBot](https://github.com/INTI-CMNB/KiBot)
- [CopperForge](https://docs.copperforge.dev/)
- [Güncel araştırma sentezi](https://docs.google.com/document/d/1dcz3_32Uf0tsr9-vKUdqMf60GC-qXrS3NpROj8Uhu4s/edit)
- [AnyShake Explorer holdout](https://github.com/anyshake/explorer/tree/dde90ce53811f090137d9a962bde6d7408477aef)

## Ürün 1 kanıt paketi listesi

Manifestte bulunmayan dosya karar kanıtı sayılmaz.

| Kanıt kimliği | Dosya veya klasör | Amaç | Zorunlu | Bağlı test | Beklenen SHA | Gerçek SHA | Durum | Konum | İnceleyen |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| EV-01 | `review.html` | İnsan tarafından okunabilir kapsam bulgu ve karar | Evet | K1-00..11 |  |  | Planlandı |  |  |
| EV-02 | `findings.json` | Kararlı bulgu kimliği ve makine çıktısı | Evet | K1-01..06 |  |  | Planlandı |  |  |
| EV-03 | `findings.csv` | İnceleme ve dışa aktarım görünümü | Evet | K1-01..06 |  |  | Planlandı |  |  |
| EV-04 | `evidence-manifest.json` | Kaynak ortam komut senaryo artefakt ve karar bağı | Evet | K1-00..11 |  |  | Planlandı |  |  |
| EV-05 | `checksums.sha256` | Teslim paketinin SHA 256 listesi | Evet | K1-07..10 |  |  | Planlandı |  |  |
| EV-06 | `logs` | stdout stderr exit code ERC DRC ve araç günlükleri | Evet | K1-00..10 |  |  | Planlandı |  |  |
| EV-07 | `raw` | Ham makine çıktıları | Evet | K1-00..10 |  |  | Planlandı |  |  |
| EV-08 | `outputs` | Gerber drill BOM CPL ve seçilen ek çıktılar | Evet | K1-00..10 |  |  | Planlandı |  |  |
| EV-09 | `reproduce.md` | Tek komut ön koşullar ve beklenen sonuç | Evet | K1-09 |  |  | Planlandı |  |  |
| EV-10 | `waivers.yml` | Bulgu kimliği gerekçe sahibi onay ve son tarih | Varsa | K1-11 |  |  | Planlandı |  |  |
| EV-11 | `independent-review.md` | Bağımsız inceleme ve K1 kararı | Evet | K1-11 |  |  | Planlandı |  |  |
