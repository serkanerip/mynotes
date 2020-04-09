# Pseudoclassical Inheritance

Constructor fonksiyonları ve prototype'ı kullanarak yapılır.

Prototype üzerinde yapılan değişiklik tüm childları etkiler bu yüzden avantaj ve dezavantajları vardır.

Childe özel olan propertyler prototipte değil childda tanımlanmalıdır.

## Ex

```js
const Mammal = function (name = "noname") {
  this.name = name;
};

const Human = function HumanConstructor(name) {
  Mammal.call(this, name);
  this.maxTeeth = 32;
};

const Male = function (name) {
  Human.call(this, name);
};

Mammal.prototype = {
  breath: () => console.log("I am breating..."),
};

Human.prototype = Object.create(Mammal.prototype);
Human.prototype.constructor = Human;
Human.prototype.sayName = function () {
  console.log(`Nave min, ${this.name}`);
};

Male.prototype = Object.create(Human.prototype);
Male.prototype.special = function () {
  console.log("Only males can do this.");
};
Male.prototype.constructor = Male;

const m1 = new Male("Serkan");

m1.special();
m1.sayName();
m1.breath();
```
