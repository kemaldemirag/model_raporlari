# GÖREV PLANLAMA — IRremoteESP8266

_Tarih: 02.11.2026 | Çalışma alanı: Katkı Yaptığım Repolar | Durum: Plan, henüz uygulanmadı_

## T0 — Kritik

Standart çalışma alanının gerçek yolunu doğrula; varsa repoyu koru; SHA, branch, git status ve test bağımlılıklarını kaydet. Çıktı: baseline raporu.

## T1 — Kritik / GO

handleToggles() için dört protokolün mevcut/dolaylı test kapsamını karşılaştır. Yalnız kanıtlı boşlukları test/IRac_test.cpp içine ekle. Kabul: deterministik testler, açık kaynak satır kanıtı, baseline bozulmaz.

## T2 — İkincil / NEEDS-EVIDENCE

IRAcUtils::decodeToState(prev) kapsamını protokol bazında kontrol et. Kanıt olmadan kod değişikliği yok.

## T3 — İkincil / NEEDS-EVIDENCE

IRsend::sendGeneric() üç overload ve header/footer/gap/MSBfirst/repeat kombinasyonlarının mevcut test kapsamını belgeleyip eksikse katkı önerisi hazırla; T1 bitmeden uygulama zorunlu değil.

## NO-GO

MWM #2294/#2289 ve Daikin200 #2299 için mevcut PR çakışmaları nedeniyle tekrar düzeltme yazma. SANYO_AC152 ve RMT geçişi bu sprintte geliştirilmez.

## Tamamlanma

T1 kabul: test kodu + başarılı test çıktısı + diff + yerel commit (varsa). T2/T3 kabul: kaynak/test satırlarıyla kanıt matrisi; otomatik ek kod üretimi değil.

## Yetki

Yerel geliştirme onay kapsamındadır; push/PR, fork, yeni bağımsız GitHub repo yok. Beklenmeyen kaynak kod değişikliği gerekirse kullanıcıya sor.

Belge statüsü: Revize görev tanımı; geliştirme/test tamamlanma kanıtı değildir.
