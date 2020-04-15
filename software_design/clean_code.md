# Clean CODE

## NAMING

- Bir değişkenin ve fonksiyonun maksatı isminden anlaşılmalıdır.
- İsimler telafuz edilebilir olmalıdır.
- İsimlendirmeler tam olarak yaptığı herşeyi açıklamalıdır bu uzun bir isme denk gelse de eğer isimlendirmekte zorlanıyorsanız refactor zamanı gelmiştir demektir.
- İsimlendirmeler yanlış bilgi vermemelidir.List, set gibi sıfatları sadece değişken o tipteyse kullanmalıyız. Onun yerine collection, group gibi isimler tercih etmeliyiz.
- Aranabilir değişkenler ve sabitler kullanılmalıdır.
  ```python
  def create_person(data):
      if data['age']['year'] < 18:
          return None
  ```
  Burada 18 yerine MIN_AGE gibi bir değişken ve ya constant kullanabiliriz daha sonradan bu gereksinim değişeceği zaman 18 sayısını aramak yerine değişken ismiyle arıyabiliriz projede 18 sayısı birden fazla yerde kullanılmış olabilir.
- Sınıf ve nesne isimleri noun(isim) olmalıdır.
- Sınıf isimlerinde **Manager**, **Info**, **Controller** gibi birden fazla anlama gelen kelimeleri kullanmaktan kaçınmalıyız.
- Metod isimleri fiil olmalıdır.(get, set, delete)
- Gerektiğinde değişken isimlerinde ait olduğu contextide belirtin.
- Bir konsept için kullandığımız kelimeyi tüm codebase de aynı şekilde kullanmalıyız.

  Farzı misal, bir şeyin değerini almak için fetchValue kullanıyorsak başka yerde getValue ve ya retrieveValue kullanmamalıyız.

## Functions

- Fonksiyonlar 20 satırdan daha fazla olmamalıdır. Hatta daha kısa olmalıdır.
- Sadece bir şeyi yapmaktan sorumlu olmalılar.
- Eğer bir fonksiyon sadece isminde belirtilen adımları yapıyorsa, o fonksiyon bir şey yapıyordur.
- Switchler polimorfizm ile değiştirilmeli.
- Parametre sayısı olabildiğince az olmalıdır test yapmayı zorlaştırır.
- Boolean argüman kullanılmamalıdır.
- Side effectler'den kaçınılmalı pure functionlara öncelik vermeliyiz. Function scope'u içinde sadece argümanlar ve local nesneler olmalıdır global olmamalıdır.
- Try catch bloklarının gövdeleri ayrı fonksiyonlara parçalanmalıdır.
- Hataları işleyen metod başka iş yapmamalıdır.

### Function Arguments:

Bir fonksiyonun argümanlarının çokluğu o fonksiyonun kullanımını meşakatli hale getirir. Parameterleri yanlış sırada vermek gibi hatalara ve kodun okunabilirliğine zarar verir.Aynı zamanda test edilmesini zorlaştırır.

Çözümler:

1. Eğer 2 den fazla parametre alıyorsa ve bunların bir biriyle ilişkisi varsa bunlar bir nesne olarak birleştirilip parametre olarak alınmalıdır.
2. Function Overloading
3. Builder Pattern: Kod örneği üzerinden görelim.

   ```java
   ProcessDialog.show('title', 'message', false, true, listener); // v1

   ProcessDialog dialog = new ProcessDialog.Builder()
   .setTitle('title')
   .setMessage('message')
   .setIndeterminate(false)
   .setCancelable(true)
   .setListener(listener)
   .build();
   ```

Yukarıdaki örnekte görüldüğü üzere builder pattern ile hangi parametrenin ne işe yaradığı açıkca anlaşılıyor.

## Command Query Seperation (CQS)

Side effectler çoğu zaman kaçınılmazdır. Bizim amacımız bu yan etkileri kontrol edebilmektir.

Yan etkileri yönetmenin iyi bir yöntemi command ve queryler arasında güçlü bir ayrım yapmalıyız.

**Command(mutator, modifier):** fonksiyonlar bir mutation yapan side effectlerdir.

**Query:** fonksiyonlar side effect bulunmayan sonuç döndüren fonksiyonlardır.

**State'i değiştiren fonksiyonlar bir değer döndürmemeliler ve bir değer döndüren fonksiyonlar da state'i değiştirmemeliler.**

Bu söz **Bertrand Meyer** tarafından bir kitabında kullanılmıştır.

Bu prensip bizim kolayca bir fonksiyonun side effect olup olmadığını anlamamızı sağlar.

```java
int m();  // query
void n(); // command
```

Bir fonksiyonda hata oluştuğunda bir şeyler döndürmek yerine error exception throw etmeliyiz.

# Kaynaklar

- https://dev.to/danialmalik/a-beginner-s-guide-to-clean-code-part1-naming-conventions-139l
- https://martinfowler.com/bliki/CommandQuerySeparation.html
- https://hackernoon.com/object-oriented-tricks-3-death-by-arguments-d070ac86d996
