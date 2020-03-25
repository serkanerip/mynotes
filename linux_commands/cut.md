## Cut

Metinden kesitler almak için kullanılır.


```bash
echo "hello bash" | cut -b 1-4
```
Bu komutun sonucu `hell` olur. <b>Byte pozisyonları birden başlar.<b>

cut -b 1-: 1 den sona kadar.

cut -c 1-5: Her satırdaki 1-5 arasındaki byteları

cut -f 1: Delimitera göre parçalar ve birinci kelimeyi verir.
    
    Default delimiter tabtır. Ancak bunu değiştirmek için -d kullanabilirsiniz.
    Örnek olarak -d " " : delimeter boşluk oldu.