# docker-web 
Ambiente de desarrollo con Apache + PHP + MariaDB

> Configuraciones necesarias para usarlo

##  Pre-condicion ( tener instalado ) 
* docker visitar: https://docs.docker.com/engine/install/debian
* docker compose visitar: https://docs.docker.com/compose/install

##  Instalacion
```shell
git clone https://github.com/samrodriguez/docker-web
cd docker-web/
docker-compose up -d
// visit http:localhost:81/info.php
```

## Agregar los virtual host
```shell
vim /etc/hosts
...
127.0.0.1   webserver.sf
...
// visit http:webserver.sf:81/info.php

```
