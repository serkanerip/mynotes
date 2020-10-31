# Serverless

Serverless, bize kodumuzu çalıştıracak bir ortam sağlar ve bu ortamın ölçeklenmesi, güncellenmesi, sürdürülebilirliği gibi konuları kendi problemi haline getirir ve sizin yerinize bunları halleder. Serverless da uygulamanıza gelen request kadar ödersiniz ve uygulamanıza gelen request sayısı artınca ölçeklenmeyi kendi ayarlar.

IaaS(Infrastructure as a Service), hizmet sağlayıcı belirttiğiniz özelliklere göre sanal makine verir ve bu makinenin ayakta kalmasını garanti eder. Bunun dışındaki firewall, networking gibi konularla biz ilgileniriz.

CaaS(Container as a Service), hizmet sağlayıcı verdiğiniz container imajının çalışağı ortamı hazırlar. Uygulamalar request geldiği zaman ayağa kalkar. Ücretlendirme genel olarak kullanılan cpu/ram üzerinden yapılır. Otomatik ölçeklenir.

PaaS(Platform as a Service), çalıştırmak istediğimiz uygulamanın runtime dosyalarını veriyoruz hizmet sağlayıcı da bunu çalıştıracak ortamı hazırlar. Otomatik ölçeklenir. Sınırlı sayıda dil ve framework destekler ve sadece web uygulamaları.

FaaS(Function as a Service), hizmet sağlayıcıya sadece oluşturduğumuz fonksiyonu gönderiyoruz bu 2 satırlık bir kodda olabilir, bize bunu bir uygulama haline getirir ve bir endpoint olarak verir. Diğer servislere göre daha pahallıdır. Sınırlı sayıda dili destekler.

SaaS(Sofware as a Service), Cloud üzerinden direk olarak bir yazılım hizmeti alabilirsiniz.

