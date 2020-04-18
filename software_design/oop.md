# Object Oriented Programming

## OOP Nedir ?

OOP, veri ve fonksiyonların kombinasyonudur diyebiliriz.Ancak şu tipte bir kombinasyondur o.f(), f(o) tipinde değil yani fonksiyonlar bir nesneye bağlılar nesneleri parametre olarak almıyorlar.

Bir başka cevap olarakta gerçek dünyanın bir modellemesidir de denilir.

OOP'nin 3 sihirli kelimesi vardır bunlar:

1. Encapsulation
2. Inheritance
3. Polymorphism
4. Abstraction

## Encapsulation Nedir ?

Sınıflar, nesneleri sayesinde dış dünyaya açılırlar.

Encapsulation, bu dış dünyaya açılan veri ve metodlara kısıtlama getirir. Bunlara private veri ve metodlar denir.

Neden ?

1. Validation
2. Debugging
3. Kullanıcının direk kontrol etmesini istemediğimiz değişkenler.
4. Multi-Thread applications, metodlarımızı thread safe yapıp değişkenlerin değişmesini kontrol altına alabiliriz aksi taktirde race condition gibi concurrency problemleri ile karşılaşabiliriz.

## Abstraction ?

Soyutlama, dışarıya sadece gerekli fieldları ve fonksiyonları açmak demektir. Programın karmaşıklığını azaltır.

## Inheritance ?

Bir grup modelin ortak bir modelden türemesi yani genelleştirilmesine inheritance denir.

## Polymorphism ?

Bir nesnenin farklı amaçlar içinde kullanılabilmesine denir.

### Static Polymorphism

Compile-Time polymorphism, Static binding, Compile-Time binding, Early binding and Method overloading.

### Dynamic Polymorphism

Run-Time polymorphism, Dynamic binding, Run-Time binding, Late binding and Method overriding.

## Paradigm Features

1. Abstract Classes
2. Concrete Classes
3. Scope / Visibility
4. Interfaces

### Abstract Classes

Abstract sınıf, abstract keywordü ile tanımlanan abstract metodları olabilen(olmak zorunda değildir) özel tip classlardır.

Abstract classlardan nesne oluşuturulamaz. Inherit eden class abstract class'ın abstract metodlarını implemente etmek zorundadır.

### Interface

Abstract class gibi davranır ancak bir class birden fazla interface inherit edebilir.

Interfaceler daha çok bir şeyi yapabilme kabiliyeti verme amacıyla yazılır. Throwable, Runnable gibi.

### Concrete Classes

Abstract sınıfların tam tersi somut sınıflardır. Nesnelerimizi bunlardan üretiriz.
