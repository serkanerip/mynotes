# General Notes About Scala

## Val, Def, Lazy Val

- Val, değişmeyen bir değer oluşturur.

```scala
val myNum = {
    println('hi');
    1337
}
```

- Burada myNum değişkeninini kullanmasakta println satırı çalışır ve 1337 değeri atanır myNum'a.
- Def ile belirlenen değişkenlerin bloğu çağırıldığı zaman çalışır.
- Lazy val, def ile val arasında bir tiptir. İlk çağırıldığında def gibi çalışır ancak sonraki çağırımlarında val gibi davranır.

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

## Control Structures

- If, while, for, try, match and function calls.
- Scalada kontrol yapıları bir değer dönderirler.

## Functions

- Scalada first-class fonksiyonlar vardır.
- Fonksiyonları değişkenlere atıyabiliriz, parametra olarak verebiliriz ve return edebiliriz.
- Fonksiyon içinde fonksiyon yazabiliriz.
- Closurelar scalada da vardır.

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

# Resources

1. https://www.artima.com/pins1ed/
