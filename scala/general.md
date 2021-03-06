# General Notes About Scala

## Val, Def, Lazy Val

- Val, değişmeyen bir değer oluşturur.
- Var, mutable bir değişken oluşturur.
- Def, bir metod oluşturur. İçerisindeki kod bloğu yanlızsa çağırıldığı zaman çalışır.
- Lazy val, def ve val arasında bir yerdedir. İlk çağırıma kadar değer hesaplanmaz yani def gibi davranır ancak daha sonraki çağırımlarda değeri neyse onu verir init eden kodu tekrar çalıştırmaz yani val gibi davranır.

```scala
val immutableInt = 5
val mutableInt = 5
def squareMethod(x:Int): Int => x * x
val squareFunction: Int => Int = num => num * num
lazy val wow = squareFunction(6)
```

## Types

- Bir değişkenin tipi yapabileceklerini belirler.
- Metod, fonksiyon parametrelerinin tipleri belirtilmek zorundadır.
- Scala, local olarak type inference yapabilir.
- Any tipini olabildiğince kullanmamak lazım. Neden ?

```scala
val myNumberWithAnyType:Any = 10;
println(myNumberWithAnyType / 2); // COMPILER ERROR!!!
```

- Yukarıdaki örnek compiler hatası verecektir. Çünkü / operatörü(metodu) Any tipi için geçerli değildir.

## Array

- Scalada array indislerine parantezle erişilir süslü ile değil.
- Bu aslında arraylere özgü bir şey değildir. Scalada arraylerde birer sınıftır aslında bir sınıftan türeyen nesneyi parantezlerle çağırınca aslında bir fonksiyon çağırmış oluyoruz.
- myArray(0) === myArray.apply(0) anlamına geliyor.
- myArray(0) = "Selam" ise myArray.update(0, "Selam") haline geliyor.
- Bu sadece arraylere özgü değildir. Eğer sizinde oluşturduğunuz bir sınıfta apply metodu varsa sizde bunu kullanabilirsiniz.

#### Array Oluşturma

```scala
val nums = Array(1, 2, 3); // == Array.apply(1, 2, 3);
println(nums(0)) // RETURNS 1
```

## List

- List, arraylerin immutable halidir scalada.

#### List İşlemleri

```scala
val nums = List(1, 2, 3);
val names = "Serkan" :: "Ali" :: Nil; // List(Serkan, Ali)
val combined = nums ::: names; // List(1, 2, 3, Serkan, Ali)
val namesNewer = "Mehmet" :: names;
```

## Set And Map

- Scalada, Set ve Map'lerin immutable ve mutable versiyonları bulunmaktadır.Farklı traitleri implemente ediyorlar isimleri aynı.
  Default olarak immutable olanı kullanırız. Mutable olanı kullanabilmek için import etmemiz lazım yada import etmeden yolunu yazmalıyız.

```scala
val immSet = Set(1, 2);
println(immSet + 5); // Set(1,2,5)
val mSet = scala.collection.mutable.Set(3, 4);
mSet += 5;
println(mSet); // Set(3, 4, 5)

// MAP

val treasureMap = scala.collection.mutable.Map[Int, String]() // empty
treasureMap += (1 -> "Go to island.")
treasureMap += (2 -> "Find big X on ground.")
treasureMap += (3 -> "Dig.")
println(treasureMap(2))


val romanNumeral = Map(
  1 -> "I", 2 -> "II", 3 -> "III", 4 -> "IV", 5 -> "V"
)
println(romanNumeral(4))

```

## For

```scala
for(i < 0 to 2)
    println(i)
```

- Burada 0 to 2 aslında şudur: (0).to(2);
- Int nesnesinin to adında bir metodu var.Yani scalada tek parametreli fonksiyonların argümanlarını parantez içinde yazmadan da kullanabiliyoruz.

## Operators

```scala
println(5.+(6)) // 11
```

- Scalada operatorler(+,-,/,\*) birer fonksiyondur.

## Classes

```scala
class Rational(n: Int, d: Int) {
  val isDenominatorZero = d != 0;
  require(isDenominatorZero);
  override def toString: String = s"Rational $n/$d"
}
```

