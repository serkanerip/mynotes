# Closures

**Execution Context notunda daha iyi ve detaylı anlatımı mevcut.**

Clousure backpack'e sahip fonksiyonlar demektir. Bir fonksiyon içinde değişken kullanıldığında kendi local değişkenlerine bakar bulamazsa onu çağıran fonksiyonda(globalde bir fonksiyondur) arar.

Closure fonksiyonlar ayrıyetten local dışında veri tutabilen(backpack) fonksiyonlardır ve bu veriler silinmez fonksiyonun işi bitince.

- Fonksiyon içinde fonksiyon döndürünce içerdeki fonksiyon outer fonksiyonun local memorysini alır ve bunu hatırlayabilir statefull bir fonksiyon elde etmiş oluruz.
- Bu local memory [[scope]] isimli gizli property'de saklanır.

## Ex 1

```js
const useState = function (data = null) {
  return function () {
    return [() => data, (_data) => (data = _data)];
  };
};

const [data, setData] = useState({ c: 1 })();
const [name, setName] = useState("")();

setData({ ...data(), a: 5 });
setName("Serkan");

console.log(data()); // {c:1, a:5}
console.log(name()); // Serkan
```

### Ex2: Bir Iterator Class Vs Closures

```js
class GenerateIteratorV2 {
  constructor(arr) {
    this.index = 0;
    this.arr = arr;
  }

  next() {
    return this.arr.length === this.index ? null : this.arr[this.index++];
  }
  prev() {
    return 0 === this.index ? null : this.arr[--this.index];
  }
}

// clousure version
function GenerateIterator(arr) {
  let index = 0;
  return {
    next: () => (arr.length === index ? null : arr[index++]),
    prev: () => (0 === index ? null : arr[--index]),
  };
}

const arr = [1, 2, 3, 4];
const arrIterator = GenerateIterator(arr);
const arrIteratorV2 = new GenerateIteratorV2(arr);
console.log(arrIterator.next()); // 1
console.log(arrIterator.next()); // 1
console.log(arrIteratorV2.next()); // 2
console.log(arrIteratorV2.next()); // 2
```

### Kaynaklar

- https://www.freecodecamp.org/news/deep-dive-into-scope-chains-and-closures-21ee18b71dd9/
- https://medium.com/dailyjs/i-never-understood-javascript-closures-9663703368e8
