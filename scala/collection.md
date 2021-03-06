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

**++**: Concat metodu için bir kısaltmadır diğer dillerdeki operatör gibi düşünülebilir.

Sol operandın elemanlarına sağ operandın elemanlarını ekler ve yeni bir iterable koleksiyon dönderir.

Her iki operandda Iterable sınıfını kalıtmalıdır.

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

**copyToArray**: Bir iterable koleksiyonun elemanlarını bir Array'e kopyalamak için kullanılır.

1. Bir Parametreli: Koleksiyondaki tüm elemanları array'e kopyalar.
2. İki Parametreli: Ek olarak array'in hangi indisinden başlanılacağını belirtebilirsiniz.
3. Üç Parametreli: İki parametreliye ek olarak, kaç adet elemanı kopyalamak istediğinizi belirtirsiniz.

```scala
test("copyToArray") {
    val list = Iterable(1, 2, 3, 4, 5, 6);
    val exArr: Array[Int] = Array.fill(6) { 0 }; // 6 adet 0'a sahip bir array olusturur.

    // tek parametre
    list.copyToArray(exArr)
    assert(exArr.toList === list)
    // iki parametre
    list.copyToArray(exArr, start = 3)
    assert(exArr.toList === List(1, 2, 3, 1, 2, 3))
    // üç parametre
    list.copyToArray(xs = exArr, start = 0, len = 6)
    assert(exArr.toList === list)
}
```

**corresponds**: İki liste elanlarını ikili şekilde sırayla, boolean dönderen bir predicate fonksiyona koyar ve eğer tüm hepsi true dönerse sonuç true olur.

**Eğer her iki listenin uzunluğu eşit değilse false döner.**

```scala
  test("corresponds") {
    val kelimeUzunluklari = Iterable(1, 3, 5, 7, 9);
    val kelimeler = Iterable("a", "aaa", "a" * 5, "a" * 7, "a" * 9);

    val predicate = (x: Int, y: String) => y.length == x;

    val result: Boolean =
      kelimeUzunluklari.corresponds(kelimeler)(predicate)

    assert(result)
}
```

Burada `kelimeleUzunluklari` koleksiyonumuzda kelime uzunlukları girdik diğer koleksiyona ise stringler girdik.

Bir predicate fonksiyonu yazdık bu fonksiyon bir int ve bir String alıyor. Çünkü ilk listemiz int tipinde, diğeri string tipinde. Daha sonra corresponds ile dönüş değerini aldığımızda true olarak dönüyor çünkü kelime uzunluklarını ilk listeye uygun bir şekilde yaptım.

**count**: Array elemanları üzerinde bir test fonksiyonu çalıştırır ve kaç tanesinin true olduğunu dönderir.

```scala
test("count") {
    val oddsBetweenOneToThirteen = Iterable(1, 3, 5, 7, 9, 11, 13);

    val numaraOndanBuyukMuTestEt: Int => Boolean =
      num => num > 10;

    assert(oddsBetweenOneToThirteen.count(numaraOndanBuyukMuTestEt) == 2)
}
```

**drop**: Parametre olarak bir integer alır buna n diyelim, listedenin başındaki n tane eleman hariç diğer tümünü geri dönderir.

```scala
test("drop") {
    val oneToThree = Iterable(1, 2, 3)
    assert(oneToThree.drop(1) == List(2, 3))
}
```

**dropRight**: Parametre olarak bir integer alır buna n diyelim, listedenin sonundaki n tane eleman hariç diğer tümünü geri dönderir.

```scala
test("dropRight") {
    val oneToThree = Iterable(1, 2, 3)
    assert(oneToThree.dropRight(1) == List(1, 2))
}
```

**dropWhile**: Parametre olarak bir test fonksiyonu alır ve bu fonksiyon **ilk defa false alana kadar**, true dönen elemanları atıp geriye kalanların listesini dönderir.

```scala
test("dropWhile") {
    val randomNumbers = Iterable(1, 3, 5, 7, 0, 2, 4, 9)
        val ciftSayiBulanaKadarDusur: Int => Boolean =
            x => x % 2 != 0
        assert {
            randomNumbers.dropWhile(ciftSayiBulanaKadarDusur) == List(0, 2, 4, 9)
        }
    }
```

