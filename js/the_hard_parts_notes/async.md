# ASYNC

Javascript tek thread üzerinde çalışan bir sistemdir. I/O işlemlerini de sistemi bloklamaması için asenkron olarak işaretler ve bunlar global contextdeki işlemler bitene kadar beklerler.


### Callback(Task) Queue: 

Asenkron işlemlerde verilen cb fonksiyonları burada sıraya girerler ve global EC. bittiğinde sırayla işleme alınır.

### MicroTask Queue:

Burası da, asenkron işlemlerin cb fonksiyonlarını tutar ancak buraya network işlemlerinden gelen cbler eklenir.

**Bu kuyruk, Task Queue'dan daha yüksek önceliklidir.**
