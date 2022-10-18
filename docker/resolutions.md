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

## Ejercicios de práctica

8. Desplegar un cloudbeaver en el puerto 8978 del host, en la red docker creada en el paso 2 y en background.

```sh
docker run --name cloudbeaver -d -p 8978:8978 --network devops_2022 dbeaver/cloudbeaver
```

9. Crear un volumen docker con el nombre __"data_psql"__.

```sh
docker volume create data_psql
```

10. Desplegar una base de datos postgres con la contraseña __"postgres_2022"__ en el puerto 5400 del host y persistir los datos en el volumen creado
en el ejercicio 9. El servicio debe correr en primer plano. La red del servicio debe ser la creada en el ejercicio 2.

```sh
docker run --name postgres_2022 -e POSTGRES_PASSWORD=postgres_2022 -p 5400:5432 --network devops_2022 -v data_psql:/var/lib/postgresql/data postgres:12
```

11. Probar la conexión a la base de datos postgres desde el servicio cloudbeaver.

Las credenciales a usar deben ser:

- host: postgres_2022
- port: 5432
- database_name: postgres
- username: postgres
- password: postgres_2022

12. Crear un archivo __"Dockerfile.app"__ que permita instalar las dependencias del proyecto Pokeapp y generar la carpeta dist, luego copiar esa carpeta en una imagen de nginx.

```Dockerfile
#stage 1
FROM node:latest as node
WORKDIR /app
COPY . .
RUN npm install
# En caso de presentar error en el proceso de build relacionado a ssl/tls, descomentar la siguiente linea:
# ENV NODE_OPTIONS=--openssl-legacy-provider
RUN npm run build --prod

#stage 2
FROM nginx:alpine
COPY --from=node /app/dist/PokeApp /usr/share/nginx/html
```

```sh
# Debe estar ubicado en la carpeta docker/Pokeapp
docker build -t pokeapp -f Dockerfile.app .
```

13. Crear una imagen en base al archivo __"Dockerfile.app"__ creado en el punto anterior con el nombre de su repositorio de Docker Hub y subir su imagen a su repositorio.

```sh
# Luego de crear el repositorio de DockerHub, loguearse mediante:
docker login -u <username> -p <token>
# Pull de la imagen
docker tag pokeapp <username>/<repositorio>:<tag>
docker pull <username>/<repositorio>:<tag>
```

14. En base a la documentacion de [HackMD](https://hackmd.io/c/codimd-documentation/%2Fs%2Fcodimd-docker-deployment), crear un manifiesto de 
docker compose con las siguientes configuraciones:

    - Una base de datos postgres con la imagen postgres:12-alpine.
    - La base de datos debe tener un usuario __devops2022__, una contraseña __devopscourse2022__ y el nombre de la base de datos __testing__.
    - La base de datos debe estar en el puerto 9200 del host y el nombre del contenedor debe ser __pg_hackmd__.
    - En el servicio de hackmd, el nombre del contenedor debe ser __main_hackmd__ y debe estar en el puerto 9300 del host.
    - Los servicios de hackmd y postgres deben tener los volumenes __hackmd_data__ y __psql_data__ respectivamente.
    - El servicio de hackmd debe tener, en adición a las variables que proporciona la documentación, las siguientes variables de entorno:

        - CMD_ALLOW_ORIGIN: "localhost,127.0.0.1"
        - CMD_SESSION_LIFE: 28800000
        - CMD_ALLOW_PDF_EXPORT: "true"
        - CMD_EMAIL: "true"
        - CMD_ALLOW_EMAIL_REGISTER: "true"

```yaml
# touch compose_1.yml
version: '3'

services:
  metabase-app:
    image: metabase/metabase
    restart: unless-stopped
    ports:
      - 3000:3000
    environment:
      MB_DB_TYPE: mysql
      MB_DB_DBNAME: metabase
      MB_DB_PORT: 3306
      MB_DB_USER: metabase
      MB_DB_PASS: metabase
      MB_DB_HOST: db-mysql
    depends_on:
      - db-mysql
  db-mysql:
    image: mysql:5
    restart: unless-stopped
    ports:
      - 3306:3306
    environment:
      MYSQL_ROOT_PASSWORD: devops2022
      MYSQL_DATABASE: metabase
      MYSQL_USER: metabase
      MYSQL_PASSWORD: metabase
```