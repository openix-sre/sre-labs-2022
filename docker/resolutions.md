# Resolución de los ejercicios

En este apartado se detalla los comandos para la resolución de los ejercicios de la unidad "Docker"

## Ejercicios previos

1. Desplegar un servicio de nginx por el puerto 8080 del host en background.

```sh
docker run -d --name nginx-service -p 8080:80 nginx
```

2. Crear una red docker con el nombre "devops_2022".

```sh
docker network create devops_2022
```

3. Inspeccionar los metadatos del contenedor desplegado en el ejercicio 1.

```sh
docker inspect nginx-service
```

4. Crear un volumen docker con el nombre "data_mysql".

```sh
docker volume create data_mysql
```

5. Desplegar una base de datos mysql con la contraseña de root "devops_2022", con el volumen "data_mysql" para persistir los datos de la misma,
    en la red docker creada en el paso 2 y escuchando el puerto 4001 del host en background.

```sh
docker run -d --name db-mysql -e MYSQL_ROOT_PASSWORD=devops_2022 -v data_mysql:/etc/mysql/conf.d --network devops_2022 -p 4001:3306 mysql
```

6. Crear un archivo "Dockerfile.nginx", copiar el contenido del la carpeta html en la carpeta html de la imagen de nginx oficial. Hacer el build
    de la imagen basado en dicho archivo y desplegar el servicio con dicha imagen en el puerto 8081 del host en background.

```Dockerfile
# touch Dockerfile.nginx

FROM nginx:latest
ADD html /usr/share/nginx/html
```

```sh
docker build -t nginx-sre -f Dockerfile.nginx .

docker run -d --name nginx-sre -p 8081:80 nginx-sre
```

7. Crear un manifiesto de docker compose "compose_1.yml" con un servicio de metabase y una base de datos mysql. Correr los servicios en primer plano.

```yaml
# touch compose_1.yaml

version: "3"
services:
  metabase-app:
    image: metabase/metabase
    restart: always
    ports:
      - 3000:3000
    environment:
      MB_DB_TYPE: mysql
      MB_DB_DBNAME: metabase
      MB_DB_PORT: 3306
      MB_DB_USER: metabase
      MB_DB_PASS: devops2022
      MB_DB_HOST: db-mysql
    depends_on:
      - db-mysql
  db-mysql:
    image: mysql
    restart: always
    ports:
      - 3306:3306
    environment:
      MYSQL_ROOT_PASSWORD: devops2022
      MYSQL_DATABASE: metabase
      MYSQL_USER: metabase

```

```sh
docker-compose -f compose_1.yaml up
```
