# Hatasız Kodlama
[![en](https://img.shields.io/badge/lang-en-red.svg)](https://github.com/kzlsahin/SD-CommonSense/blob/main/BUG-FREE-CODDING.md)

## İlkeler

### Genel İlkeler

- Sihirli sayılar ve dizgiler (magic number ve magic string) kullanımından kaçının [örnek](https://github.com/kzlsahin/SD-CommonSense/blob/main/BUG-FREE-CODDING.md#1-avoid-megic-literals)
  
  - Sihirli sayılar ve dizgiler geliştirici için anlam taşımaz
  - Eğer kodlar arasında sihirli sayılar varsa, muhtemelen `enum` kullanılmalıdır,
  - Eğer dizgiler insan tarafından okunabilir durum belirleyicisi olarak kullanılıyorsa, `enum` kullanılmalıdır.
  - Aynı dizgi birden çok yerde kullanılıyorsa, her bir dizgi bellek işgal ettiği için sabit değer kullanın.
  - Dizgileri durum değişkenleri olarak kullanmayın, bu muhtemelen yazım hatalarına yol açabilir ve gelecekteki yeniden yapılandırmalarda büyük sıkıntı ([Shotgun Surgery](https://refactoring.guru/smells/shotgun-surgery))  yaratabilir.
  - Geliştiriciye intellisense avantajı sağlamak için sabit değerler ve enum'lar kullanın.

### Hatasız Fonksiyonlar/Metodlar Yazma 

Her şeyden önce, herhangi bir fonksiyon veya metod (biriyle değiştirilebilir olarak kullanılacak) öyle bir şekilde yazılmalıdır ki, herhangi bir geliştirici fonksiyonun özelliklerini ilk bakışta anlamalıdır. Bu özellikler şunlardır:
- **İstisnalar**: Fonksiyon tarafından fırlatılan istisnalar ele alınmalıdır (ve hangi türde olduğu),
- **Giriş doğrulama**: Giriş türleri ve özel doğrulama kuralları
- **Dönüş türü**: Fonksiyonun null döndürüp döndürmemesi beklenebilir
- **Bağımlılık**: Fonksiyonun dışında herhangi bir değişken veya sabite bağlı olup olmadığı
- **Yan Etkiler**: Fonksiyon kapsamının dışında herhangi bir şey yapar mı

İşte benim Hatasız metotlar yazmak için oldukça önemli gördüğüm prensipler:

- **Her fonksiyonu yazmadan önce ana hatlarıyla belirtin:**

  - Genel iş akışını belirleyin
  - Mümkün olan giriş aralığını belirleyin
  - Mümkün olan çıkış aralığını belirleyin
  - Olası yan etkileri belirleyin

- **Fonksiyonun özelliklerini belirleyin:**

- **Fonksiyon için ihtiyaç duyulan testleri tasarlayın:**
  
  - Belirtilen kuralları test kapsamı kontrol listesi olarak kullanın
  - Null girişlerini test edin
  - İstisnaları test edin
  - Global bağımlılıkların her olası durumunu test edin
  
- **Mümkünse herhangi bir global bağımlılıktan kaçının:**
  
  - Herhangi bir global benzeri bağımlılığı açıkça belirtin ve gösterin ([örnek](https://github.com/kzlsahin/SD-CommonSense/blob/main/BUG-FREE-CODDING.md#document-side-effects).
  - Eğer fonksiyon herhangi bir global özelliği kullanıyorsa, bunu fonksiyonun başında alın
  - Eğer fonksiyon iç sürecin bir ürünü olarak herhangi bir global benzeri özelliği ayarlıyorsa, bunu fonksiyonun sonunda yapın
  - Global bağımlılıkları eşzamanlılık açısından kontrol edin
    
- **Fonksiyonu mümkün olduğunca basit tutun.**
  
- **Girişleri dikkatlice ele alın:**
  
  - Başlangıç durumunu doğrulamak için erken dönüşleri kullanın
  - Fonksiyon argümanlarını null değerlere karşı kontrol edin
  - Eğer herhangi bir koleksiyon tipi argüman varsa boş olup olmadığını kontrol edin
  
- **Karmaşık iç içe geçmiş tipler için tip aliası kullanın, örneğin `Action<Action<UseState>, Object[]>`:**
  
  - Bu, gelecekteki hata ayıklama ve kod tabanını yeniden yapılandırma ([Shotgun Surgery](https://refactoring.guru/smells/shotgun-surgery)) zorluklarını önler

- **Tekrarlanan noktalı notasyon zincirleri için yerel değişkenleri aliaz olarak kullanın.**

  - Bu, kodun performansını ve okunabilirliğini artırır.
 
- **Null değeri yerine boş koleksiyon döndürün**
  
  - Bu şekilde, çağıran için sadece koleksiyonun uzunluğunu kontrol etmek yeterli olacaktır.
  - Bu, gözden kaçırılan null istisnalarını önler.

- **Fonksiyonu açıkça belgeleyin ve güncel tutun:**
  
  - Fonksiyonun ne yaptığını kısaca açıklayın
  - Herhangi bir girişle ilgili doğrulama kuralını belirtin
  - Herhangi bir global bağımlılığı kısaca belirtin
  - Fırlatılabilecek tüm istisnaları listeleyin
  - `Null` dönüş olasılığını belirtin.
    
- **Her fonksiyonu sanki bir kağıda yazılmış gibi çift kontrol edin:**
  
  - Herhangi bir olası istisna için kontrol edin.
  - Herhangi bir olası null değeri için kontrol edin.
  - Herhangi bir olası boş koleksiyon için kontrol edin.
