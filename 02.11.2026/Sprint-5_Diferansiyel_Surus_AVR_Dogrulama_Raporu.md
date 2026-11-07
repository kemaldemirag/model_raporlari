# SPRINT-5

_Diferansiyel Sürüş ve AVR Zamanlama Doğrulaması_

03.11.2026  |  Hedef: anis022/inf1900-107111  |  Durum: Araştırma ve geliştirme hazırlığı

## 1. Kapsam ve sınırlar

Amaç: lib/motor.hpp ve ilişkili tekerlek/timer uygulamalarında hareket komutlarını, PWM çıkışlarını ve zamanlama davranışlarını model tabanlı testlere dönüştürmek. Kod henüz çalıştırılmadı; repo mevcut 02.11.2026 çalışma alanına eklenmiş sayılmıyor. Yeni GitHub reposu, fork, push veya PR onaysız yapılmayacak.

## 2. İş paketleri

| ID | Görev | Öncelik | Kabul kanıtı |
| --- | --- | --- | --- |
| S5-01 | Lisans, commit, README, build/test komutları | P0 | Pinlenmiş SHA, bağımlılık ve baseline kanıtı |
| S5-02 | Motor arayüzü ve hareket primitive’leri | P0 | Fonksiyon–önkoşul–sonkoşul tablosu |
| S5-03 | Sol/sağ PWM ve yön işaretleri | P0 | Mock PWM/GPIO çağrı izi |
| S5-04 | Timer / PHASE_CORRECT ve periyot | P0 | Register–prescaler–frekans eşleştirmesi |
| S5-05 | Forward/backward/spin/stop geçişleri | P1 | FSM transition ve boundary matrisi |
| S5-06 | Diferansiyel sürüş referans modeli | P1 | Analitik kinematik karşılaştırma |
| S5-07 | Host-test ve akademik kanıt paketi | P1 | Test komutu, log, sonuç, açık riskler |

## 3. Dosya yolu bazlı inceleme

- lib/motor.hpp — Motor sınıfı, kurucu, hareket API’si; mevcut kodda doğrulanmış yol.

- lib/wheel.hpp veya ilgili tekerlek başlığı — Gerçek konumu ve adları repo ağacından doğrulanacak; PWM/yön eşlemesi.

- Timer sınıfının başlık ve uygulama dosyaları — Gerçek yollar aranacak; PHASE_CORRECT, prescaler ve compare register davranışı.

- README / build dosyaları — Derleyici, MCU hedefi, test ve lisans kanıtı.

## 4. Akademik modeller ve deneyler

Diferansiyel sürüşte v=(vR+vL)/2 ve ω=(vR−vL)/L. Bu kinematik ilişki, tekerlek hızlarının fiziksel olarak doğru üretildiğini değil, komutlardan beklenen ideal hareketi tanımlar. Eşit hız, zıt hız, sıfır hız ve asimetrik tekerlek senaryolarını test et.

PWM testinde gerçek timer saat frekansı, prescaler, TOP değeri ve seçilen sayma modu doğrulanmadan frekans formülü sabitlenmeyecek. PHASE_CORRECT için datasheet’e göre çift eğimli sayım hesaba katılacak.

Örnek senaryolar: ileri→dur; geri→dur; ileri→geri; sol/sağ spin; PWM=0 ve maksimum; tekrarlanan stop; timer taşma ve geçersiz girişler. Her testte giriş, beklenen register/pin izi ve gözlenen sonuç ayrı tutulmalı.

## 5. Kaynaklar ve izlenebilirlik

- Kaynak repo: https://github.com/anis022/inf1900-107111

- Öncelikli dosya: https://github.com/anis022/inf1900-107111/blob/master/lib/motor.hpp

- Borenstein & Feng (1996), Measurement and Correction of Systematic Odometry Errors in Mobile Robots: https://doi.org/10.1109/70.544770

- Tretmans (2008), Model Based Testing with Labelled Transition Systems: https://doi.org/10.1007/978-3-540-78917-8_1

- NASA MC/DC tutorial: https://ntrs.nasa.gov/citations/20010057789

- AVR timer için ilgili MCU’nun resmî Microchip datasheet’i ve Arduino core kaynakları ayrıca sürüme göre seçilecek.

## 6. Red/Green ve teslim şartları

RED: hedef davranışın mevcut kodda başarısız olduğunu gösteren tekrarlanabilir host-test veya açıkça belgelenmiş test boşluğu. GREEN: en küçük düzeltme sonrası aynı testin ve baseline’ın başarılı çalışması. İyileştirme testi ise RED zorunlu olmayabilir; gerekçesi raporlanır. Her iddiaya commit SHA, komut, ortam, stdout/stderr ve test dosyası eklenir. Fiziksel motor güvenliği için host-test yeterli kanıt değildir.

## 7. Sprint kapanış kararı

İlk kapı: lisans ve gerçek kaynak dosya yollarının doğrulanması. İkinci kapı: timer/PWM ve hareket sözleşmesi. Üçüncü kapı: testlerin derlenmesi, koşması ve kanıtların kaydı. Upstream PR yalnızca ayrı kullanıcı onayıyla.

## Belge durumu

> Sprint-5 | Tasarım raporu — yürütülmüş test sonucu değildir