- Scalada sınıfların gövdesi constructor fonksiyonudur diyebiliriz.
- Constructor parametreleri yukarıda gösterildiği gibi yazılır.
- require metodu constructor parametrelerimizi kontrol edip bunların doğruluğunu check etmemizi sağlar eğer false olursa içindeki expression exception fırlatır.
- def this şeklinde başka constructorlar oluşturabiliriz ancak bu constructorlar primary constructorı çağırmalıdır.
- Superclass constructorlarını sadece primary constructor çağırabilir.
- +,\*,/,- Gibi metodlar yazıp kullanabiliriz sınıflarımızda.
- Superclass'ta parameterless bir metod varsa bunu türeyen sınıfta val haline getirebiliriz.
- Bir sınıfın metodunu alt sınıfların override etmemesini istiyorsak metodun başına final keywordünü eklemeliyiz, eğer sınıfdan başka bir sınıf türemesin istiyorsak sınıfın başına final keywordü eklemeliyiz.
- Sınıflar arasında == işareti kullanınca sınıfın equals metodunu çağırmış oluruz.

## Abstract Classes

- abstract class MyClass şeklinde yazılır.
- Metodlarının abstract olması için javadaki gibi abstract keywordünü kullanmamız gerekmiyor implementasyonu bulunmuyorsa bunu abstract olarak görür compiler.

## Case Classes

- Case sınıfların karşılaştırması alanlarının eşitliği üzerinden yapılır
- New keywordüne ihtiyaç duymadan nesne oluşturabiliriz.
- Pattern matching ile güçlü bir kullanım sağlanabilir.
- Constructor parametreleri, sınıfın üyeleri haline gelir ve immutabledırlar.
- Kopyalanabilirler.
- toString, equals, apply, unapply metodları ile birlikte gelir.

## Traits

- Traitler, alan ve metod tanımlamalarının yapıldığı, classlarla kullanılabilen bir yapıdır.
- Bir sınıf sadece bir süper sınıfı extends edebilirken birden fazla traiti extends edebilir.
- Traitler, javadaki interfacelere benzetebiliriz.
- Extends ve ya with keywordleri ile sınıflara mix edilebilir.
- Sınıfların sahip olduğu tüm şeyleri yapabilir 2 durum dışında
  1. Constructor parametresi alamazlar.
  2. ???

## Sealed

- Sealed, keywordü sınıf ve ya trait'in nerelerde extend edilebileceğini belirler.
- Implementasyonu ve alt sınıfları kaynak kod ile aynı dosyada olmak zorundadır.
- Compiler bir dosyada tüm implementasyonların yapıldığını bildiği için pattern match gibi kullanımlarda ele alınmayan case var ise önceden uyarır.

## Implicit Class

- Bir sınıfı genişletmek için kullanılır.
- Varsayalım bir kütüphaneden ve ya bağımlılıktan bir sınıf kullanıyorsunuz ve buna bir davranış daha eklemeniz lazım ancak kaynak koda erişiminiz yok implicitlerle sınıfı modifiye etmeden genişletebilirsiniz.

```scala
case class Donut(name: String, price: Double, productCode: Option[Long] = None)

object DonutImplicits {
  implicit class AugmentedDonut(donut: Donut) {
    def uuid: String = s"dnt-${donut.name.toLowerCase}-${donut.productCode.getOrElse(101)}"
  }
}

object Main extends App {
  import DonutImplicits._


  assert(Donut("Vanilla", 1.50).uuid === "dnt-vanilla-101")
  assert(Donut("Vanilla", 2, Some(1)).uuid === "dnt-vanilla-1")
}
```

## Objects

- Nesne, bir sınıf tipinden oluşturulan elemanlara denir, Scalada ayrıyeten Object keywordü vardır. Çeşitli amaçlar için kullanılmaktadır bunlar
  - Singletons (stateless olan sınıflar yerine object)
  - Companion Objects (statik metodlar için)
  - Package Object (sık kullanılan metod ve nesneleri bir paket nesnesi içinde yazarsak herhangi bir prefix kullanmadan kullanabiliriz.)
- Nesneler arası tip dönüşümü sık kullanılır scalada bunu `.asInstanceOf[donusturulecekTip]` metodu ile yapabilirsiniz.
- Nesnenin sınıfını öğrenmek icin `.getClass` metodunu kullanabilirsiniz.
- Bir nesneyi App traiti ile genişletirseniz bu nesne main metodu gibi davranır.
- Companion Object içinde apply metodunu gerekli tanımlamalar ile oluşturursanız o sınıftan `new` keywordünü kullanmadan nesne üretebilirsiniz.

