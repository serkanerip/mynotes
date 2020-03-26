# Views

Views, bir sanal tablodur. Asıl tablolarımıza yaptığımız kompleks statik(değişken almayan) sorguların sonucunu tek bir tabloymuş gibi gösteririr viewslar.

  - Sadece SELECT sorgusu yapılabilir
  - ORDER BY yapılamaz
  - Çağırırken aynı tablo SELECT eder gibi sorgu yazabiliriz.

# Nasıl Oluştururum!

```sql
    CREATE VIEWS [VIEW_NAME] AS [QUERY]
```

### Örnek:


```sql
    CREATE VIEWS vSelectOrderWithDetails AS 
        SELECT o.*, od.customer_name, od.ship_city FROM orders o, orderdetails od
```

### Kullanım:

```sql
    SELECT * from public.vSelectOrderWithDetails
```

### Kaynakçalar:

- https://www.furkanpezek.com.tr/2018/04/sql-view-kullanimi/
- [VIEWS and FUNCTIONS in the PostgreSQL Northwind DB By  Technology is King](https://www.youtube.com/watch?v=u1iB8-VywT8)


License
----

MIT