# SOC

Temiz bir şekilde bir birinden ayrışabilen modüllerin iki karakteristik özelliği şunlardır:
    1. Low Coupling
    2. High Cohesion

* Öğelerimizi modüllere ayırırken genel olarak izlediğimiz prensip şu: Bir değişiklik yapmamız gerekiyorsa sadece bir yerde yapmamız gerekmeli, bir yeri değiştirmemiz gerekiyorsa bunun sadece bir nedeni olmalı.



## Cohesion

Bir modülün içindeki öğelerin bir biriyle alakasının ölçülmesine denir. Yüksek cohesion, modülün içindeki öğelerin bir biriyle alakasının yüksek olduğunu belirtir low cohesion ise tam tersi durumu gösterir. Yazılımlarda amaç high cohesiondır.

## Coupling

Modüller arasındaki bağı ifade eder. Yüksek coupling modüllerin bir birine bağımlılığının çok olduğunu gösterir bu yazılımda kötü bir senaryodur. Hedeflenen low couplingdir.


High cohesion, low coupling'i class, package, library katmanlarında uygulayabiliriz. Bir sınıf içindeki metodların cohesion'ı yüksek tutarak, bir üst katmanda o paketteki sınıfların bir biriyle alakalı sınıflar olmasını sağlarız, library ya da application seviyesinde ise uygulamanın