**empty**: Aynı iterable koleksiyonun boş olanını dönderir.

```scala
test("empty") {
    val randomNumbers = Iterable(1, 3, 5, 7, 0, 2, 4, 9)

    assert(randomNumbers.empty === List())
}
```

**exists**: Koleksiyonda, test fonksiyonumuzu true dönderen eleman var mı kontrolünü yapar.

```scala
test("exists") {
    val randomChars = Iterable('a', 'b', 'e', 'f', 'G')
    val listedeBuyukHarfVarMi: Char => Boolean = ch => ch.isUpper;

    assert(randomChars.exists(listedeBuyukHarfVarMi))
}
```

**filter**: Koleksiyonda, filtreleme yapmamızı sağlar işimize yarıyan elemanları liste olarak alabiliriz.

```scala
test("filter") {
  val randomChars = Iterable('a', 'b', 'e', 'f', 'G', 'K')
  val buyukHarfleriGetir: Char => Boolean = ch => ch.isUpper;
  assert(randomChars.filter(buyukHarfleriGetir) === Iterabl('G', 'K'))
}
```

**filterNot**: Filter, metodunun tersi etki yapar, test fonksiyonumuz sonucunda eşleşmeyen elemanları dönderir.

```scala
test("filterNot") {
  val randomChars = Iterable('a', 'b', 'e', 'f', 'G', 'K')
  val kucukHarfleriGetir: Char => Boolean = ch => ch.isUpper == false;
  assert(randomChars.filterNot(kucukHarfleriGetir) === Iterabl('G', 'K'))
}
```

**map**: Bir koleksiyon elemanları üzerinde işlem yaparak yeni bir koleksiyon elde etmek istediğimizde kullanırız.

Aşağıda ki örnekte kelimelerin bulunduğu koleksiyondan kelimelerin uzunlunlarının bulunduğu bir koleksiyon elde ettik.

```scala
test("map") {
  val randomWords = Iterable("foo", "ok", "test")
  val kelimeninUzunlugunuAl: String => Int =
    word => word.length
  val kelimelerinUzunluklari = randomWords.ma(kelimeninUzunlugunuAl)
  assert(kelimelerinUzunluklari === Iterable(3, 2, 4))
}
```

**flatten**: Bir koleksiyon içinde, koleksiyonlar var ise ve bu iç koleksiyonun elemanlarının hepsini dışarı istediğimiz zaman kullanılır.

İç koleksiyonlar farkı uzunluklarda ve farklı tiplerde olabilirler.

```scala
  test("flatten") {
    val notes = Iterable(
      Map("Ali" -> 15),
      Map("Veli" -> 85)
    )
    val result = notes.flatten

    assert(result === Iterable(("Ali", 15), ("Veli", 85)))
  }
```

### Map (Dictionary, Associative Array)

Anahtar, değer ilişkileri tutan koleksiyonlara verilen isimdir. Scalada Map'lerin elemanları birer tupledır. Anahtarlar benzersizdir bir anahtara ait iki değer olamaz?. Type Unsafe(Any) kullanmadığınız taktirde anahtarlar için bir tip ve değerler için bir tip belirleyebilirsiniz bunlar aynı da olabilirler.

#### Genel Kullanım

```scala
  val nameAndAgeMap: Map[String, Int] = Map(
    "Serkan" -> 21,
    "Gülçin" -> 33,
    "Yahya" -> 41
  )

  println(nameAndAgeMap get "Serkan") // Some(21)
  println(nameAndAgeMap get "Ali") // None
  println(nameAndAgeMap getOrElse ("Ali", "yok")) // yok
  println(nameAndAgeMap contains "Yahya") // true
  println(nameAndAgeMap isDefinedAt "Ali") // false

  println(nameAndAgeMap.keys) // Set(Serkan, Gülçin, Yahya)
  println(nameAndAgeMap.values) // Iterable(21, 33, 41)
  println(nameAndAgeMap + ("Ali" -> 11)) // Ali'li map
  println(nameAndAgeMap - "Yahya") // Yahya'sız map

  nameAndAgeMap.foreach({
    case (name, age) => println(s"$name, $age yasinda")
  }) // Serkan, 21 yasinda, Gülçin 33 yasinda, Yahya 41 yasinda
```