```scala
// Singleton Ornegi
object Utils {
  def printClassOfObject(x: Any): Unit = println(s"class of object is ${x.getClass}")
}

// Companion Object Ornegi
class Pizza(val size: String)
object Pizza {
  def apply(): Pizza = new Pizza("M")
}

// Package Object Ornegi
// Asagidaki ornekle com.serkanerip.business paketi icerisinde tanimli sinif
// ve ya Object'lerde checkEmailIsValid metodunu kullanabilirsiniz herhangi bir paket belirteci olmadan
package com.serkanerip

package object business {
  def checkEmailIsValid(email: String): Boolean = {
    val regex = """(?=[^\s]+)(?=(\w+)@([\w\.]+))""".r
    regex.findFirstIn(email) != None
  }
}
```

## Generic Types

- Bir sınıfa ve ya fonksiyona tip bağımsız işlem yaptırma gücü kazandırır.
- class Stack[A] => Burada A her hangi bir tip olabilir sınıf içindede metodlarda bu tipi kullanabiliriz bu sembol ile.
- 3 Tip Varyans vardır genericlerde.
  1. Covariance: Stack[A+], şeklinde gösterilir. Tip olarak belirlenen sınıfın alt sınıfları ile 2 yönlü ilişkisi olur yani iç Stack[Person] içine Stack[Student]' ta ekliyebiliriz.
  2. Contravariance: Stack[-A] şeklinde gösterilir. Tek yönlü ilişki vardır alt sınıf, süper sınıf tipinde genericlere girebilir ancak tersi olamaz.
  3. Invariance: Stack[A] şeklinde gösterilir. Sadece belirtilen sınıf ve ya primitif neyse o kullanılabilir alt sınıf süper sınıflar dahil olamaz!

## Pattern Matching

- Diğer dillerdeki switch, işlemini ve daha fazlasını yapabilen bir yapı.
- Switchlerden farkı, matchler birer expressiondır, switchler statementtır.
- Sadece bir case çalışır bu yüzden breaklere ihtiyaç yoktur.
- Eğer hiçbir case ile match olmazsa MatchException fırlatır.
- Bir pattern if expressionu ile kombine edilebilir ve buna Guard denir.
- Tip kontrolü ile bir iş yapıyorsak isIstanceOf, asInstanceOf yerine tercih edilebilir.
- Patternlar yukarıdan aşağı doğru execute edilir.
- Matchler, runtime esnasında işlenir ve generic tipler jvmde silinirler bu yüzden örneğin spesifik bir Map tipi için eşleşme kontrolü yapmaya çalışırsak beklenmeyen çıktılar ve hatalarla karşılaşma ihtimaliniz yüksek.

```scala
val myMap: Map[Int, String] =
      Map(1 -> "one", 2 -> "two")

    val matchedTypeInString = (x: Any) =>
      x match {
        case x: Map[Int, Int]    => "Int, Int"
        case x: Map[Int, String] => "Int, String"
        case _                   => "Any"
      }

    println(matchedTypeInString(myMap)) // Int, Int
```

## Namespaces

- 2 adet namespace bulunmaktadır scalada.
  1. values (fields, methods, packages, and singleton objects)
  2. types (class and trait names)
- Aynı namespacete bulunan misal bir field ve metod aynı ismi taşıyamazlar.

## Control Structures

- If, while, for, try, match and function calls.
- Scalada kontrol yapıları bir değer dönderirler.

## Functions

- Scalada first-class fonksiyonlar vardır.
- Fonksiyonları değişkenlere atıyabiliriz, parametre olarak verebiliriz ve return edebiliriz.
- Fonksiyon içinde fonksiyon yazabiliriz.
- Closurelar scalada da vardır.

## Parameterless Methods

- Parametresiz ve side effect bulunmayan metodlarda kullanılmalıdır.
- myArray.length == myArray.length()

## Placeholder Syntax

- Scalada önemli bir özelliktir. Bir çok işlevi vardır. `_`
- Placeholder parametrelerin yerini tutarak gereksiz detayları yazmamamızı sağlar.
- İlk placeholder ilk parametre, ikincisi ikinci parametre ....
- İlk parametreyi 2 defa kullanayım şeklide bir kullanım yapamazsınız.

