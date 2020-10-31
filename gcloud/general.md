# GCR'e imajlarınızı yüklemek

Gcloud'a login olup docker için bir auth key eklemeniz gerekiyor öncelikle dokümantasyonlardan bulabilirsiniz.

Daha sonra gcr.io/[PROJECT_ID]/[IMAGE_NAME] şeklinde imaj dosyanıza tag verip docker push yapıyoruz.

```
docker push gcr.io/serkan-test/curl
```

# Service Accounts

Servislerimizi kullanan hesaplar ekleyip çıkarabiliriz. Ve servislerimize hangi kullanıcının hangi rol ile erişebileceğini belirleyebiliriz.

# CLOUD RUN

Serverless olarak http handle eden docker containerlarını çalıştırmak için kullanılır containerlarınız aldığı requestlere göre fatura çıkarıyor.
