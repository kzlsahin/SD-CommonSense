# Hatasız Kodlama
[![en](https://img.shields.io/badge/lang-en-red.svg)](https://github.com/kzlsahin/SD-CommonSense/blob/main/BUG-FREE-CODDING.md)

## İlkeler

### Genel İlkeler

- Sihirli sayılar ve dizgiler (magic number ve magic string) kullanımından kaçın [örnek](https://github.com/kzlsahin/SD-CommonSense/blob/main/BUG-FREE-CODDING.md#1-avoid-megic-literals)
  
  - Sihirli sayılar ve dizgiler geliştirici için anlaşılır bir bilgi sunmaz
  - Eğer kodlar arasında sihirli sayılar varsa, muhtemelen orda `enum` kullanılmış olmalıydı,
  - Durum belirleyicisi (switch case) olarak dizgiler (`string`) yerine `enum` kullan.
  - Aynı dizgi birden çok yerde kullanılıyorsa, bunun yerine sabit değer kullan, her bir dizgi bellekte ayrı bir yer işgal eder.
  - Dizgileri durum değişkenleri olarak kullanma, bu muhtemel yazım hatalarına karşı savunmasızdır, hata durumunda fark edilmesi zordur. Ayrıca, gelecekteki yeniden yapılandırmalarda büyük sıkıntı ([Shotgun Surgery](https://refactoring.guru/smells/shotgun-surgery))  yaratabilir.
  - Sabit değerler ve enum'lar kullanmak 'intellisense' desteğini önünüze serer.

### Hatasız Fonksiyonlar/Metodlar Yazma 

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
  
  - Herhangi bir global benzeri bağımlılığı açıkça belirt ([örnek](https://github.com/kzlsahin/SD-CommonSense/blob/main/BUG-FREE-CODDING.md#document-side-effects)).
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
