# Elasticsearch

ElasticSearch, BigData yani büyük veriler ile çalışan şirketlerin, adından da anlaşılacağı gibi içerik arama, veri analizi, soruglamalar ve öneriler gibi işlemlerde özellikle performans kabiliyetleri, güçlü ve esnek olmasından dolayı tercih ettiği bir search engine’dir. 


* Veri saklama biçimi ilişkisel değil document-oriented şeklindedir.
* Dağıtık ve ölçeklenebilir yapıda çalışabilir.
* Gerçek zamanlı verileri analiz edebilir.
* Restfull bir api ile hizmet verir bu sayede her hangi bir http isteği atabilen dilde kullanabilirsiniz.
* Elasticsearch’ü monitör edebilen Kibana ve log barındırmak için Logstash araçları ile bilirkte kullanılabilir.
* Dökümanları JSON olarak indexler.

RelationalDB  -> Databases -> Tables -> Rows      -> Columns
Elasticsearch -> Indices   -> Types  -> Documents -> Fields


### Index

Öncelikle ElasticSearch’de her kayıt bir Json dökümandır. Yani indexler, Json belgeler topluluğudur. Default veya Custom olarak yaratılabilirler. Kısaca her bir index bir çeşit veritabanıdır.

### Document

E'de her bir kayda, yani row'a document denir.

### Field

Databaselerdeki column alanına denk gelir yani sütundur.

### Type

Bir index, birden fazla type barındırabilir.

### Full Text Search

Herhangi bir kaynaktan alınan metin belgeleri içinden, herhangi bir anahtar kelimenin aratılarak, anahtar kelime ile eşleşen dökümanların bulduğu  sonuca hızlı şekilde erişime verilen isimdir. 

### Mapping

Verileri indexlerken bu verilerin hangi tipte olduğunu göstermemiz gerekir. Yani bir kelimeyi indexlerken o kelimenin hangi veri tipinde(string, integer, boolean) olduğu bilgisinin tanımlandığı işlemdir.

### Shard

Bir seferde milyonlarca dökümanı indexlemek için yeterli donanıma/server kapasitesine sahip olunmayabilir. Bir seferde 2TB lık veriyi indexlemek zorunda kaldığınızı varsayalım, bu durumda bu indexlemeyi tek bir node ile yapamak istediğinizde, disk kapasitesinin dolması veya aşırı yavaş bir indexleme hızı ile karşı karşıya kalabilirsiniz. Bunun önüne geçmek için Shard ve Replika kavramları bulunmakta.

* Birden fazla node üzerinde işlemleri dağıtmanızı ve paralelleştirmeyi sağlar. Böylece performans artar.


### Replica

Shard’ın devre dışı kalması ihtimaline karşı index shard’larının bir veya birden çok kopyasının oluşturulabilmesini sağlayan replica-shard yapısı bulunur.
Bir shard’a ait replica, aynı node’da barındırılmamalıdır. Bir node çöktüğünde o node’daki shard(lar)’ınyedeklerinin diğer nodelarda bulunması veri kaybını önlemek için şarttır.



ES, hem bir database hemde bir text search engine dir. Ancak asıl amacı birincil veritabanı olmak değil.

### Use Cases

* Loglama ve log analizi
* Full text search
* Visualizing Data
* Scraping and Combining Public Data