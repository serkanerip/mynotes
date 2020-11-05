# PHP

## ?:

Kısa ternary operator.

```php
$result = $condition ? $condition : 'default';

$result = $condition ?: 'default';
```

## ??=

Eğer sol taraftaki değişken null ise sağ taraftaki değeri ver.

```php
$k = null;
$k ??= 'default';

echo $k; // 'default'
```

## <=>

Kıyas yapmak için kullanılan bir operatör.

-1, 0, 1 değerlerinden birini dönderir.

Operatörün solundaki operand sağındaki operanda göre:

Eşit ise: 0,
Büyük ise: 1,
Küçük ise: -1

.

```
1 <=> 2; // Will return -1, as 2 is larger than 1.
'a' <=> 'z'; // -1
$array = [5, 1, 6, 3];

usort($array, function ($a, $b) {
    return $a <=> $b;
});

// $array = [1, 3, 5, 6];
```