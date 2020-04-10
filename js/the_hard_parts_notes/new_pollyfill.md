# New Pollyfill

```js
function newPollyfill(parent) {
  let emptyObj = {};
  const parentPrototype = parent.prototype;
  emptyObj.__proto__ = parentPrototype;
  parentPrototype.constructor.call(emptyObj);
  return emptyObj;
}

const custStudent = newPollyfill(Person);

console.log(custStudent instanceof Person); // true
```


New keywordünün yaptığı işlemleri basitçe anlatabilmek için bir pollyfill.

* Fonksiyon new keywordü ile çağırılır.
* Boş bir nesne oluşturulur.
* Bu nesnenin __proto__ değişkeni fonksiyonun prototype'ı olur.
* Fonksiyonun constructor methodu call fonksiyonu ile boş nesne üzerinde işlemlerini yapar.
* Ve geriye oluşturduğumuz boş nesne dönderiliriz.