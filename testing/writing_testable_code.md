# Writing Testable Code


### Notlar

* Bağımlılıklar, parametre ve ya setterlarla inject edilmelidir. Bu şekilde kodun test edilmesi kolaylaşacak ve loosely couple olacak.
* Her zaman referans isteyin, oluşturmayın ve ya global kullanmayın.
* Nesne oluşturma(Object Construction) ve application logic bir birinden ayrılmalıdır.
* Hedefimiz logic barındıran ve ya new operatörleri barındıran sınıflar olmalıdır.
* Uygulamamızın bağımlılık grafındaki leafler, her zaman test edilmesi kolaydır çünkü her hangi bir bağımlılık barındırmazlar.
* Bağımlılıkları referans alarak kolayca izole edebiliriz.
* New operatörlerini yani nesneleri factory, builderlar ile oluşturabiliriz.
* Constructor içinde bağımlılık oluşturmak(create) Single Responsibility prensibini deler.
* Constructor içinde logic yazılmamalıdır.
* Eğer logice ihtiyacımız var ise bu sınıfları oluşturacak creator sınıflar yaratıp oralarda bu işlemleri yapmalıyız.


### Kaynaklar

- http://misko.hevery.com/2008/09/10/where-have-all-the-new-operators-gone/
- http://misko.hevery.com/2008/08/29/my-main-method-is-better-than-yours/
- http://misko.hevery.com/2008/07/08/how-to-think-about-the-new-operator/
- http://misko.hevery.com/2008/10/21/dependency-injection-myth-reference-passing/
- http://misko.hevery.com/attachments/Guide-Writing%20Testable%20Code.pdf
- https://www.dashdevs.com/blog/writing-testable-code-main-rules/