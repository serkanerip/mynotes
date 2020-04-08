## DOCKER COMPOSE


* Docker compose ile oluşturduğumuz servislerden birinden diğerine haberleşmeyi servise verdiğimiz isim ile kurabiliriz.
  - Misal api ve db diye servislerimiz var ise. Api container içiden http://db ile db containerına erişebiliriz.



* Docker compose'u container olarak kullanmak:
  * docker run docker/compose:1.24.0 version
  * docker run --rm \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v "$PWD:$PWD" \
    -w="$PWD" \
    docker/compose:1.24.0 **up**