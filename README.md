# docker-web 
Ambiente de desarrollo con Apache + PHP + MariaDB

> Configuraciones necesarias para usarlo

##  Pre-condicion ( tener instalado ) 
* docker visitar: https://docs.docker.com/engine/install
* docker compose visitar: https://docs.docker.com/compose/install

##  Instalacion
```shell
git clone https://github.com/samrodriguez/docker-web
cd docker-web/
docker-compose up -d
// visit http:localhost:8000/index.html
```

## Agregar los virtual host
```shell
vim /etc/hosts
...
127.0.0.1   docker.abcis
...
// visit http:docker.abcis:8000/index.html

```
