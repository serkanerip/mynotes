# Ports And Adapters

## Application

- Dış dünyanın uygulama ile nasıl etkileşime geçeceğinin tanımlandığı yerdir.
- Bu bir rest controller, message service ve ya command line client olabilir.
- Clientın application core'a ulaşabilmesi için bir geçittir.

## Core

- Burada uygulamanızın amacını gerçekleştirmek için yazdığınız kodlar bulunmaktadır (business logic).
- Basit ve anlaşılır olması istenir domainlere ayrıştırılır.

## Infrastructure

- Uygulamalar çoğu zaman sadece business logicten oluşmazlar.
- Bazen dış sistemlere ihtiyaç duyarlar bu bir; database, queue, ftpServer olabilir.
- Bu katmanda bu dış sistemlerle nasıl iletişime geçileceği implemente edilir.

**Application, Core, Infrastructure**: Aslında normal layered architecture'a çok benzer, controller, services, repositoriesler ancak farklı isimlerde. Ancak burda en büyük fark core katmanı application ve infrastructure hakkında bir şey bilmiyor.

## Domain

Domain uygulamanın küçük bir parçasıdır. Uygulamanın yaptığı işler domainlere parçalanır.
E.g. user domainimiz olsun bu kullanıcı işlemleri için gereken business logici implemente eder. Application, core, infra katmanları her domainde de olur.

- Core içinde modellerimiz, portlarımız ve facedelarımız bulunur.
- Modeller, entityler ve ya başka amaçla kullanılan sınıflar olabilir.Validation logicleri modellerin içinde olur.
- Facede clientin isteğine göre yapılacak kodların yazıldığı sınıftır (service).
- Ports, core logici application ve infra katmanlarından soyutlamak için kullanırız.

### Core/Models

Modeller, entity, dto, exception gibi domainin kullacağı sınıfların yazıldığı yerdir. Bu sınıfların validationları içlerinde yazılır.

### Core/PORTS

- Incoming ports, application layerdan core'a erişmek için yazdığımız soyut sınıfları ve ya interfaceleri tutar.
- Outgoing ports, core logicten, infra layera erişmek için yazdığımız soyut sınıfları ve ya interfaceleri tutar.
- Portlar, yapmak istediğimiz şeylerin tanımlamasını barındırır nasıl yapacağımız barındırmazlar. Nasıl yapacağımızı **adapter**'lar belirler.

### Core/Facades

- Incoming portların implementasyon detayları burada yazılır.

### Infrastructure

- Outgoing portların, adapterları burada yazılır.
- Bir portun birden fazla adapterı olabilir çünkü core logic buranın nasıl işlemesi gerektiği ile ilgilenmez.
- Eğer orm kullanılmıyorsa mapperlar, burada yazılır db sonuçlarının modellere dönüştürülmesinden sorumlu sınıflardır.

### Kaynaklar

1. https://medium.com/@wkrzywiec/ports-adapters-architecture-on-example-19cab9e93be7
2. https://medium.com/azimolabs/ports-and-adapters-implementation-in-php-with-a-little-symfony-help-6d4fdbe830ba
3. https://www.gokhan-gokalp.com/getting-started-with-clean-architecture-using-asp-net-core-02/
