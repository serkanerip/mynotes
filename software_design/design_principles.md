# Design Principles

### Composition Over Inheritance

- Öncelikle bu ikiside kötü bir teknik değildir ikisininde doğru ve yerinde kullanılması kimi zaman beraber kullanılması gerekmektedir.
- Inheritance ile ilişkilendirilen sınıflar bir biriyle sıkı bağlanırlar.
- Ancak composition ile oluşturulan ilişkiler daha esnek bağlar oluşturur.
- Esnek bağlı ilişkiler koda esneklik sağlar ve test yazmayı kolaylaştırır.
- Inheritance, geleceği görmeye zorlar, çünkü planlamadığınız bir değişiklikte inheritance ilişkiniz tamamen patlayabilir. Örnek olarak Bird adında bir superclass'ınız var ve bundan türeyen alt sınıflarınız var farzı misal Owl, Crow gibi daha sonra Penguin sınıfına ihtiyacınız oldu bunu Bird sınıfından inherit ettiniz ancak bir sorun var. Bird sınıfının fly adında bir metodu var ? Penguen uçamayan kuşlardandır ve fly adında bir metodu oldu bu yüzden inheritance kullanırken geleceği görmek(!) lazım.

### Hollywood Principle

**Don't call us, we'll call you.**

- Üst seviyedeki katmanlar, alt seviyedeki katmanları çağırabilir tersi olamaz.
- Observer ve Template pattternlar ile bunu sağlıyabiliriz.
- Page ve Document adlı iki sınıfımız olduğunu düşünelim. Burada document üst seviye katman, page ise alt seviye, bu yüzden page sınıfı document sınıfını çağıramamalıdır. Document bir değişiklikte sahip olduğu pagelere re-render etmesini söyler callback ve ya eventler ile bu şekilde bu prensibi uygulamış oluruz.

### Dry Principle

**Don't repeat yourself.**

Adındanda anlaşılacağı üzere kendini tekrar etme !

Peki ne anlama geliyor bu ? Diyelim ki belli bir işi yapan bir kod parçacığınız var ve sizde buna projenin bir kaç yerinde ihtiyaç duyuyorsunuz. Ve bunu her ihtiyaç olan yerde copy-paste yardımıyla ve ya tekrardan yazarak yerleştiriyorsunuz. Program çalışıyor mu ? çalışıyor ta ki bir o kod parçacığında bir bug bulunana kadar ve ya yeniliklere göre değiştirilmesi gerektiğinde.

O kod parçacığını bir yerde fix'liyorsunuz gidip başka bir yerde yine aynı işlemi tekrarlıyorsunuz ancak bunun için o kod parçacığını aramak zorundasınız ve muhtemelen 1 tane de olsa atladığınız yer olacaktır. Buda sizin bir problemi çözmeniz için daha çok efor ve zaman harcamanıza sebeb olacak bu da istenen bir durum değildir !

Bu sorunu o kod parçacığını bir fonksiyon ve ya sınıf haline getirerek lazım olan yerlerde kullanarak aşabiliriz.

Bu yüzden sistemimizi tekrar kullanılabilir küçük parçalardan oluşturmamız yararımıza olacaktır.

### KISS Principle

**Keep It Simple and Stupid**

Basit ve aptal(kolay anlaşılır) tut demektir. Yani bir yazılımı geliştirirken basit ve anlaşılır tutmak daha sonra sizin bu uygulamayı geliştirirken ve ya başkalarının yazdığınız kodu okuması için harcayacağı eforu ve zamanı azaltabilirsiniz. Buda yazılımcıların daha üretken olmasını sağlar.

**Unutmayın kod bir defa yazılır bir çok defa okunur. O yüzden okunabilir kod yazmak önemlidir.**
