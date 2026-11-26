# Ürün-1 • Test fikstürü ve lehim pastası şablonu

26 Kasım 2026 | 21.11.2026 araştırma kayıtları

Sekiz yeni repo ve önceki turdaki dokuz repo. Kaynak adresleri proje sahiplerinin depolarına gider. Aşağıdaki deneyler öneridir. Bu taramada yazılımlar çalıştırılmadı ve fiziksel prob teması ya da lehim baskısı ölçülmedi.

## Yeni kaynaklar

### 1. snhobbs/kicad-testpoints

KiCad kartından test noktası raporu çıkarır. Tek bir test pedini 1 mm kaydır. Referans aynı kalırken rapordaki koordinat farkını ara.

https://github.com/snhobbs/kicad-testpoints

### 2. TheJigsApp/kicad-testpoints-pcm

Pedin üretim özelliğinden test noktalarını seçerek kimlik, ağ ve konum içeren tablo üretir. Bakırı değiştirmeden bir pedin test noktası işaretini kaldır. Zorunlu noktanın rapordan düşmesini yakala. Aynı JigsApp biçimini kullanan araçların sonuçları bağımsız ölçüm sayılmaz.

https://github.com/TheJigsApp/kicad-testpoints-pcm

### 3. Donnyp751/kicad-test-fixture-tools

Hiyerarşik şemadaki test noktalarını PCB konumlarıyla eşler, karşılık gelen sembol ve kılıfları üretir. İki alt sayfada farklı ağlara bağlı iki test noktası tanımla. Beklenen ağ ve referans eşleşmelerini ayrı ayrı karşılaştır.

https://github.com/Donnyp751/kicad-test-fixture-tools

### 4. hatlabs/kitfg

PCB çevresi ve prob konumlarından yaylı prob fikstürü için STEP plakaları üretir. Revizyon B’de tek probu kaydır. B kartını A fikstürüyle eşleştirince merkez sapmasını raporla.

https://github.com/hatlabs/kitfg

### 5. mmalecki/pcb-jigify

DXF tabanlı kart tutucu ve test fikstürü üretir. Aynı kart çevresini iki koordinat başlangıcıyla dışa aktar. Birden fazla referans deliğinde ortak kaymayı ölç.

https://github.com/mmalecki/pcb-jigify

### 6. tobiasvogel/GerberStencilGenerator

RS-274X açıklıklarını şekil grubu bazında yeniden boyutlandırır ve yeni Gerber üretir. Sensör ped grubunun pasta açıklığını küçült. Açıklık sayısı sabitken toplam açık alan farkını mm² cinsinden kaydet.

https://github.com/tobiasvogel/GerberStencilGenerator

### 7. kennycoder/pcb-to-stencil

Gerber pasta katmanını yazdırılabilir STL şablona dönüştürür. Asimetrik üç açıklıkla üst yüz ve aynalı yüzü ayırt et. Kıyaslamada aynı bakış yönünü kullan.

https://github.com/kennycoder/pcb-to-stencil

### 8. kirberich/gerber_to_scad

Gerber verisinden OpenSCAD şablonu üretir. Aynı pasta dosyasını ikinci bir dönüşümden geçir. Birim, dış ölçü ve seçili açıklığın merkezini kontrol et.

https://github.com/kirberich/gerber_to_scad

## Önceki dokuz referans

### 9. MakerPnP/makerpnp

Varyantlar, otomatik ve elle montaj aşamaları. A ve B varyantlarının yerleşim listesinde beklenen sensör referansını denetle.

https://github.com/MakerPnP/makerpnp

### 10. opiopan/pcb-tools-extension

Gerber panelleme. Döndürülmüş ikinci kartta ped ve delik merkezlerini karşılaştır.

https://github.com/opiopan/pcb-tools-extension

### 11. weiliangng/pcb-diff

Gerber görüntü farkı ve örnek raporlar. Tek ped değişikliğinde farkın doğru katmana düştüğünü kontrol et.

https://github.com/weiliangng/pcb-diff

### 12. curtacircuitos/pcb-tools

Python ile Gerber ve Excellon okuma. Üç delik merkezini ikinci bir okuyucuyla karşılaştır.

https://github.com/curtacircuitos/pcb-tools

### 13. ThisIsNotRocketScience/GerberTools

Gerber seti düzenleme ve panelleme. Panelde kart sayısını, çevreyi ve aralıkları incele.

https://github.com/ThisIsNotRocketScience/GerberTools

### 14. ajw287/diffgerber

Gerber revizyonlarını görsel karşılaştırma. Yalnız lehim maskesi açıklığı değişen iki dosyayı karşılaştır.

https://github.com/ajw287/diffgerber

### 15. dennevi/GrbDiff

GerbV tabanlı revizyon karşılaştırması. Kart çevresinin değişmesinin görsel hizalamaya etkisini incele.

https://github.com/dennevi/GrbDiff

### 16. tracespace/tracespace

PCB katmanlarından SVG görünümü. Denetim bulgusunu ilgili pedin yakınlaştırılabilir görünümüyle belgele.

https://github.com/tracespace/tracespace

### 17. antmicro/gerber2ems

Gerber iz geometrisini OpenEMS simülasyonuna taşır. Aynı uyarım ve malzemelerle iki dönüş yolu geometrisini karşılaştır.

https://github.com/antmicro/gerber2ems

## Öncelikli deney

Kaynak kartta ağ adı, ped kimliği, yüz, koordinatlar ve orijini kaydet. Dört dosya seti hazırla. Temiz sette tüm zorunlu noktalar eşleşmeli. İkinci sette yalnız test noktası işaretini kaldırınca rapor eksik pedi göstermeli. Üçüncü sette yalnız X koordinatını 1 mm değiştirince eski fikstür ile yeni ped arasında 1 mm fark bulunmalı. Dördüncü sette yalnız pasta açıklığını küçültünce pedin bakır merkezi sabit kalmalı ve pasta alanı azalmalı. Geometrik sonuçlar üretim toleransları belirlendiğinde yorumlanır. Bunlar mevcut K1 matrisine eklenmesi değerlendirilmek üzere araştırma önerileridir.
