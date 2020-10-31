- Parametre fonksiyonda kullanıcıdan alınan verinin labelıdır.
- Argument ise fonksiyonda kullanıcdan alınan veridir.

## Lexical Enviroments:

    1. { this } Block Scope
    2. Global Scope <script>this</script>
    3. Function Scope function f(){this;}

- Block lexical enviroment anında oluşturulurken, **fonksiyonlarda fonksiyon çağırıldığı zaman LE oluşturulur**.

```js
let foo = function () {};
let baz = baz;
baz = 1; // pass by value
console.log(baz); // 1
console.log(foo); // Function
```

- Bir constructor fonksiyon çağırıldığında kendi lexical scope'unu(this) oluşturur.

- Her fonksiyon çağırıldığı contexti this objesi olarak alır.
- Eğer belirli bir contextte değilse global contexti alır.
