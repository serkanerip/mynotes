# Linux Containers (LXC)

LXC is an operating-system-level virtualization method for running multiple isolated Linux systems on a control host using a single Linux kernel.

Uygulamaları çalıştırmak için gerekli konfigürasyonlar yüklenmesi gerekenler gibi bir çok yapılması gereken oluyor.Uygulamayı tutan konteynırda gerekli kütüphaneler, bağımlılıklar ve dosyaları barındırarak kolayca ve hızlı bir şekilde başka makinalarda çalıştırabilmemizi sağlar.

## Isn’t this just virtualization?

Virtualization lets your operating systems (Windows or Linux) run simultaneously on a single hardware system.

Containers share the same operating system kernel and isolate the application processes from the rest of the system.

## Cgroups

Processin neleri ne kadar kullanabileceğini limitler.

Bir uygulama bütün bir bilgisayarın çökmesine sebeb olmasın diye uygulamanın cpu, memory, disk ve ya networkünü sistemin geri kalanından izole edebilmemizi sağlar.

Cgroup sayesinde bir ve ya birde fazla uygulamanın aynı kaynak limitlerine sahip olması sağlanabilir.

Eğer gerekli paketler yüklüyse cgroup'u direk /sys/fs/cgroup altında yönetebiliriz. Memory, cpu, disk, io gibi kaynaklar için ayrı ayrı klasörler bulunmakta. Bunların içinde yeni bir cgroup oluşturabiliriz. Dockerı görebiliyoruz bu dosyaların içinde dockerda cgroup ile kaynakları limitliyor.

## Chroot

Processin, dosya sistemini izole etmek için kullanılır.

## Namespaces

Processin neleri görebileceğini limitler. e.g. process lists, network interfaces

    Mount - isolate filesystem mount points
    UTS - isolate hostname and domainname
    IPC - isolate interprocess communication (IPC) resources
    PID - isolate the PID number space
    Network - isolate network interfaces
    User - isolate UID/GID number spaces
    Cgroup - isolate cgroup root directory

## Kaynaklar

1. https://www.linuxjournal.com/content/everything-you-need-know-about-linux-containers-part-i-linux-control-groups-and-process
2. https://blog.scottlowe.org/2013/09/04/introducing-linux-network-namespaces/
3. https://medium.com/@teddyking/linux-namespaces-850489d3ccf
4. https://medium.com/@teddyking/namespaces-in-go-basics-e3f0fc1ff69a
5. https://medium.com/@teddyking/namespaces-in-go-user-a54ef9476f2a
6. https://medium.com/@teddyking/namespaces-in-go-reexec-3d1295b91af8
7. https://medium.com/@teddyking/namespaces-in-go-mount-e4c04fe9fb29
8. https://medium.com/@teddyking/namespaces-in-go-network-fdcf63e76100
9. https://medium.com/@teddyking/namespaces-in-go-uts-d47aebcdf00e
