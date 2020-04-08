# Fundamentals

## Unary Operatörler:
*  Bir operatör kullanıldığında statementları expression'a çevirir.
### void
* Discards a return value of an expression.
### delete: 

* Deletes specific index of an array or specific property of an object. (Eğer obje nesnesi configurable olarak ayarlanmamışsa silinemez.)
* Fonksiyon ve değişkenlerde işe yaramaz.
* Array elemanı silinince orası bir delik olarak kalır. Array uzunluğuna baktığımızda hala aynıdır.Bu iyi bir yöntem değildir!

### (+)
  * Number olmayan veri tiplerini number'a çevirir.
  * Bunların sonucu NaN ve ya Infinity olabilir.
  * +function(){}, sonuç olarak NaN verir, ama fonksiyonda çalışır.
  * +function(){}() şekline çevirirsek fonksiyon çalışır ter temiz bir IIFE'miz olur.
  * Eğer bir obje ile toplanırsa NaN verir ancak objede valueOf anahtarı bulunuyorsa bu da bir değer döndürüyorsa işleme alır. 
```js
$: 1+{
  valueOf: function(){
    return 5
  } // returns 6
}
```
### (!)
* Operandı boolean tipine çevirir negate etmeden önce.
* null, NaN, 0, undefined, boş string bunların boolean karşılıkları false'dır.
* O yüzden ! operatörüyle kullanınca true'ya çeviriyor doğal olarak.
* Bu yüzden misal bir nesnenin undefined ve ya null olmadığını kontrol için ! kullanırsak tam tersi bir etki alırız.
* Misal foo bizim nesnemiz. if(!!foo) -> eğer foo null, nan, 0, undefined ve ya boş string değil ise
* Dikkat edilmeli stringlerde değişken tanımlıda olsa eğer boş ise bu yinede onun boolean karşılığının false olmasından kurtamıyor.


# Kaynakça
* https://scotch.io/tutorials/javascript-unary-operators-simple-and-useful