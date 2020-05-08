# Collections

## Mutable Collections Diagram

![](https://docs.scala-lang.org/resources/images/tour/collections-mutable-diagram.svg)

## Immutable Collections Diagram

![](https://docs.scala-lang.org/resources/images/tour/collections-immutable-diagram.svg)

**Maviler: Trait, Siyahlar: Class**

Scalada görüldüğü üzere bir sürü collection tipi vardır. Collection sınıflarının hem mutable hem immutable implementasyonları bulunmaktadır. Scala default olarak **immutable** olanları import eder.

**Traversable trait'i Scalanın 2.13.0 versiyonunda deprecate edilmiştir ve artık hiyerarşinin başında Iterator var.**

```scala
  @deprecated("Use Iterable instead of Traversable", "2.13.0")
  type Traversable[+A] = scala.collection.Iterable[A]
  @deprecated("Use Iterable instead of Traversable", "2.13.0")
  val Traversable = scala.collection.Iterable
```

Traitlere bir göz atalım öncelikle.

## Traits

### Iterable

Sadece bir soyut metodu bulunur bu da **iterator** metodudur. Foreach, metodu bu trait'te implemente edilmiştir. Sadece bazı alt sınıflar bu metodu override etmiştir daha performanslı olmaları için.

#### Somut Metodlar

**++:**: Concat metodu için bir kısaltmadır diğer dillerdeki operatör gibi düşünülebilir.

Sol operandın elemanlarına sağ operandın elemanlarını ekler ve yeni bir iterable koleksiyonu dönderir.

Her iki operandda Iterable sınıfını kalıtmalıdırlar.

```scala
test("++") {
    val x: Seq[Int] = Seq(1, 2, 3);
    val y: List[Int] = List(4, 5);

    assert(x ++ y == List(1, 2, 3, 4, 5))
  }
```

**addString**: Koleksiyondaki tüm elemanları bir string builder içine ekler.

Overload edilmiş halleriyle beraber 3 farklı şekilde kullanılabilir.

İki parametreli versiyonu aynı şekilde ilk parametre bir string builder ikinci parametre ise bir seperator.

Dört parametreli versiyonunda, oluşacak stringin başına, sonuna ve elemanlar arasına seperator koymamıza yarıyor.

```scala
test("addString") {
    val x: Seq[Int] = Seq(1, 2, 3);
    val ayrac: String = ":"

    val sb: StringBuilder =
        x.addString(new StringBuilder)
    val sbV2: StringBuilder =
        x.addString(new StringBuilder, ayrac)
    val sbV3: StringBuilder =
        x.addString(new StringBuilder, "<", ayrac, ">")

    assert(sb.toString === "123")
    assert(sbV2.toString == "1:2:3")
    assert(sbV3.toString == "<1:2:3>")
}
```

**collect**: Koleksiyonun bütün elemanlarına parametre olarak verilen parçalı fonksiyonu uygular ve yeni bir koleksiyon dönderir.

```scala
test("collect") {
    case class User(name: String, age: Int)
    val users = Iterable(
      User("Serkan", 21),
      User("Ali", 44),
      User("Marrie", 25)
    )
    val yirmiUcYasindanKucuklerinIsimleri = users.collect({
      case User(name, age) if age < 23 => name
    });
    assert {
      yirmiUcYasindanKucuklerinIsimleri == Iterable("Serkan")
    };
}
```

Burada anonymous bir partial function verdik parametre olarak tipi User => String olan. Sadece yaşı yirmi üçten küçük olanlar için bir case'imiz olduğu için büyük olanlar yeni listede olmayacaklar.

**collectFirst**: Collect metodunun özel bir halidir, ilk eşleşmede case sonucunu Option tipinde dönderir.

```scala
  test("collectFirst") {
    val integerBulunanListe = Iterable("A", true, Vector(), 1);
    val integerBulunmayanListe = Iterable("A", true, Vector(), 5.5f);

    val integerlariBul: PartialFunction[Any, Int] = { case x: Int => x }

    val sonuc = integerBulunanListe.collectFirst(integerlariBul);
    val sonuc2 = integerBulunmayanListe.collectFirst(integerlariBul);

    assert(sonuc.getOrElse(-1) == 1)
    assert(sonuc2.getOrElse(-1) == -1)
}
```

```scala

```