Maplerin genel metodları yukarıda kullanılanlardır. Gördüğünüz üzere bir anahtarın değerine eriştiğimiz zaman bize Option tipinde döner null safety sağlamak amacıyla. Kullandığımız +, ve - operatörleri bizim mapimizin içeriğini değiştirmez iki mapi toplar ve yeni bir map dönderir.

#### Bazı Mutable İşlemler

```scala
  val nameAndAgeMap = mutable.Map(
    "Serkan" -> 21,
    "Gülçin" -> 33,
    "Yahya" -> 41
  )
  nameAndAgeMap += ("Ali" -> 11) // ekleme
  nameAndAgeMap -= "Yahya" // çıkarma
  nameAndAgeMap ++= Map("Sedat" -> 44, "Necip" -> 24)
  println(nameAndAgeMap isDefinedAt "Ali") // true
  println(nameAndAgeMap isDefinedAt "Yahya") // false
  println(nameAndAgeMap isDefinedAt "Sedat") // true
  println(nameAndAgeMap isDefinedAt "Necip") // true

  nameAndAgeMap.update("Ali", 33);
  assert(nameAndAgeMap.get("Ali") == Some(33))
```

Peki hangi Map'i tercih etmeliyim ?

1. Anahtarlara göre sıralı bir map istiyorsanız **SortedMap** kullanmak işinize yarar.
2. Ekleme sırasına göre sıralanmasını istiyorsanız **LinkedHashMap** kullanmak işinizi görür.
3. Eklenme sırasının tersine göre sıralanmasını istiyorsanız **ListMap** kullanmak işinizi görür. Çünkü elemanları başa ekler.

Burada sadece 3 farklı implementasyondan bahsettim ancak daha bir çok çeşidi mevcuttur. Concurrent işlemlerde kullanmak için de ayrı implementasyonlar mevcut.

### Seq (Sequence)

Sıralı elemanları bulunduran koleksiyon tipidir. Bir çok alt sınıfa sahiptir.İki alt traite sahiptir bunlar LinearSeq ve IndexedSeq'tir bunlar yeni metodlar eklemezler fakat farklı performans karakteristikleri vardır. Eğer hangisini kullanacağınızın bir önemi yok ise direk Seq tipinde oluşturabilirsiniz.

#### IndexedSeq

İndis tabanlı erişimlerin ve güncellemelerin çok olduğu yerde performanslı çalışır.

Range, String, Vector ...

#### LinearSeq

İndis tabanlı erişimlerde ve güncellemelerde yavaştır çünkü Linked List benzeri bir veri yapısına sahiptir. Tail, head ve isEmpty metodlarında performanslılardır.

List, Queue, Stack, Stream ...

### Set

Set, benzersiz elemanlara sahip koleksiyon tipidir. Bir koleksiyon ile diğerinin ortak elemanlarını ve ya farklı elemanlarını alma gibi işlemler için kullanılır genelde.

Aynı elemanı eklemeye kalkışırsanız hata almazsınız ancak bir değişiklikte olmaz.

1. **SortedSet**, elemanların sıralı olmasını istediğimiz durumlarda kullanabiliriz.
2. **BitSet**, SortedSet'ten türer. Int tipinde negatif olmayan elemanları barındırmak için yapılmıştır.

#### Genel Kullanım

```scala
val randomNumbers = scala.collection.mutable.Set[Int]()
set += 1 // ekleme
set -= 1 // cikarma
set += (2, 3) // coklu ekleme
set.contains(1) // elemanın kümede olup olmadığını kontrol etmek için
println(set + (3, 4)) // 2, 3, 4
```

# Kaynaklar

1. https://www.lihaoyi.com/post/BenchmarkingScalaCollections.html
2. https://alvinalexander.com/scala/how-to-choose-collection-class-scala-cookbook/
