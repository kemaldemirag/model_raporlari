# CODEX HANDOFF — IRremoteESP8266 Katkısı

_Tarih: 02.11.2026 | Çalışma alanı: Katkı Yaptığım Repolar | Durum: Plan, henüz uygulanmadı_

## Amaç

Yalnızca mevcut upstream repoya yerel test katkısı geliştir. Yeni proje, bağımsız GitHub deposu, fork veya başka çalışma kolu açma.

## Sabit kaynak

crankyoldgit/IRremoteESP8266; araştırma SHA: 1e2f0f3ef0a93cbf2a8ddb2e95130f8f4c584b3f. Çalışma öncesinde SHA ve git durumunu doğrula; kaynak güncellenmişse kullanıcı onayı olmadan baseline değiştirme.

## Çalışma alanı

Önce mevcut standart tarihli çalışma alanını ve yapısını keşfet. Yalnızca doğrulanmış yapıda 02.11.2026/Çıktı Repoları/irremoteesp8266 kullan. Yol mevcut değilse üst dizin varsayma; kullanıcıdan gerçek kök yolu iste. Başka repo oluşturma.

## T1: Toggle testleri

test/IRac_test.cpp içinde DAIKIN128 (power/light), ELECTRA_AC (light), FUJITSU_AC (turbo/econo), MIDEA (turbo/econo/light/clean) davranışlarını kaynakta doğrula. Önce mevcut testleri ve dolaylı kapsamı incele; yalnız eksik ve anlamlı testleri ekle. Önceki durum yok/same/different, protokol-model uyumsuzluğu senaryolarını uygun olduğu ölçüde kapsa.

## Sınırlar

Öncelikle test-only diff. src/ değişiklikleri ancak gerçek hata kanıtıyla ve ayrıca onayla. PR #2293, #2295, #2300, #2249 üzerinde geliştirme yok; çakışan katkıları tekrar üretme.

## Test ve kanıt

Önce test/Makefile ve bağımlılıkları oku. Mümkünse cd test && make IRac_test && ./IRac_test. Kurulum gerekiyorsa raporla. Baseline ve değişiklik sonrası sonuçları ayrı kaydet. Test yapılmadıysa PASS yazma.

## Git sınırları

Yerel git status, diff ve commit hazırlanabilir; mevcut değişikliklere dokunma. Yeni remote, fork, push ve upstream PR yok. PR ayrı kullanıcı onayı gerektirir.

## Teslim

Gerçek repo yolu; git status; başlangıç ve son test sonuçları; değiştirilen dosyalar; diff; yerel commit SHA (varsa); açık engeller; PR hazırlık uygunluğu. Başvuru kararı varsa Proje Dokümantasyonu/Irremoteesp8266 oluşturulması ayrıca değerlendirilecek.

Belge statüsü: Revize görev tanımı; geliştirme/test tamamlanma kanıtı değildir.
