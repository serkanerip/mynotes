# Protobuf

Büyük uygulamalar bir çok servisle çalışır ve bu servisler network üzerinden haberleşirler. Servis mimarisi şuanda popüler olan mimaridir monolitik mimariye göre tercih edilmesinin sebebi kolay ölçeklenebilen ve sürdürülebilen uygulamalar geliştirmek.

## Service (Servis)

Network üzerinden belli bir mesaj protokolü ile iletişim kuran yazılımlara denir.

## Rest

Webservislerin haberleşmesi için yapılmış text bazlı bir protokoldür.

![](https://safehammad.com/wp-uploads/2010/10/json-rest3.png)

## Binary Protokoller

Redis, mysql gibi uygulamaların kullandığı protokoldür. Az veri akışıyla çok iş yapan özel geliştirilmiş protokollerdir dezavantaj olarak bütün dillerde client implemente etmek zorunda kalınır.

## Protobuf

Binary bir protokoldür kolay bir şekilde yazılabilir, fieldların isimleri, tipleri ve numaraları vardır. Ancak alan isimleri, boşluklar gibi fieldların değerini represente etmeyen karakterler iletişim esnasında gönderilmez.

### Json Ve Xml dezavantajları

Json ve xml ler insanların okuyarak işlem yapmasına dayalı protokollerdir fieldların isimlerini biliriz ve ona göre implementasyon yaparız ancak bu field isimleri, yazılma biçimleri gibi nedenlerle data boyutu artar. Buda servis mimarisi ile çalışan sistemlerde performans kayıplarına ve fazla kaynak harcanmasına neden olur.

### Performans Farkları

- Numerik verilerde protobuf, json'a göre daha hızlı encode ve decode yapıyor. 2x-13x arasında değişebiliyor bu performans farkı.

- String encode/decode işlemlerinde json kütüphaneleri ile yakın sonuçlar alıyor bazılarından geride bile olabiliyor.

- Double, double arraylerde protobuf büyük bir fark atıyor diğer kütüphanelere.

Yorumlamalar Tao Wen'in yaptığı benchmark sonuçlarına istinaden yapılmıştır detaylı sonuçlar için kaynaklar kısmından linke gidebilirsiniz.

### Veri Boyutu Farkları

Protobuf, binary şeklinde veri yolladığı için jsonlara göre daha az alan kaplarlar. Yollanacak veri boyutları küçük olduğu zaman protobuf büyük bir fark atıyor jsonlara ancak veri boyutu büyüyünce transfer edilen veri boyutu farkı da azalıyor protobuf ile json arasında.

Tabi JSON'lar çoğu sunucuda raw halinde gelip gitmiyor genellikle gzip tarzı sıkıştırma algoritmaları ile sıkıştırılıp transfer ediliyorlar. Bu işlemde tabiki cpu kaynaklarından harcıyor ancak protobuf'ın buna ihtiyacı yoktur.

Kaynaklar kısmında benchmark sonuçlarını detaylı paylaşan bir makalenin linki bulunmaktadır.

### Kaynaklar

- [gRPC ve Protobuf ile verimli client/server iletişimi [GDG Ankara Meetup] by Ahmet Alp Balkan](https://www.youtube.com/watch?v=D2mP5vWtVL4)
- [Is Protobuf 5x Faster Than JSON? (Part 1) by Tao WEN](https://dzone.com/articles/is-protobuf-5x-faster-than-json)
- [Is Protobuf 5x Faster Than JSON? (Part 2) by Tao WEN](https://dzone.com/articles/is-protobuf-5x-faster-than-json-part-ii)

- [Comparing sizes of protobuf vs json by nilsmagnus](https://nilsmagnus.github.io/post/proto-json-sizes/)
