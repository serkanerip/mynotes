# This

Javascriptte, scoping işlemi dynamic olduğu için bir fonksiyonun erişebildiği scope çalışma zamanında belirlenir. Kodun yazıldığı zaman belirlenmez.

Bu yüzden this anahtarıda yazıldığı yeri referans etmez çağırıldığı zaman çağırıldığı yeri referans eder.

**Ancak lambda functionlar, lexical this yani statik this kullanırlar. Yani nerede yazıldıysa ona göre scope'unu belirler.**

Ex:

```js
const foo = {
  x: 5,
  showX: function () {
    setTimeout(function () {
      console.log(this.x);
    }, 1000);
  },
  showXWithArrow: function () {
    setTimeout(() => console.log(this.x, "arrow"), 1000);
  },
};

foo.showX(); // undefined
foo.showWithArrow(); // 5, arrow
```

**Eğer objenin içinde direk olarak lambda function kullanılırsa globale işaret eder.**

Ex:

```js
x = 'hello from global',
const foo = {
    x: 'hello from foo object',
    showX: () => console.log(this.x),
    showXV2: () => console.log(foo.x),
    showXV3: function(){
        const inner = () => console.log(this.x);
        inner();
    },
}

foo.showX(); // hello from global
foo.showXV2(); // hello from foo object
foo.showXV3(); // hello from foo object
```

Diğer Durumlar:

1. Eğer fonksiyon new keywordü ile çağırılırsa fonksiyon için yeni bir this {} oluşturulur.
2. Eğer fonksiyon bind edilirse ve ya apply edilirse, parametre olarak aldığı objeyi **this** olarak alan bir fonksiyona dönüşür.

   fooBinded = foo.bind({a:1});

3. Eğer fonksiyon bir contextle çağırılmışsa çağırıldığı contexti this olarak alır.

   myObj.foo();

4. Eğer bunların hiçbiri değilse global contexti this olarak alır.
