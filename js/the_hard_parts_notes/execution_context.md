# Execution Context

Her fonksiyon çağırımında yeni bir execution context oluşur.

- Execution Context 2 aşamada oluşur.
  - Creation Stage:
    - Create the scope chain
    - Create variables, functions and arguments
    - Determine value of this
  - Activation / Code Ex. Stage:
    - Assign values, references for functions
    - interpret and exec code

Execution contexti 3 propertyli bir nesne olarak tanımlayabiliriz.

```js
executionContextObj = {
  scopeChain: {
    /* variableObject + all parent execution context's variableObject */
  },
  variableObject: {
    /* function arguments / parameters, inner variable and function declarations */
  },
  this: {},
};
```

#### VariableObject:

VariableObject, fonksiyonun argümanları, local değişken ve fonksiyonlarını tutan nesnedir.

#### ScopeChain:

ScopeChain, erişebildiği değişkenler zinciridir. EC. içinde sadece burada tanımlı değişkenler ve fonksiyonlar kullanılabilir. Parent EC. ve Global EC. inherit eder.

Ex:

```js
function one() {
  two();
  function two() {
    three();
    function three() {
      alert("I am at function three");
    }
  }
}
one();
```

Scope Chains:

1. one => [[one() VO] + [Global VO]]
2. two => [[two() VO] + [one() VO] + [Global VO]]
3. three => [[three() VO] + [two() VO] + [one() VO] + [Global VO]]

Görüldüğü üzere child fonksiyon parentlarında VO'sunu scope'unda barındırır.

#### This:

This, çağırıldığı anda hangi context içinde çağırıldıysa ona işaret eder.

Ex:

```js
x = "global";
function foo() {
  console.log(this.x);
}
const baz = { x: "baz" };

foo(); // RET: global, öncesinde bir .(dot notation) yani bir objeye ait olmadığı için this, global execution contexte işaret eder.

baz.showX = foo;

baz.showX(); // RET: baz
```

## Hoisting Hakkında

- Fonksiyonlara ve var ile oluşturulan değişkenlere implementasyondan önce erişebilmemizin sebebi Code Execution Stage'den önce Creation Stage'de değişkenler ve fonksiyonlar oluşturulur.
- Ancak değişkenlerin sadece labelları oluşturulur değerleri undefined olur. Alakalı satıra gelince atama gerçekleşir.
- Aynı isimde bir fonksiyon ve var değişkeni var ise hoistingde bu fonksiyon olarak görünür.
- Hoisting aşamasında label check edilir eğer oluşturulmuşsa daha önceden devam eder.

## Clousure Hakkında

- Execution Context'in mantığından anladığımız üzere bir contextin memory nesnesi VO lardır.
- Direk olarak bu VO nesnesine kodla erişimimiz yoktur.Ancak scope chain sayesinde erişebiliyoruz.
- Biz bir fonksiyon çağırdığımızda ve o fonksiyon return ettikten sonra o VO ya artık erişimimiz olmamaktadır. Çünkü onun VO'suna ancak child contextler erişebiliyordu.
- Bizde fonksiyon içinde fonksiyon dönderince parent fonksiyon bitmiş olsada onun oluşturduğu VO child fonksiyonun scope chainine eklenmiş olacak. Bu sayede oluşan child fonksiyon hala parent fonksiyonun VO'suna erişebilecek.

### Ex

```js
function GenerateIterator(arr) {
  let index = 0;
  return {
    next: () => (arr.length === index ? null : arr[index++]),
    prev: () => (0 === index ? null : arr[--index]),
  };
}

const arr = [1, 2, 3, 4];
const arrIterator = GenerateIterator(arr);
console.log(arrIterator.next()); // 1
console.log(arrIterator.next()); // 2
```

## Kaynaklar

- http://davidshariff.com/blog/what-is-the-execution-context-in-javascript/
