# General Notes About Scala

## Val, Def, Lazy Val

- Val, değişmeyen bir değer oluşturur.

```scala
val myNum = {
    println("hi");
    1337
}
```

- Burada myNum değişkeninini kullanmasakta println satırı çalışır ve 1337 değeri atanır myNum'a.
- Def ile belirlenen değişkenlerin bloğu çağırıldığı zaman çalışır.
- Lazy val, def ile val arasında bir tiptir. İlk çağırıldığında def gibi çalışır ancak sonraki çağırımlarında val gibi davranır.

## Types

- Bir değişkenin tipi yapabileceklerini belirler.
- Any tipini olabildiğince kullanmamak lazım. Neden ?

```scala
val myNumberWithAnyType:Any = 10;
println(myNumberWithAnyType / 2); // COMPILER ERROR!!!
```

- Yukarıdaki örnek compiler hatası verecektir. Çünkü / operatörü(metodu) Any tipi için geçerli değildir.

## Array

#### Array Oluşturma

```scala
val nums = Array(1, 2, 3);

val numsV2 = Array.apply(1, 2, 3);
```

#### Array Elemanları Üzerinde İşlemler

```scala
myArray(0); // arrayin sıfırıncı elemanı döner.
```

- Scalada array indislerine parantezle erişilir süslü ile değil.
- Bu aslında arraylere özgü bir şey değildir. Scalada arraylerde birer sınıftır aslında bir sınıftan türeyen nesneyi parantezlerle çağırınca aslında bir fonksiyon çağırmış oluyoruz.
- myArray(0) === myArray.apply(0) anlamına geliyor.
- myArray(0) = "Selam" ise myArray.update(0, "Selam") haline geliyor.
- Bu sadece arraylere özgü değildir. Eğer sizinde oluşturduğunuz bir sınıfta apply metodu varsa sizde bunu kullanabilirsiniz.
-

```scala

object Main {
  def main(args: Array[String]): Unit = {
    println(t(1)); // RETURNS 1
  }
}

class Test {
  def apply(i: Int):Int = i;
}
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

- Burada 0 to 2 aslında şudur -> (0).to(2);
- Int nesnesinin to adında bir metodu var.Yani scalada tek parametreli fonksiyonların argümanlarını parantez içinde yazmadanda kullanabiliyoruz.

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
- Sınıf parametrelerine, sınıfın metodları içinden erişemeyiz. Bunları bir field'a assign etmeliyiz.
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
- Fonksiyonları değişkenlere atıyabiliriz, parametra olarak verebiliriz ve return edebiliriz.
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

**Örnekler:**

```scala
val r = 0 to 100;
var sum = 0;

r.map(_*_); // EQ= r.map(x => x * x)
r.reduce(_*_) // EQ= r.filter((x,y) => x * y)
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

## Collections

- List'lerde head listenin ilk elemanını tail ise geriye kalan elemanları işaret eder.

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
