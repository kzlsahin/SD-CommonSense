# Hatasız Kodlama
[![en](https://img.shields.io/badge/lang-en-red.svg)](BUG-FREE-CODING.md)

## Contents
- [Genel İlkeler](#genel-i̇lkeler)
- [Hatasız Fonksiyonlar/Metodlar Yazma ](#hatasız-fonksiyonlarmetodlar-yazma )
- [İstisna (Hata) Kaynakları](#i̇stisna-kaynakları)
- [Sneaky bugs](#sneaky-bugs)
- [Object Oriented Design](#object-oriented-design)
- [Concurrency](#concurrency)
  - [Essential Considerations for Thread Safety and Locking in Concurrent Programming](#essential-considerations-for-thread-safety-and-locking-in-concurrent-programming)
  - [Principles](#principles)
- [Logging](#logging)

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

### Essential Considerations for Thread Safety and Locking in Concurrent Programming
- **Data Integrity:**
  > Data integrity refers to the accuracy, consistency, and reliability of data stored in a system. In concurrent programming, ensuring data integrity involves preventing corruption or inconsistencies in shared data structures when accessed by multiple threads concurrently.

- **Race Conditions:**
  > Race conditions occur in concurrent programs when the outcome of operations depends on the non-deterministic interleaving or timing of threads. They typically occur when multiple threads access shared resources without proper synchronization, leading to unexpected behavior or incorrect results.

- **Atomicity:**
  > Atomicity refers to the property of an operation that ensures it is performed as a single, indivisible unit. In concurrent programming, atomicity prevents interleaving of operations from multiple threads, ensuring that operations are either fully completed or not executed at all.

- **Memory Visibility:**
  > Memory visibility refers to the guarantees provided by the programming language or runtime environment regarding when changes made by one thread to shared data become visible to other threads. Synchronization mechanisms like locks enforce memory visibility to ensure that changes are propagated to other threads in a timely manner.

- **Deadlocks and Livelocks:**
  > Deadlocks occur in concurrent programs when two or more threads are blocked indefinitely, waiting for resources held by each other. Livelocks occur when threads continuously change their states without making progress, often due to incorrect or ineffective locking strategies.

- **Performance:**
  > Performance in concurrent programming refers to the efficiency and scalability of multi-threaded applications. While locking mechanisms introduce overhead due to synchronization and context switching, they are necessary for ensuring correctness. Optimized locking strategies and thread-safe data structures help minimize contention and maximize performance in concurrent applications.

- **Concurrency Control:**
  > Concurrency control involves managing access to shared resources in multi-threaded applications to prevent race conditions, ensure data integrity, and maintain correctness. It includes designing effective locking strategies, using thread-safe data structures, and coordinating access to shared resources among multiple threads.

### Principles

- **Prefer not to use concurrency if not neccessary, it makes debugging harder.**
  - Rather asynchronous programming than multithread programming.
- **Use thread-safe collections if the collection is supposed to be modfied via "add" and "remove" methods.**
  - Reading operations are thread-safe operations. 
  - Keep in mind reference tpyes and value types. Eventhough thread-safe collections are used, objects may have mutable fields.

- **Use functional programming paradigm for the methods consumed by asynchronous processes**
  - Methods shall never call any external data besides the ones provided as arguments. This makes managing synchronization between async calls easier.
  - Keep methods as simple as possible.
  - Avoid side effects inside methods.
 
- **Modification of objects requires `sync lock` where reading does not**
  - orginize modification calls together and keep them smaller as much as possible
  
## Logging

(in progress)
