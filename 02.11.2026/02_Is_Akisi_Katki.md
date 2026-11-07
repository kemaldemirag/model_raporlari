# İŞ AKIŞI — Yalnızca Repo Katkısı

_Tarih: 02.11.2026 | Çalışma alanı: Katkı Yaptığım Repolar | Durum: Plan, henüz uygulanmadı_

## İş akışı

İlan/kod ifadesi → mevcut upstream repo → issue/PR ve test boşluğu doğrulama → yerel katkı → test/kanıt → isteğe bağlı başvuru dokümantasyonu → ayrıca onaylanırsa upstream PR.

## Ayrım

Katkı Yaptığım Repolar ve Fork Ettiğim Repolar farklıdır. Bu sprintte fork açılmaz. Projelerim, Model Bağımsız Çalışmaları ve Portfolyo için geliştirme başlatılmaz.

## Klasör kuralı

Tarih başlıklı çalışma alanında Çıktı Repoları/irremoteesp8266 (küçük harf). Yalnız katkı üzerinden başvuru planlanırsa Proje Dokümantasyonu/Irremoteesp8266 (ilk harf büyük). Standart yapının diğer dizinleri değiştirilmez.

## Ajan rolü

Copilot: araştırma kanıtları ve mevcut PR çakışmaları. Codex: yerel test geliştirme ve doğrulama. ChatGPT: sonuçları kontrol etme ve başvuru dokümantasyonu; hiçbir ajan onaysız upstream PR göndermez.

## Kontrol kapıları

1 gerçek klasör yolu; 2 repo SHA ve temiz/korunmuş çalışma durumu; 3 baseline; 4 kanıtlı test boşluğu; 5 test-only diff; 6 sonuç ve yerel commit; 7 başvuru/PR için ayrı karar.

## Doğruluk

Yerel commit upstream kabulü değildir. Test edilen protokol davranışı gerçek klima ile fiziksel doğrulama anlamına gelmez.

Belge statüsü: Revize görev tanımı; geliştirme/test tamamlanma kanıtı değildir.
