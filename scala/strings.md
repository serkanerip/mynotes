# Strings In Scala

## Metodlar

**take**: baştan n tane karakteri alır.

```scala
    val myStr: String = "Night Watch"
    println(myStr.take(3)) // RET: Nig
```

**tail**: İlk karakter hariç diğer tüm karakterleri alır.

```scala
val myStr: String = "Night Watch"
println(myStr.tail) // RET: ight Watch
```

**drop**: Tüm karakterleri al baştaki n tanesi hariç

```scala
val myStr: String = "Night Watch"
println(myStr.drop(3)) // RET: ht Watch
```

**count**: Pametre olarak filtre görevi gören bir fonksiy onalır ve filtreye uyan karakter sayısını dönderir.

```scala
val myStr: String = "Night Watch"
println({
  val myFilter: Char => Boolean =
    ch => ch == 'h'
  myStr.count(myFilter)
}) // RET 2
```
