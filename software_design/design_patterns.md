# Design Patterns

Design patterns are typical solutions to common problems in software design. Each pattern is like a blueprint that you can customize to solve a particular design problem in your code. 

## Creational

### Singleton

Bir sınıfın sadece bir nesnesi olur yeni nesne oluşturulamaz ve uygulamanın her yerinde bu kullanılır.

### Factory

Bir interface den türetilen 3 tane somut sınıfımız olduğunu düşünelim biz bunlardan hangisinden nesne oluşturacağımızın algoritmasını bir factory sınıfına yazıp diğer bütün codebase'de bunu kullanarak sınıf oluşturma işlemini tek bir elden yapabiliriz.


ICustomer - > NormalCustomer, ElitCustomer, PlatiniumCustomer.

Yukarıda ki örnekte 3 tip müşteri vardır biz müşterimiz ile ilgili işlem yaparken hangi tip müşteri oluşturacağımızı müşterinin hesabındaki para miktarına göre yaptığımızı düşünelim bir factory sınıfı yazıp bu logici oraya yazıp uygun müşteriyi bizim için oluşturur. **Bu şekilde nesneyi seçme ve oluşturma işlemi de testable bir kod haline gelir.**


###  Abstract Factory

Benzer tipte fakat farklı ürün üreten 2 ayrı factorymiz olduğunu varsayalım bu 2 ayrı factory'i üretmek için bir factory sınıfı yazarız. Factory üreten factory.

### Builder

Üretimi complex olan ve ya birden fazla aşama olan sınıfları üretmek için kullanılan bir patterndir.

### Prototype

Bir nesnenin aynısından bir tane daha oluşturmak istediğimizi varsayalım eğer kopyalamaya çalıştığımız nesnenin private değişkenleri varsa bunları kopyalamamız
mümkün olmuyacak. Bu sorunu aşmak için clone metodu olan bir interface yaratırız ve bu nesnelerde bu interface'i implemente ederiz bu şekilde nesneleri klonlayabiliriz.


## Structural Design Patterns

SDP, nesnelerin ve ya sınıfların daha büyük yapılar inşa etmek için nasıl kompoze edileceği ile alakalıdır.


### Adapter

Adapter patterni kodda ortak bir interface üzerinden işlemlerin yapılması için interface'e uymayan sınıfları interface uydurmak için yazılır.

Diyelim ki bir email gönderen bir kütüphane kullanıcağız, programımızı tamamen bu kütüphanenin sınıfına göre yazmaktansa bir EmailSender interface'i oluştururuz. Daha sonra bu kütüphanenin sınıfını interface'e göre uygulayan bir class(adapter) yazıp uygulamanın geri kalanında bu adapteri kullanırız.
Bu sayede uygulamamızın bir kütüphaneye couple olmamasını sağlarız.

![](https://camo.githubusercontent.com/53c4558d66ea643aa6bbf83ffafc51e0042defaf/68747470733a2f2f6f62736f6c657465646576656c6f7065722e66696c65732e776f726470726573732e636f6d2f323031322f30392f68662d616461707465722e6a7067)


### Bridge

Bridge is a structural design pattern that lets you split a large class or a set of closely related classes into two separate hierarchies—abstraction and implementation—which can be developed independently of each other.

Abstraction (also called interface) is a high-level control layer for some entity. This layer isn’t supposed to do any real work on its own. It should delegate the work to the implementation layer (also called platform).

Bridge patternda, inheritance yerine composition kullanmayı öneriyor.
**The Bridge Pattern can be thought of as part if the Composition over Inheritance Argument.**

### Composite

Composite tasarım deseni, nesneleri ağaç yapısına göre düzenleyerek ağaç yapısındaki alt üst ilişkisini kurmaya yarayan bir desendir.

![](https://raw.githubusercontent.com/yusufyilmazfr/tasarim-desenleri-turkce-kaynak/master/images/composite-uml.png)


### Decorator

Decorator is a structural design pattern that lets you attach new behaviors to objects by placing these objects inside special wrapper objects that contain the behaviors.

Bir sınıfı genişletmek ilk akla gelendir. Ancak inheritance statik bir şeydir runtime esnasında nesnenin davranışı değiştirilemez.

### Facade

Facade Bir alt sistemin parçalarını oluşturan classları istemciden soyutlayarak kullanımı daha da kolaylaştırmak için tasarlanmış tasarım kalıbıdır.

Bir işi yapmak için birden fazla sınıf kullandığımızı bazı sınıf işlemlerinin sırasıyla yapılması gerekildiğini düşünelim. Bu işi yapmak için her defasında sınıfların ilgili metodlarını çağırmamız gerekir bu uzun ve komplex bir işlem olduğunda, bir facade ile bu kompleksliği soyutlayıp kolayca yapmak istediğimiz işi yapabiliriz.

### Flyweight

Kullanım amacı, benzer nesneleri tekrar tekrar oluşturmak yerine ortak bellek alanlarını kullanarak bellek kullanımı azaltmaktır.

Bir factory sınıfımız olduğunu düşünelim bu factory sınında bir map'imiz olsun oluşturduğumuz nesnelerin listesini tutan. Tekrar bir nesne oluşturma isteğinde eğer daha önce istenen tipte nesne oluşturulmuşsa mapten alınıp geri dönderilir aksi taktirde yenisi oluşturulur. Bu sayede bellek kullanımı azaltılır. Tabi ki bunun kullanımı object valueler üzerinde mantıklıdır aksi taktirde her bir nesne ayrı bir kimliği temsil ediyorsa böyle bir yapı kullanmak mantıklı değildir.

### Proxy

Proxy, başka bir nesne için bir placeholder sağlar. Bu proxy asıl nesneye erişimi kontrol eder,  bizimde bu nesneye erişmeden önce ve ya sonra yapmak istediğimiz işlemler varsa bunları yapmamızı sağlar. 