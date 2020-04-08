## React


### Jsx Dönüşümü

```html
<h1 foo="bar">Hello, JSX</h1>
```
```js
React.createElement('h1', {foo: 'bar'}, 'Hello, JSX');
```
```js
    {
        type: 'h1',
        props: {
            foo: 'bar',
            children: 'Hello, JSX'
        },
        key: null,
        ref: null,
        $$typeof: Symbol.for('react.element'),
    }
```

* JSX, aslında bir fonksiyon çağırımıdır(createElement)
* Oda geriye yukarıdaki gibi bir obje döndürür.
* typeof anahtarı güvenlik amaçlı eklenen bir semboldür. React bu anahtarı kendi oluşturduğuna, dışarıdan injection atakları ile eklenmediğine emin olmak için kullanır.
* React'te {} içinde expression kullanıp sonucunu text olarak parse edersiniz. Html kodu olarak doma enjekte etmez.
* Eğer type html elementlerinden biri değilse onunda js objesini oluşturur böyle döngüyle iç içe child kalmıyana kadar ilerler.