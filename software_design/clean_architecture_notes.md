# Clean Architecture Book Notes

## What is Design And Architecture ?

- Architecture, bir sistemin iskeletidir. Sistemin en yüksek seviyeli soyutlamasıdır. Sistemin yapısını belirler.

- Architecture, tüm sistemin tasarımı iken, software design modül/komponent/sınıf bazlı bir tasarımdır.

- İkisi bir birini tamamlar. Her mimari bir tasarımdır ancak her tasarım bir mimari değildir.

## The goal of good software design ?

**Minimum efor, maksimum üretkenlik.**

İyi bir yazılım mimarisinin amacı sistemin ihtiyaçlarını giderebilmek ve sürdürebilmek için gereken insan kaynağını minimize etmektir.

Yazılım mimarisinin kalitesini ölçmek basittir, müşterinin isteklerini gerçekleştirebilmek için gereken efor ve zaman az ise mimari iyidir ancak her yenilikte daha fazla efor ve zaman gerekiyorsa bu kötü bir mimaridir.

## A Tale Of Two Values

Bir sistemin fonksiyonu mu daha önemli mimarisi mi ?

Bir çoğu için bu fonksiyonu; yani çalışmasıdır. Ancak bu yanlış bir tutumdur.Sebebini size 2 madde ile açıklayayım:

    1. Elimizde çalışan bir program olsun, ancak değiştirilmesi imkansız. Gereksinimler, ihtiyaçlar değiştiği zaman buna ihtiyaçlara göre değiştiremiyeceğimiz için istediğimiz gibi çalışmayacak. Yani sistem elverişsiz olacak.
    2. Elimizde değiştirilmesi kolay çalışmayan bir sistem olduğunu varsayalım. Ben bunu değiştirip çalışır hale getirebilirim ve yeni ihtiyaçlar geldiğindede bunu sürdürebilirim bu şekilde sistem kullanışlı olmaya devam eder.

## Programming Paradigms

Paradigma, bir şeyi yapmak için izlenen yoldur. Bilgisayar bilimlerinde programming paradigm ise, dil bağımsız programlama için izlenilen yoldur.

Bir paradigma sana hangi programlama yapısını kullanacağını ve nerede kullanacağını söyler.

### Imperative Programming

İlk programlama paradigmalarındandır. Bilgisayara ne yapacağını adım adım ifade etmek diyebiliriz. Soyutlama yoktu her şey olduğu gibiydi.

Yapıtaşlarından olan GOTO ifadesi anlaşılabilir kod yazımını zorlaştıran bir ifadeydi. Bu soruna çözüm olarak structured programming paradigması geliştirilmiştir.

### Structured Programming

Edsger Wybe Dijkstra tarafından keşfedilmiştir.

Dijkstra, kontrolsüz jumpların(atlamaların) programın yapısına zarar verdiğini göstermiştir. Burda bahsedilen **goto** statementlarıdır. Daha sonraları bu gotoları if/then/else ve do/while/until statementları ile değiştirmiştir.

Kısacası yazılımcıların özgürlüğü elinden alınmış oldu ama daha iyi programlar yazılmasını sağladı.

Geniş programları, küçük parçalara bölerek modüler yapılara getirme burada ilk olarak kullanılmıştır.

Daha sonra programcılar, imperative programlamanın yapı taşı olan assignment(yeniden değer atama)'ı evil buldu.

Bunun sonucu olarak buna karşın Functional Programming Paradigması ortaya çıktı.

### Function Programming

Fonksiyonel programlamada temel düşüncelerden biri immutabilitydir yani sembollerin değerleri değişemez.

Bu bize, fonksiyonel programlama dillerinde assignment statement(ifade)'larının olmadığını gösterir. Bazı programlama dillerinde değişken değerlerini değiştirmek için bazı yollar vardır ancak çok katı kurallar ile birlikte.

Ancak bir grup programcı Pointer to Functions yani fonksiyonların referansları ile return edebilmemizi, parametre olarak almamızın iyi bir yazılım yazmak için engel olduğunu öne sürdü.

Buna karşın ortaya OOP çıktı.

**Fonksiyonel programlama bize assignmentlardan kaçınmamızı dayatıyor.**

### Object Oriented Programming

Nesne tabanlı programlama, her yerde kullanılan ve öğretilen paradigma oldu.

Ancak programlar büyürken çıkan problemleri çözmek için bir çok
tasarım desenleri, anti desen ve prensip ortaya çıktı.

Bu prensip, desenlerin çoğuda oop'nin sorunlarını örtmek için çıkarıldı. Composition over inheritance gibi.

Functional programmine göre; concurrency alanında daha başarısız çünkü assignmentlar maintain etmeyi çok zorlaştırıyor. Test edilebilir kod kısmında daha test yazarken daha çok efor istiyor.(mocking/DI)

OOP hakkında daha fazla bilgi için [tıklayınız.](/software_design/oop.md)

#### Çıkarımlar

Gördüğümüz üzere paradigmalar, yazılımcılardan bazı şeyleri yapmamalarını söylüyor ve onların elinden alıyor. Hiçbiri yeni kabiliyetler eklemiyor. Bize daha uzun ömürlü sistemler yaratmak için daha iyi kod yazılacağını düşündükleri bazı disiplinlere teşvik ediyorlar.

**Paradigmalar, ne yapmamızdan çok, neyi yapmamamızı söylerler.**

# REFERENCES

1. https://medium.com/bili%C5%9Fim-hareketi/yaz%C4%B1l%C4%B1m-geli%C5%9Ftirme-trendleri-2017-15-functional-programming-5967f7cd394b
2. Clean Architecture Book By Robert C. Martin
