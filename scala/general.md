# General Notes About Scala

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

# Resources

1. https://www.artima.com/pins1ed/
