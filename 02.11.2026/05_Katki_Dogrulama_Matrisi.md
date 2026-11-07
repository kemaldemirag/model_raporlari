# KATKI VE DOĞRULAMA MATRİSİ

_Tarih: 02.11.2026 | Çalışma alanı: Katkı Yaptığım Repolar | Durum: Plan, henüz uygulanmadı_

## Araştırma sabiti

IRremoteESP8266 SHA 1e2f0f3ef0a93cbf2a8ddb2e95130f8f4c584b3f. Copilot raporları tarihsel bulgulardır; Codex güncel çalışma ağacını doğrulamalıdır.

## GO

handleToggles(): DAIKIN128, ELECTRA_AC, FUJITSU_AC, MIDEA. İlk katkı test-only. Dosya: test/IRac_test.cpp. Ön koşul: gerçek test boşluğu ve baseline.

## NEEDS-EVIDENCE

decodeToState(prev) protokol kapsamı; sendGeneric() overload kapsamı; SANYO_AC152 #2292 gerçek yakalama verisi.

## NO-GO

MWM #2294/#2289 (PR #2295/#2293), Daikin200 #2299 (PR #2300), RMT geçişi #2249 (ayrı dal ve CI riskleri).

## Kanıt paketi

git status; commit SHA; test komutları ve stdout özeti; diff; kaynak satır bağlantıları; varsa CI sonucu. Upstream kabulü ayrıca kanıtlanır.

## Başvuru şartı

Yalnız başvuru kararı verilirse Proje Dokümantasyonu/Irremoteesp8266. Yerel katkı, açık PR ve merged PR statüleri ayrı yazılır.

## Kapsam dışı

HVAC Controller sıfırdan geliştirme, Projelerim, MBÇ, Portfolyo sprinti, yeni GitHub deposu.

Belge statüsü: Revize görev tanımı; geliştirme/test tamamlanma kanıtı değildir.
