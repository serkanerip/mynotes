# Kubectl

- Kubectl, k8s clusterlarımızı yönetmek için kullandığımız cli'dır. Master içindeki kube-apiserver processinin sağladığı api ile clusterı yönetiriz.
- Konfigürasyon için kubectl **~/.kube/config** dosyasına bakar. Farklı bir config dosyası belirtmek için **KUBECONFIG** env. variable'ını ve ya --kubeconfig flagini kullanabiliriz.
- Bu ayar dosyası içinde contextler ve clusterlar hakkında gerekli bilgiler bulunur.
- **current-context** fieldı ile hangi cluster'ı yöneteceğimizi seçeriz.
- gcloud, aws, amazon gibi kubernetes providerlarda cluster olusturduğumuzda cluster credentiallarını bu dosyaya çekmemiz lazım. Aşağıda örnek olarak gcloud'da cluster bilgileri nasıl kubectl'e eklenir gösteriliyor. Gcloud otomatik olarak kubeconfig dosyasını günceller ve current-context'i set eder.
  ```
      gcloud container clusters get-credentials [CLUSTER_NAME]
  ```

## Komutları

1. Apply: Yaml dosyası halindeki konfigürasyonu json'a çevirip api ile nesneleri oluştur eğer daha önce oluşturulmuşsa update eder.
2. Get: Belirttiğiniz kaynak tipinde oluşturulan kaynakların listesini dönderir. e.g. kubectl get pods
3. Describe: Kaynak hakkında detaylı bilgi verir. e.g. kubectl describe pod my-pod
4. Logs: Kaynağın stdout çıktısını gösterir. e.g. kubectl logs my-pod
