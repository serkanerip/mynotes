# Kubectl

- Kubectl, k8s clusterlarımızı yönetmek için kullandığımız cli'dır.
- **~/.kube/config** dosyası içinde yaml formatında kubectl için gerekli ayarlar bulunmaktadır.
- Eğer **KUBECONFIG** env. belirtilmişse belirtilen adresteki dosyayı ayar dosyası olarak görür.
- Bu dosya içerisinde contextler ve clusterlar hakkında gerekli bilgiler bulunur.
- **current-context** fieldı şuanda kubectl'in hangi cluster uzerinde islem yaptığın belirtir.
- gcloud, aws, amazon gibi kubernetes providerlarda cluster olusturduğumuzda cluster credentiallarını bu dosyaya çekmemiz lazım. Aşağıda örnek olarak gcloud'da cluster bilgileri nasıl kubectl'e eklenir gösteriliyor. Gcloud otomatik olarak kubeconfig dosyasını günceller.
  ```
      gcloud container clusters get-credentials [CLUSTER_NAME]
  ```
-
