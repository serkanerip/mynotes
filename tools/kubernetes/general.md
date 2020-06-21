# Kubernetes

K8S, açık kaynak bir conteynır şefidir.

## Kubernetes Master

Cluster'ı yönetmekle sorumludur. kubectl komutu ile master ile iletişim kurarız.

Master node 3 process çalıştırır:

1. kube-apiserver
2. kube-controller-manager
3. kube-scheluder

## Kubernetes Nodes

Node, cluster içindeki uygulamanızı çalıştıran makinalardır(VM's, physical servers, etc).

Master olmayan nodelar 2 process çalıştırır:

1. kubelet: master ile iletişimi sağlar.
2. kube-proxy: k8s network işlemlerini sağlar.

![](https://d33wubrfki0l68.cloudfront.net/7016517375d10c702489167e704dcb99e570df85/7bb53/images/docs/components-of-kubernetes.png)

## Pods

Containerları soyutlayan bir nesnedir docker ve ya rkt containerları çalıştırır. Podlarda nodelarda bir node'da birden fazla pod olabilir bir podda da birden fazla konteynır. Pod içindeki nodelar aynı ip ve port alanını kullanır, localhost üzerinden iletişim sağlıyabilirler.

K8S'de, direk pod çalıştırmak iyi bir pratik değildir deployment ile bu iş yapılır.

## Deployment

Belirlediğimiz ayarlara göre uygulama conteynırlarından podlar oluşturur ve bu podları ayakta tutmaya ve sayısını korumaya çalışır.

## Service

Podları dışarıya expose etmek için kullanılır.

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
