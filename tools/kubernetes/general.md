# Kubernetes

K8S, açık kaynak bir conteynır şefidir.

## Cluster Oluşturmak

Cluster oluşturmak için 3 yöntem bulunur.

1. Cloud providerlar sayesinde kolayca bir cluster oluşturabiliriz. e.g. gcloud, aws, azure
2. Custom bir şekilde k8s clusterı oluşturabiliriz.
3. Minikube, aracı ile localde tek node'luk bir cluster oluşturabiliriz development için kullanılır.

## Kubernetes Node

Node, clusterı oluşturan makinalardır(VM's, physical servers, etc).

## Kubernetes Master Node

Cluster'ı yönetmekle sorumludur.

Master node 3 process çalıştırır:

1. kube-apiserver
2. kube-controller-manager
3. kube-scheluder

## Kubernetes Non-master Node

Master olmayan nodelar 2 process çalıştırır:

1. kubelet: master ile iletişimi sağlar.
2. kube-proxy: k8s network işlemlerini sağlar.

![](https://d33wubrfki0l68.cloudfront.net/7016517375d10c702489167e704dcb99e570df85/7bb53/images/docs/components-of-kubernetes.png)

## Pods

- Containerları soyutlayan bir nesnedir docker ve ya rkt containerları çalıştırır.
- Containerlar podların içinde çalışır. Bir pod'da birden fazla container olabilir.
- Podlar, nodeların içinde çalışır. Bir node'da birden fazla pod olabilir.
- Pod içindeki containerlar aynı networkü kullanırlar localhost üzerinden iletişim kurabilirler.
- Podlar, service ile expose edilmediği sürece dışarıya kapalıdırlar. Expose edilmeyen podlara kube-proxy ile erişebiliriz.
- K8S'de, imperative bir şekilde pod nesnesi oluşturmak iyi bir pratik değildir deployment ile declarative bir şekilde yapılması tavsiye edilir.
- Podlar arasında iletişim için podlara cluster ip almamız gerekir bunuda service ile yaparız, pod için service oluşturduktan sonra servisin ismiyle ya da ipsi ile ulaşabiliriz.
- Podlara volume mount edebiliriz.

## Deployment

- Deklaratif, bir şekilde pod ayağa kaldıran bir k8s nesnesidir.
- Podları yönetir.

## Service

- Podları dışarıya expose etmek için kullanılır.

## Notlar

expose yaparken loadbalancer olarak ayarlamazsak external olarak expose etmiyor.

kubectl proxy ile podların ağına bağlanabiliyoruz.

```sh
curl localhost:8001/api/v1/namespaces/default/pods/[POD_NAME]/proxy/
```

**Cluster olusturma komutu:**

```
gcloud container clusters create cluster-http --machine-type=g1-small --num-nodes=1 --enable-autoscaling --min-nodes 1 --max-nodes 4
```

**Clustera kubectl ile erisebilmek icin crendtialslari indirmemiz lazım kubeconfig dosyasına**

```
gcloud container clusters get-credentials [CLUSTER_NAME]
```

**Deploymenttaki containerların imajlarını update etme**

```
kubectl set image deployments [DEPLOYMENT_NAME] [CONTAINER_NAME]=[IMAGE_NAME] --record
```

### Komutlar

kubectl scale --replicas=5 deployment/goapp-deployment // deploymentun pod sayısını 5 yap
kubectl delete services [service-name]

### Kaynaklar

1. https://blog.digia.com/getting-started-with-kubernetes-in-google-cloud
2. https://medium.com/google-cloud/kubernetes-nodeport-vs-loadbalancer-vs-ingress-when-should-i-use-what-922f010849e0
3. https://linuxacademy.com/blog/containers/building-a-three-node-kubernetes-cluster-quick-guide/
