# Time Functions


SQL'de kullanabileceğimiz yararlı fonksiyonlar ve örnekler. 

Postgresql üzerinde yapılmıştır örnekler.
Diğer veritabanı yazılımları üzerinde farklılıklar gösterebilir.

Veritabanı Yazılımı: <b>PostgreSQL 12.2 (Debian 12.2-2.pgdg100+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 8.3.0-6) 8.3.0, 64-bit</b>


    Veritabanında zaman dilimi(timezone) doğru bir şekilde ayarlanmamışsa beklenmedik ve hatalı sonuçlar alabilirsiniz.


Geçici Timezone Değiştirme:

    SET TIMEZONE='Turkey';


Veritabanı Timezone Değiştirme Kalıcı Olarak:

    ALTER DATABASE postgres SET timezone TO 'Turkey';
    

### Fonksiyonlar:

   - CURRENT_DATE
   - CURRENT_TIME
   - CURRENT_TIMESTAMP
   - LOCALTIME
   - LOCALTIMESTAMP
   - NOW
   - TIMEOFDAY
   - DATE_PART
   - AGE
   - EXTRACT



### Örnekler


#### CURRENT_DATE:
```sql
SELECT CURRENT_DATE -- RETURNS 2020-03-26
```

#### CURRENT_TIME:
```sql
SELECT CURRENT_TIME -- RETURNS 20:42:41.324073+03:00
```

#### CURRENT_TIMESTAMP:
```sql
SELECT CURRENT_TIMESTAMP -- RETURNS 2020-03-26 20:43:00.921386+03
```

#### LOCALTIME:
```sql
SELECT LOCALTIME -- RETURNS 20:55:10.565876
```

#### LOCALTIMESTAMP:
```sql
SELECT LOCALTIMESTAMP -- RETURNS 2020-03-26 20:55:49.508973
```


#### NOW:
```sql
SELECT NOW() -- RETURNS 2020-03-26 21:03:08.943293+03
```

#### TIMEOFDAY:
```sql
SELECT TIMEOFDAY() -- RETURNS Thu Mar 26 21:03:54.248302 2020 +03
```

#### DATE_PART:
```sql
SELECT DATE_PART('year', CURRENT_DATE) -- RETURNS 2020
SELECT DATE_PART('month', CURRENT_DATE) -- RETURNS 3
SELECT DATE_PART('day', CURRENT_DATE) -- RETURNS 26
SELECT DATE_PART('week', CURRENT_DATE) -- RETURNS 13 // [1-54 arasi deger alir]
SELECT DATE_PART('quarter', CURRENT_DATE) -- RETURNS 1 // [1-4 arasi deger alir]
```

#### AGE:
```sql
SELECT AGE(CURRENT_DATE, timestamp '1998-08-17') -- RETURNS 21 years 7 mons 9 days

SELECT EXTRACT(year FROM age(CURRENT_DATE,'1998-08-17'))*12 + EXTRACT(month FROM age(CURRENT_DATE,'1998-08-17')) as months -- RETURNS 259 // bu islem bize iki tarih arasinda kac ay oldugunu bulur.

```

#### EXTRACT:
```sql
SELECT EXTRACT(year FROM CURRENT_DATE) as year -- RETURNS 2020
SELECT EXTRACT(milliseconds FROM CURRENT_TIMESTAMP) as ms -- RETURNS 5763.741
```


## Örnekler


Belli bir yıla ait kayıtları getirmek.

```sql
select shipped_date from orders
WHERE
EXTRACT(year FROM shipped_date) = 1997
```

Ayın birinci günlerinde yapılan toplam sipariş adetini getirmek.
```sql
select count(*) as total_orders from orders
WHERE
EXTRACT(day FROM shipped_date) = 01
```


## Kaynakçalar:

- https://www.postgresql.org/docs/8.0/functions-datetime.html
- https://www.postgresqltutorial.com/postgresql-extract/
- http://www.lexykassan.com/coding-conundrums/calculating-months-between-dates-in-postgresql/


License
----

MIT