## Try And Options

- Try ve, optionlarda foreach ile bir sideeffect yapılacaksa None ve Failure olanlar elimine edilir.

**Örnekler:**

```scala
val r = 0 to 100;
var sum = 0;

r.map(_*_); // EQ= r.map(x => x * x)
r.filter(_*_) // EQ= r.filter((x,y) => x * y)
r.reduce((x, y) => x + y / x min y); // Bunu placeholder ile yapamayız.
val f = (_: Int) + (_: Int) // EQ= val f = (x, y) => x + y;
r.foreach(sum += _)
```

## Repeated Parameters

```scala
def main(args: String*) = println(args); // birden fazla String ogesi alabilir.
val arr = Array("What's", "up", "doc?");
echo(arr) // Hata verir cünkü bizden istenen array degil en az bir String nesnesi
echo(arr: _*) // _* Sembolü ile bu arrayi spread edebiliriz. JS deki ...arr gibi
```

## Partial Functions

- PF'ler sadece belirli giriş değerleri için çalışan fonksiyonlardır.
- Pattern mathcing ile çalışırlar.
- PF'ler, fonksiyon tipi parametrelere girebilirler. Çünkü fonksiyonların bir alt tipine girerler.Ancak eğer case ile match olmazsa MatchError fırlatır.
- Type Alias For PF `type ~>[-Input, +Output] = PartialFunction[Input, Output]`

## Collections

[Koleksiyonlar burada anlatılmaktadır.](collection.md)

## Syntax Sugar

```scala

// (->) bir metoddur. Tuple dönderir.
Map(1 -> "one") // Map[Int,String] = Map(1 -> one)
Map(1.->("one")) // Map[Int,String] = Map(1 -> one)

```

## Access Modifiers

1. Private, bir değişken ve ya alanın sadece sınıf ve ya nesne içinden erişilebilmesine izin verir.
2. Protected, ile alanların erişimini sadece alt sınıflara açar.
3. Default, herhangi bir kısıtlama olmadan alan ve metodların erişimini açar. **Diğer dillerdeki public erişim belirtecine denk gelir.**

- Scalada erişim belirteçlerine scope tanımlayabiliyoruz.

```scala
package outside {
  package inside {
    object Messages {
      // inside package'ı bu değişkenere erişebilir.
      private[inside] val Insiders = "Hello Friends"
      // outside package'ı bu değişkenere erişebilir.
      private[outside] val Outsiders = "Hello People"
    }
    object InsideGreeter {
      def sayHello(): Unit =
        // 2 alanada erişebilir
        println(Messages.Insiders + " and " + Messages.Outsiders)
    }
  }
  object OutsideGreeter {
    def sayHello(): Unit =
      // Sadece Outsiders alanına erişebilir.
      println(inside.Messages.Outsiders)
  }
}
```

- **private[this]**: Aynı sınıftan oluşturulan nesneler birbirlerinin private değişken ve ya alanlarına metodları ile erişebilirler. Bunu engellemek için private[this] yani sadece oluşan nesne erişebilir diye belirtmemiz lazım.

## Futures

- Future, içinde yazılan kod bloğunun asenkron olarak başka bir Thread üzerinde çalışmasını sağlar.
- Futurelar, genellikle değer dönderirler.
- Bir future'ın bitmesini bekleyebiliriz yani bloklayabiliriz ana ipliği ama bunu Future kullanmadanda yapabiliriz.
- Callback, metodları kullanabiliriz.
- Future sonunda Success ve ya Failure nesneleri geri dönderir duruma göre.
- Await ile belirli bir süre ana ipliği bekletebiliriz, eğer bu süre içinde iş bloğu tamamlanmaz ise TimeOutException fırlatır.
- Await.result eğer bir hata fırlatılırsa future kod bloğu içinde tekrar fırlatır bu yüzden bunuda try catchlemek zorunda kalırsınız bunun yerine Await.ready kullanabiliriz.
- Future'a eşitlenen değişkenin değeri işlem bitince şöyle bir iç içe yapıya girer:
  - Future -> Success Ve Ya Failure -> Some Or None
- Future sonucunu pattern matching ile kullanabiliriz.
- Bir Future'ın, bir diğerinden sonra başlamasını istiyorsak def ile tanımlayıp birinci future'ın tamamlandığı zaman çalışacak olan kod bloğunun içinde çağırırız.

