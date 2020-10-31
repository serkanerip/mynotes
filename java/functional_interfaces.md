# Functional Interfaces

- Function<T,R>: T tipinde bir parametre alıp, r tipinde değer dönderen fonksiyonlardır. IntFunction, LongFunction gibi türevleri vardır bunlar isminde parametrenin tipini belirtir ve sadece dönüş tipini belirlemeniz gerekir. ToIntFunction, ToDoubleFunction gibi türevleride dönüş tipini isminde belirtir size sadece parametre tipini belirtmek düşer.
- BiFunction<T,U,R>: Yukarıda belirtilen function ile aynıdır ancak ekstra bir parametre daha alır.
- Consumer<T>: T, tipinde parametre alır ve void döndürür side effectdir.
- Predicate<T>, T, tipinde parametre alır ve boolean dönderir.
- Runnable: Parametre almayıp, void döndüren metoddur.
- Supplier<R>: Parametre almayıp, R tipinde değer dönderen metotdur.
