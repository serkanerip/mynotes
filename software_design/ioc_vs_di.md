# IOC vs DI

## IOC

İmperatif programlamada control flow, kodun bir abstraction(module, class, object, function)'ından  bir başkasına geçmesidir.

Örneğin foo diye bir fonksiyonun içinde bar fonsiyonunu çağırdığımızı düşünelim.

Burada control flowun foo -> bar ' a doğrudur.

Ve foo, bar'ı çağırıyorsa ona bağımlıdır.

Burada bağımlılıkta foo -> bar şeklindedir.

Görüldüğü üzere kontrol ve bağımlılık akışı aynı yöndedir.

**foo -> bar**

Tabi bu herzaman böyle olmak zorunda değildir. Bar'ı çağrımak için Foo'nun ona bağımlı olması gerekmiyor. Gerçekte ikisi arasında hiçbir bağımlılık bulunmayabilir. Örneğin, foo fonksiyonu bar'ı callback olarak alabilir çağırmak için. Bu durumda kontrol akışı tekrar foo dan bar'a doğrudur ancak artık foo, bar'a bağımlı değildir.

Yazılım mimarisinde, bağımlılıkları tersine çevirmek yaygın bir pratiktir.

```
Inversion of Control occurs when an abstraction A depends on abstraction B, but the control flows in the opposite direction from B to A.
```

![](https://miro.medium.com/max/400/1*KJ9AVfXaGgmKzvERtRfsZg.png)

Örneğin uygulamanızın domain katmanında IUserRepository adında bir sınıfımız olsun biz domain katmanında bir feature'ı gerçekleştirirken bu interface den oluşan bir nesneyi kullandığımızı düşünelim.

Infrastructure katmanında ise MysqlUserRepository adında IUserRepository interface'ini implemente eden bir sınıf olduğunu düşünelim. Ve biz uygulamamızda bu sınıfı kullanalım.

Bu durumda kontrol akışı:

Domain -> Infrastructure

Bağımlılık akışı:

Infrastructure -> Domain

şeklinde olacaktır. Bu sayede katmanlar arasındaki bağımlılığı azaltmış oluruz. Ve uygulamamızın asıl katmanının(core(application, domain)) diğer katmanlara bağımlı kalmamasını sağlarız.

## Dependency Injection

Uygulamaların bağımlılıklarını direk olarak kendisinin oluşturması test etmeyi ve bağımlılıkları kontrol etmeyi zorlaştırıyor. DI tekniğinde bağımlılıklar constructor ve ya setter metodlarla set edilir.

## Dependency Injection Frameworks

DI teknolojileri, Dependency injection işlemlerini bizim için kolaylaştıran teknolojilerdir.