#### Future içindeki degere erişme

```scala
val myAge = Future {Thread.sleep(1000); 1}
myAge.onComplete(t => {
  println(t.value.)
})
```

#### Future Ex

```scala
    val num1 = Future { Thread.sleep(1000); 1 }
    val num2 = Future { Thread.sleep(5000); 5 }

    val withFlatMapFunc = num1.flatMap(n1 => num2.map(n2 => n1 + n2))
    val withMapFunc = num1.map(n1 => num2.map(n2 => n1 + n2))
    val withForExpr = for (n1 <- num1; n2 <- num2) yield n1 + n2;

    while (!withFlatMapFunc.isCompleted) {}

    println(withFlatMapFunc) // Future(Success(6))
    println(withForExpr) // Future(Success(6))
    println(withMapFunc) // Future(Success(Future(Success(6))))
```

## Reduce, Fold Custom Implementation

```scala
def reduceLeft[A](list: Seq[A], f: (A, A) => A): A = {
    def loop(l: Seq[A], f: (A, A) => A): A = {
      l match {
        case x: Seq[A] if x.size == 1 => x(0)
        case x: Seq[A]                => loop(f(x(0), x(1)) +: x.drop(2), f)
      }
    }

    if (list.isEmpty) throw new InvalidParameterException("ups")

    loop(list, f);
  }

  def reduceRight[A](list: Seq[A], f: (A, A) => A): A = {
    def loop(l: Seq[A], f: (A, A) => A): A = {
      l match {
        case x: Seq[A] if x.size == 1 => x(0)
        case x: Seq[A] =>
          loop(x.dropRight(2) :+ f(x(x.size - 2), x(x.size - 1)), f)
      }
    }

    if (list.isEmpty) throw new InvalidParameterException("ups")

    loop(list, f);
  }

  def foldLeft[A](acc: A)(list: Seq[A], f: (A, A) => A): A = {
    reduceLeft(acc +: list, f);
  }

  def foldRight[A](acc: A)(list: Seq[A], f: (A, A) => A): A = {
    reduceLeft(list :+ acc, f);
  }
```

## Imperative To Functional

#### EX 1

```scala

// IMPERATIVE
def printArgs(args: Array[String]): Unit = {
  var i = 0
  while (i < args.length) {
    println(args(i))
    i += 1
  }
}

// FUNCTIONAL
def formatArgs(args: Array[String]) = args.mkString("\n")
println(formatArgs(args))
```

#### EX2

**FINDING LONGEST LINE:**

```scala
// IMPERATIVE
var maxWidth = 0
for (line <- lines)
  maxWidth = maxWidth.max(widthOfLength(line))

// FUNCTIONAL

val longestLine = lines.reduceLeft(
   (a, b) => if (a.length > b.length) a else b
 )
```

#### EX3

**CHECK ANY NEG EXISTS IN NUMBER LIST**

```scala

// IMPERATIVE
def containsNeg(nums: List[Int]): Boolean = {
    var exists = false
    for (num <- nums)
      if (num < 0)
        exists = true
    exists
}

// FUNCTIONAL
def containsNeg(nums: List[Int]): Boolean = nums.exists(_ < 0)
```

#### EX4

```scala

// IMPERATIVE
def beside(that: Element): Element = {
    val newElementContents = new Array[String](this.contents.length)
    for (i <- 0 until newElementContents.length)
      newElementContents(i) = this.contents(i) + that.contents(i)
    new ArrayElement(newElementContents)
}

def beside(that: Element): Element = {
    new ArrayElement(
      for(
        (line1, line2) <- this.contents.zip(that.contents)
      ) yield line1 + line2
    )
}
```

# Resources

1. https://www.artima.com/pins1ed/
2. http://alvinalexander.com/downloads/scala/Scala-Cheat-Sheet-devdaily.pdf
3. http://twitter.github.io/effectivescala/#Introduction
4. https://danielwestheide.com/blog/the-neophytes-guide-to-scala-part-1-extractors/
5. https://apiumhub.com/tech-blog-barcelona/scala-type-bounds/
6. http://twitter.github.io/scala_school/
7. https://docs.scala-lang.org/style/overview.html
8. https://www.youtube.com/channel/UCSBUwLT9zXhUalKfJrc2q2A
9. https://www.lihaoyi.com/post
