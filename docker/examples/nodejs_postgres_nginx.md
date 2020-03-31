### Ex

Servisler:

    - Nodejs ile geliştirilen bir rest api
    - Postgresql DB
    - Nginx

Bu örnekte docker compose ile bu 3 servisin bir biriyle entegrasyonu yapılacaktır.

Nodejs, postgresql'e bağlıdır ve bu servise erişebilmesi lazım.
Nginxte, nodejs'e bağlıdır ve bu servise erişebilmesi lazım reverse proxy işlemini yapabilmek için.

```yaml
version: '3.1'
services:
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
    - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - api
  api:
    build: backend
    command: npm run prod
    restart: always
    environment:
      CONNECTION_STRING: postgres://postgres:toor@pg:5432/project
    depends_on:
      - pg
    ports:
    - 1907:1907
  pg:
    build: backend/sql

```

* Burda apiye pg servisine bağlı olduğunu belirtiyoruz. Postgresql connection stringi içinde de postgresql servisine verdiğimiz ismi host için kullanıyoruz. postgres://postgres:toor@**pg**:5432/project

