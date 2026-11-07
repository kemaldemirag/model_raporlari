# SPRINT-2 | GST

_Model-Based Testing • Gömülü Sistem Simülatörü • Geliştirme ve Kanıt Planı_

Tarih: 03.11.2026  |  Repo: https://github.com/emrullahgu/GST  |  Durum: Araştırma / henüz uygulanmadı

## 1. Kapsam ve sınırlar

Sprint-2, GST içinde FSM geçiş sözleşmelerini, testlerin tekrar üretilebilirliğini ve seçilmiş simülasyon davranışlarını incelemeyi hedefler. GST henüz 02.11.2026 Katkı Yaptığım Repolar çalışma alanına eklenmiş değildir. Klonlama, dosya değişikliği, push ve upstream PR için ayrı onay gerekir. Sprint-1 IRremoteESP8266 ile bağımsız yürütülür.

## 2. Doğrulanmış kaynak bulguları

- src/StateMachine.cpp: processEvent() ilk eşleşen ve guard koşulunu geçen geçişi seçer; action(), transitionTo() öncesinde çalışır.

- src/StateMachine.cpp: reset() önce başlangıç durumuna geçer; ardından stateHistory temizlenir ve isRunning=false yapılır. Entry/exit callback etkileri ayrıca sınanmalıdır.

- CMakeLists.txt: incelenen sürümde CTest enable_testing()/add_test() tanımları bulunmadı.

- docs/TESTING.md: 20 PASS ve %92 kapsam beyanı vardır; bu beyanların gerçek yürütme loglarıyla yeniden doğrulanması gerekir.

## 3. İş paketleri

| ID / Öncelik | İş paketi | Dosyalar | Kabul kanıtı |
| --- | --- | --- | --- |
| S2-01 P0 | Baseline ve test envanteri | CMakeLists.txt; docs/TESTING.md | Derleme/test komutları, exit code, log ve commit SHA |
| S2-02 P0 | Guard / geçiş matrisi | include/StateMachine.h; src/StateMachine.cpp | False guard geçiş yapmaz; true guard doğru hedefe gider |
| S2-03 P0 | Action ve callback sırası | src/StateMachine.cpp | Olay iziyle action, onExit, onEntry sırası |
| S2-04 P0 | Geçersiz hedef ve reset | src/StateMachine.cpp | Yan etkiler, tarihçe ve running durumuna ilişkin sözleşme |
| S2-05 P1 | Simülasyon zamanı | src/SimulationCore.cpp | Sabit seed/step ile deterministik sonuç karşılaştırması |
| S2-06 P1 | Sensör arızası ve kestirim | src/Sensor.cpp; src/KalmanFilter.cpp | Gürültü ve stuck-at-zero testleri; varsayımları belgeli |
| S2-07 P1 | CTest ve regresyon | CMakeLists.txt; tests/ (öneri) | CTest keşfi, host CI logları, baseline karşılaştırması |

## 4. Akademik senaryolar

### A — Guarded transition conformance

Referans: Tretmans (2008), Model Based Testing with Labelled Transition Systems, DOI: 10.1007/978-3-540-78917-8_1. Örnek model: (IDLE, START, guard=true) → RUNNING; guard=false ise gözlemlenen durum değişmemeli. Bu bir test tasarımıdır, yürütme sonucu değildir.

### B — Assume–guarantee sözleşmesi

Referans: Benveniste ve diğerleri (2018), Contracts for System Design, DOI: 10.1145/3182014. Varsayım: hedef durum tanımlıdır. Garanti adayı: action ve callback sırası tanımlı olur. Geçersiz hedefte action önce çalışabildiğinden bu sıra özellikle incelenmelidir; düzeltme kararı sözleşmeden sonra verilmelidir.

### C — Mutation / negative tests

Referans: Jia & Harman (2011), An Analysis and Survey of the Development of Mutation Testing, DOI: 10.1109/TSE.2010.62. İzole kopyada guard sonucunu tersine çevirme, geçiş hedefini değiştirme ve callback sırasını bozma mutasyonlarının testlerce yakalanması değerlendirilir.

## 5. Önerilen deney akışı

Kaynak commit sabitle → baseline derle → mevcut test/iddia envanteri → FSM sözleşme matrisi → host-test red/green → simülasyon deterministiklik kontrolü → isteğe bağlı mutation testing → kanıt raporu. Her aşama sonuçları ve başarısızlıklar ayrı kaydedilir.

## 6. Kanıt dosyası şeması

Her test için: repo, commit_sha, test_id, source_path, symbol, assumption, input_sequence, expected_observation, actual_observation, test_command, exit_code, environment, log_path, evidence_level, status (PASS/FAIL/NOT_RUN). NOT_RUN varsayılanıdır.

## 7. Tamamlanma ölçütleri ve riskler

- Baseline derlemesi ve test komutları kayıt altına alınmalı; başarısızlıklar saklanmalı.

- FSM için guard, geçiş, action, callback ve reset senaryoları gerçek API ile derlenip yürütülmeli.

- Dokümantasyondaki kapsam ve performans iddiaları bağımsız ölçüm olmadan doğrulanmış sayılmamalı.

- GST bir simülatördür; gerçek donanım doğrulaması veya dijital ikiz güvenilirliği iddia edilmemeli.

- Upstream PR ancak ayrı kullanıcı onayıyla hazırlanıp gönderilebilir.

## 8. Kaynak bağlantıları

- https://github.com/emrullahgu/GST/blob/master/src/StateMachine.cpp

- https://github.com/emrullahgu/GST/blob/master/include/StateMachine.h

- https://github.com/emrullahgu/GST/blob/master/CMakeLists.txt

- https://github.com/emrullahgu/GST/blob/master/docs/TESTING.md

- https://doi.org/10.1007/978-3-540-78917-8_1

- https://doi.org/10.1145/3182014

- https://doi.org/10.1109/TSE.2010.62
