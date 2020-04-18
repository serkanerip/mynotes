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

### YAGNI Principle

**You Ain't Gonna Need It**

Birşeyleri gerçekten ihtiyacınız olduğu zaman projenize ekleyin ileride kullanırım diye değil.
Unutmayın ne kadar çok kod o kadar bug o ve kadar maintain edilmesi zor bir sistemdir.

Bu prensip kısaca bizden bunu istemektedir.

### SOLİD Principles

SOLID, uncle bob tarafından ortaya atılmış nesne tabanlı tasarımın ilk 5 prensibinin kısaltmasıdır.

Bu prensiplerin kullanılmasıyla kolayca geliştirilebilen ve sürdürülebilen yazılımlar geliştirebiliriz.

#### Single Responsibility Principle

**Bir modül, sadece bir sebepten dolayı değişmelidir.**

Modül en basit anlamıyla bir kaynak dosyasıdır.

Bir sınıf düşünelim Employee adında bunun calculatePay, reportHours ve save adında 3 metodu bulunsun.

calculatePay ve reportHours metodlarının ortak bir algoritma kullandıklarını varsayalım bu algoritmada regularHours adında private bir metod üzerinde çalışıyor.

Bir aktör calculatePay üzerindeki hesaplamanın değişmesini istedi ve bir takım bu işe girişti ve bu düzenlemeyi yapmak için regularHours fonksiyonunu düzenlediler. Bu metodu reportHours metodununda kullandıklarını göremediler ve bu sorunu çözüp kapattılar.

Daha sonra baktılarki raporlarda hatalı sonuçlar var çünkü regularHours metodu hesaplama biçim değişti ve bu reportHours metodunu etkiledi ve bu da birilerinin çok zarar etmesine sebeb oldu.

**Gördüğümüz gibi bir class farklı işler yapıyor ve bu yüzden birden fazla sebeble değişebiliyor. Buda hatalar ve buglara sebeb olabiliyor.**

#### Open Closed Principle

**Bir yazılım yeniliklere açık, değişimlere kapalı olmalıdır.**

Yani bir yazılım, eski kodları silmeden, değiştirmeden genişleyebilmeli.

Bu prensibe bizim yazılım mimarisi geliştirmemizin temel nedeni diyebiliriz.

**Ex:**

Düşünün, bir sistemimiz var finansal raporları web sayfası üzerinde görüntülüyor. Web sayfası scroll edilebiliyor ve negatif rakamlar kırmızı ile yazılmış.

Daha sonra bizden bu raporun siyah-beyaz çıktı olarak çıkarılabilmesinin istendiğini düşünün. Negatif rakamlar bu seferde parantez içinde yazılmalı.

İyi bir yazılım çok az bir değişilikle bu yeniliği ekleyebilir ideal olanı sıfır değişiklik.

Peki Nasıl ?

Farklı sebeblerle değişen şeyleri düzgün bir şekilde ayırmak (SRP) ve bunlar arasındaki bağımlılıkları iyi yönetmek.(DIP)

FinancalReportData adında bir superclass olduğunu düşünün ve bundan türetilmiş WebReporter ve PrintReporter adında 2 sınıf.

Bu şekilde kodumuzda çok fazla efor sarfetmeden ve diğer çalışan yerleri bozmadan yenilik ekliyebiliriz.
