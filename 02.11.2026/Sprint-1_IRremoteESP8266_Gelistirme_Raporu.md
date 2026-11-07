# SPRINT-1 | Model-Based Protocol Testing

_03.11.2026  •  IRremoteESP8266  •  Araştırma ve uygulama planı_

## Yönetici özeti

Amaç: IRremoteESP8266 içindeki mevcut IRac testlerinin kapsamadığı davranışları kanıtlayıp en küçük, tekrarlanabilir test katkısını hazırlamak. Bu rapor plan ve kaynak incelemesidir; testlerin çalıştırıldığı veya yeni bir kusurun kanıtlandığı iddia edilmez.

## Kapsam ve sabitleme

Repository: https://github.com/crankyoldgit/IRremoteESP8266<br>
İncelenen commit: 1e2f0f3ef0a93cbf2a8ddb2e95130f8f4c584b3f<br>
Öncelikli dosyalar: src/IRac.cpp, src/IRac.h, test/IRac_test.cpp, test/ir_Hitachi_test.cpp, test/ir_Bosch_test.cpp, test/Makefile, ReleaseNotes.md.<br>
GST ve Omega3 yalnızca metodolojik karşılaştırma kaynağıdır. Mevcut çalışma alanına eklenmeyecek.

## Doğrulanan davranış sözleşmesi

IRac::handleToggles(desired, prev), prev mevcutsa ve protocol/model eşleşiyorsa belirli protokollerin toggle alanlarını dönüştürür; aksi halde desired döndürülür. IRAcUtils::decodeToState() ayrı bir isim alanı fonksiyonudur. test/IRac_test.cpp içinde handleToggles testi zaten vardır; yeni katkı mevcut assertion kapsamıyla karşılaştırılmalıdır.

| Protokol | Boolean XOR alanları | Ek semantik |
| --- | --- | --- |
| DAIKIN128 | power, light | Önceki protokol/model eşleşmesi |
| ELECTRA_AC | light | Aynı guard |
| FUJITSU_AC | turbo, econo | Aynı guard |
| MIDEA | turbo, econo, light, clean | swingv: Off/On sınıflaması; fall-through |

## Sprint iş paketleri

### S1-00 — Baseline ve test envanteri [P0]

Sabit commit ve temiz çalışma ağacı doğrulanır. Mevcut test komutu Makefile üzerinden belirlenir. Önce baseline koşulur; log, ortam ve exit code kaydedilir. Test çalıştırılamazsa BLOCKED olarak raporlanır.

### S1-01 — Assertion düzeyi kapsam matrisi [P0]

test/IRac_test.cpp ve protokol testleri okunur. Her assertion protokol, alan, önceki durum, beklenen komut ve guard durumuyla eşleştirilir. Mevcut testi tekrarlayan adaylar elenir.

### S1-02 — Toggle geçiş testleri [P0]

Dört protokol için desteklenen XOR alanlarında 00, 01, 10, 11 kombinasyonları; null prev, farklı protocol ve farklı model guard senaryoları. Önce doğru test oracle doğrulanır.

### S1-03 — MIDEA swingv [P1]

Off→Off, Off→On, On→Off, On→On geçişleri; On enum varyantları kAuto/kHigh gibi desteklenme koşullarına göre sınanır. Boolean XOR testiyle karıştırılmaz.

### S1-04 — Decode önceki durum [P1]

IRAcUtils::decodeToState() için mevcut Bosch ve diğer protokol testleri karşılaştırılır; önceki durumun geçerli kullanımına yönelik negatif/pozitif test adayları belirlenir.

### S1-05 — MC/DC ve mutation [P2]

prev != NULL && protocol match && model match kararının bağımsız etkisi için test çiftleri; yalnızca izole kopyada geçici XOR/guard mutasyonları. Eşdeğer mutantlar ayrı işaretlenir.

### S1-06 — Kanıt ve upstream hazırlığı [P0]

Değişiklik diff’i, test logları, commit SHA, test sayısı, başarısızlık ve sınırlamalar raporlanır. PR gönderilmez; yalnızca onay için öneri hazırlanır.

## Akademik dayanak ve kaynaklar

- Tretmans (2008), Model Based Testing with Labelled Transition Systems — https://doi.org/10.1007/978-3-540-78917-8_1

- Benveniste et al. (2018), Contracts for System Design — https://doi.org/10.1145/3182014

- Segura et al. (2016), A Survey on Metamorphic Testing — https://doi.org/10.1109/TSE.2016.2532875

- Jia & Harman (2011), Mutation Testing Survey — https://doi.org/10.1109/TSE.2010.62

- NASA, A Practical Tutorial on MC/DC — https://ntrs.nasa.gov/citations/20010057789

- IRac.cpp — https://github.com/crankyoldgit/IRremoteESP8266/blob/1e2f0f3ef0a93cbf2a8ddb2e95130f8f4c584b3f/src/IRac.cpp

- IRac_test.cpp — https://github.com/crankyoldgit/IRremoteESP8266/blob/1e2f0f3ef0a93cbf2a8ddb2e95130f8f4c584b3f/test/IRac_test.cpp

## Kabul kriterleri / kanıt şeması

Her test için: test_id, repo_commit, source_path, test_path, protocol, field, prev_state, desired_state, expected_output, actual_output, command, environment, exit_code, status (PASS/FAIL/BLOCKED/NOT_RUN), log_path. Sprint kabulü: baseline durumu belgeli; mevcut assertion matrisi tamam; eklenen testler gerçekten derlenip koşulmuş; tüm farklar raporlanmış. PASS yalnızca yürütme kanıtıyla yazılır.

## Riskler ve sınırlar

Toggle işlemlerinin tüm protokollerde tersinir olduğu varsayılamaz. ioco uygunluk iddiası için tam giriş/çıkış ve sessizlik modeli gerekir; bu sprint yalnızca MBT esinli geçiş testlerini hedefler. Test artışı tek başına upstream değeri kanıtlamaz. Donanım olmadan fiziksel klima davranışı doğrulanmış sayılamaz.

## Yetki sınırları

Yeni GitHub reposu, fork, push veya upstream PR yok. Çalışma alanının mevcut klasörleri korunur. Kod değişikliği ancak ayrı uygulama adımında ve kullanıcı onayıyla gerçekleştirilir.
