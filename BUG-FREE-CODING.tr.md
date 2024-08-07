# Hatasız Kodlama
[![en](https://img.shields.io/badge/lang-en-red.svg)](BUG-FREE-CODING.md)

## Contents
- [Genel İlkeler](#genel-i̇lkeler)
- [Hatasız Fonksiyonlar/Metodlar Yazma ](#hatasız-fonksiyonlarmetodlar-yazma )
- [İstisna (Hata) Kaynakları](#i̇stisna-kaynakları)
- [Sinsi Hatalar](#sinsi-hatalar)
- [Nesne Yönelimli Tasarım](#nesne-yönelimli-tasarım)
- [Eşzamanlılık](#eşzamanlılık)
  - [Eşzamanlı Programlamada Thread Güvenliği ve Kilitlemenin Temel Düşünceleri](#eşzamanlı-programlamada-thread-güvenliği-ve-kilitlemenin-temel-düşünceleri)
  - [İlkeler](#i̇lkeler)
- [Loglama](#loglama)

## Genel İlkeler

- Ağaç kesmeden önce baltanı bile
  
- Sabırlı ol ve rahat yaz
  
  - kod yazarken bilinçli, uyanık old ve acele etme.
  - Süreç sonuçlarını değerlendir, süreçlerin başarısız olabileceğini gözardı etme.
  - etkili bir hata yönetim stratejisi belirle. Mesela, sonuç nesneleri kullan ve log tut.

- Yeniden düzenleme tekniklerini ve tasarım desenlerini öğren ([Refactoring Guru](https://refactoring.guru/refactoring)).
  
- Sihirli sayılar ve dizgiler (magic number ve magic string) kullanımından kaçın [örnek](BUG-FREE-CODING.md#1-avoid-megic-literals)
  
  - Sihirli sayılar ve dizgiler geliştirici için anlaşılır bir bilgi sunmaz
  - Eğer kodlar arasında sihirli sayılar varsa, muhtemelen orda `enum` kullanılmış olmalıydı,
  - Durum belirleyicisi (switch case) olarak dizgiler (`string`) yerine `enum` kullan.
  - Aynı dizgi birden çok yerde kullanılıyorsa, bunun yerine sabit değer kullan, her bir dizgi bellekte ayrı bir yer işgal eder.
  - Dizgileri durum değişkenleri olarak kullanma, bu muhtemel yazım hatalarına karşı savunmasızdır, hata durumunda fark edilmesi zordur. Ayrıca, gelecekteki yeniden yapılandırmalarda büyük sıkıntı ([Shotgun Surgery](https://refactoring.guru/smells/shotgun-surgery))  yaratabilir.
  - Sabit değerler ve enum'lar kullanmak 'intellisense' desteğini önünüze serer.

- "Fail Fast" yaklaşımını benimse ve "Guard Clause" yapıları kullan. 

## Hatasız Fonksiyonlar/Metodlar Yazma 

Her şeyden önce, herhangi bir fonksiyon veya metod (bundan sonra birbiri yerine kullanılacak) öyle bir şekilde yazılmalıdır ki, herhangi bir geliştirici, fonksiyonun karakteristik özelliklerini ilk bakışta anlamalıdır. Bu özellikler şunlardır:

- **`Exception`**: Fonksiyon tarafından fırlatılan istisnalar,
- **Girdi doğrulama**: Girdi türleri ve özel doğrulama kuralları,
- **Dönüş türü**: Fonksiyonun döndürdüğü değer tipi ve `null` döndürüp döndürmemesi,
- **Bağımlılık**: Fonksiyonun dışında herhangi bir değişken veya sabite bağımlı olup olmadığı,
- **Yan Etkiler**: Fonksiyon kapsamının dışında herhangi bir şey yapıp yapmadığı.

İşte benim Hatasız metotlar yazmak için oldukça önemli gördüğüm prensipler:

- **Her fonksiyonu yazmadan önce ana hatlarını belirle:**

  - Genel iş akışını belirle
  - Mümkün olan girdi yelpazesini belirle
  - Mümkün olan çıktı yelpazesini belirle
  - Olası yan etkileri belirle

- **Fonksiyonun özelliklerini belirle:**

- **Fonksiyon için ihtiyaç duyulan testleri tasarla:**
  
  - Belirtilen kuralları test kapsamı kontrol listesi olarak kullan
  - Null girişlerini test edin
  - İstisnaları test edin
  - Global bağımlılıkların her olası durumunu test edin
  
- **Mümkünse herhangi bir global bağımlılıktan kaçın:**
  
  - Herhangi bir global benzeri bağımlılığı açıkça belirt ([örnek](BUG-FREE-CODING.md#2-document-side-effects)).
  - Eğer fonksiyon herhangi bir global özelliği kullanıyorsa, bunu fonksiyonun başında yap.
  - Eğer fonksiyon iç sürecin bir ürünü olarak herhangi bir global benzeri özelliği ayarlıyorsa, bunu fonksiyonun sonunda yap.
  - Global bağımlılıkları eşzamanlılık açısından kontrol et.
    
- **Fonksiyonu mümkün olduğunca basit tutun.**
  
- **Girdileri dikkatlice gözden geçir:**
  
  - Başlangıç durumunu doğrulamak için erken dönüşleri kullan.
  - Fonksiyon argümanlarını null değerlere karşı kontrol et.
  - Eğer herhangi bir koleksiyon tipi argüman varsa boş olup olmadığını kontrol et.
  
- **Karmaşık iç içe geçmiş tipler için tip aliası kullan, örneğin `Action<Action<UseState>, Object[]>`:**
  
  - Bu, gelecekteki hata ayıklama ve kod tabanını yeniden yapılandırma ([Shotgun Surgery](https://refactoring.guru/smells/shotgun-surgery)) zorluklarını önler

- **Tekrarlanan nokta notasyonu zincirleri için yerel değişkenleri alias olarak kullan.**

  - Bu, kodun performansını ve okunabilirliğini artırır.
 
- **Null değeri yerine boş koleksiyon döndür**
  
  - Bu şekilde, çağıran için sadece koleksiyonun uzunluğunu kontrol etmek yeterli olacaktır.
  - Bu, gözden kaçırılan null istisnalarını önler.

- **Fonksiyonu açıkça belgele ve güncel tut:**
  
  - Fonksiyonun ne yaptığını açıkça belirt.
  - Herhangi bir girdiylr ilgili doğrulama kuralını belirt.
  - Herhangi bir global bağımlılığı açıkça belirt.
  - Fırlatılabilecek tüm istisnaları listele.
  - `Null` dönüş olasılığını belirt.
    
- **Her fonksiyonu sanki bir kağıda yazılmış gibi iki kere kontrol et:**
  
  - Gözden kaçırılmış herhangi bir olası `Exception` için kontrol et.
  - Gözden kaçırılmış, null değeri gelebilecek noktaları kontrol et.
  - Herhangi bir boş koleksiyon ihtimalini kontrol et.


## İstisna Kaynakları
- **Sıfıra bölme**

Bir kod bloğunu gözden geçirirken, bölme operatörlerine dikkat edin. Bölenin sıfır değerine sahip olmadığından veya bu istisnanın işlendiğinden emin olun.

- **Boş argümanlar**

Argümanlar veya karmaşık değişkenler boş değerlere sahip olabilir. Herhangi bir referans tipi değerinin boş olup olmadığını kontrol edin veya bu istisnanın uygun şekilde işlendiğinden emin olun.

- **Aralık dışı indis**

Döngülerde indis akışını her zaman gözden geçirin, ve emin olun ki;

  - indisler doğru değerden başlıyor
  - indisler uygun şekilde arttırılıyor
  - indis artışı etrafındaki karmaşık mantıktan kaçınılıyor.
 
## Sinsi Hatalar

İstisna fırlatmayan ancak istenmeyen davranışlara neden olan hatalar. Bu hataları tanımlamak zordur.

- **Varsayılan Değerler**

Değer tipindeki değişkenlerin varsayılan değerleri vardır. Özellikle derinlemesine iç içe geçmiş algoritmalar üzerinde çalışırken, tanımlanmamış değişkenleri kullanmak bile fark edilmeden mümkündür.

## Nesne Yönelimli Tasarım
(devam ediyor)

## Eşzamanlılık

Microsoft'un CLR ekibi içerisindeki paralel programlama tasarımcısının bakış açısından [eşzamanlı programlamanın (concurrency) tarihi](https://joeduffyblog.com/2016/11/30/15-years-of-concurrency/).

### Eşzamanlı Programlamada Thread Güvenliği ve Kilitlemenin Temel Düşünceleri
- **Veri Bütünlüğü:**
  > Veri bütünlüğü, bir sistemde depolanan verilerin doğruluğu, tutarlılığı ve güvenilirliği ile ilgilidir. Eşzamanlı programlamada, veri bütünlüğünü sağlamak, birden çok iş parçacığı tarafından aynı anda erişilen paylaşılan veri yapılarında bozulmayı veya tutarsızlıkları önlemeyi içerir.

- **Yarış Koşulları:**
  > Yarış koşulları, işlemlerin sonucunun iş parçacıklarının belirlenmemiş sıralanması veya zamanlamasına bağlı olduğu zaman eşzamanlı programlarda meydana gelir. Genellikle birden çok iş parçacığı, uygun senkronizasyon olmaksızın paylaşılan kaynaklara erişir, bu da beklenmeyen davranışlara veya yanlış sonuçlara yol açar.

- **Atomluluk:**
  > Atomluk, bir işlemin tek bir, bölünemez birim olarak gerçekleştirilmesini sağlayan özelliğe referans eder. Eşzamanlı programlamada, atomluk, iş parçacıklarının işlemlerinin karşılıklı olarak geçiştirilmesini önler, işlemlerin tamamen tamamlanıp tamamlanmadığını veya hiç yürütülmediğini garanti eder.

- **Bellek Görünürlüğü:**
  > Bellek görünürlüğü, bir iş parçacığının paylaşılan verilere yaptığı değişikliklerin diğer iş parçacıkları tarafından ne zaman görülebilir hale geldiğine dair programlama dilinin veya çalışma zamanı ortamının sağladığı güvencelere atıfta bulunur. Kilitler gibi senkronizasyon mekanizmaları, bellek görünürlüğünü zorunlu olarak sağlar ve değişikliklerin diğer iş parçacıklarına zamanında yayılmasını sağlar.

- **Kilitlenmeler ve Canlı Kilitler:**
  > Kilitlenmeler, eşzamanlı programlarda iki veya daha fazla iş parçacığının, birbirlerinin elinde tuttuğu kaynakları sonsuz bir şekilde beklediği durumlarda ortaya çıkar. Canlı kilitler, iş parçacıklarının sürekli olarak durumlarını değiştirmelerine rağmen ilerleme kaydetmemeleri durumunda, genellikle yanlış veya etkisiz kilit stratejilerinden kaynaklanır.

- **Performans:**
  > Eşzamanlı programlamadaki performans, çoklu iş parçacıklı uygulamaların verimliliği ve ölçeklenebilirliği ile ilgilidir. Kilit mekanizmaları senkronizasyon ve bağlam değiştirme nedeniyle bir miktar iş yükü getirse de, doğruluğu sağlamak için gereklidirler. Optimizasyon yapılmış kilit stratejileri ve iş parçacığı güvenli veri yapıları, çekişmeyi en aza indirir ve eşzamanlı uygulamalardaki performansı maksimize eder.

- **Eşzamanlılık Kontrolü:**
  > Eşzamanlılık kontrolü, çoklu iş parçacıklı uygulamalarda paylaşılan kaynaklara erişimi yöneterek yarış koşullarını önlemek, veri bütünlüğünü sağlamak ve doğruluğu korumakla ilgilidir. Bu, etkili kilit stratejileri tasarlamayı, iş parçacığı güvenli veri yapıları kullanmayı ve birden çok iş parçacığının paylaşılan kaynaklara erişimini koordine etmeyi içerir.

### İlkeler
- **Eğer gerekli değilse eşzamanlılıktan kaçının, hata ayıklamayı zorlaştırır.**
  - Çok iş parçacıklı programlamadan ziyade asenkron programlamayı tercih edin.
    
- **Koleksiyon "ekle" ve "kaldır" yöntemleri ile değiştirilmek üzere ise, iş parçacığı güvenli koleksiyonları (thread-safe collections) kullanın.**
  - Okuma işlemleri iş parçacığı güvenli işlemlerdir.
  - Referans türlerini ve değer türlerini akılda tutun. İş parçacığı güvenli koleksiyonlar kullanılsa bile, nesnelerin değiştirilebilir alanlara sahip olabileceğini unutmayın.

- **Asenkron işlemler tarafından tüketilen yöntemler için fonksiyonel programlama paradigmını kullanın.**
  - Yöntemler asla argümanlar olarak sağlananlar dışında herhangi bir harici veri çağırmamalıdır. Bu, asenkron çağrılar arasındaki senkronizasyonu yönetmeyi kolaylaştırır.
  - Yöntemleri mümkün olduğunca basit tutun.
  - Yöntemlerin içinde yan etkilerden kaçının.

- **Nesnelerin değiştirilmesi, okumaya gerek yoksa sync lock gerektirir.**
  - Değiştirme çağrılarını bir araya getirin ve mümkün olduğunca küçük tutun.
  
## Loglama

(in progress)
