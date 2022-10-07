# Prácticas

Los siguientes ejercicios corresponden a prácticas de la unidad Docker del curso de DevOps 2022

## Ejercicios previos

1. Desplegar un servicio de nginx por el puerto 8080 del host en background.
2. Crear una red docker con el nombre "devops_2022".
3. Inspeccionar los metadatos del contenedor desplegado en el ejercicio 1.
4. Crear un volumen docker con el nombre "data_mysql".
5. Desplegar una base de datos mysql con la contraseña de root "devops_2022", con el volumen "data_mysql" para persistir los datos de la misma,
    en la red docker creada en el paso 2 y escuchando el puerto 4001 del host en background.
6. Crear un archivo "Dockerfile.nginx", copiar el contenido del la carpeta html en la carpeta html de la imagen de nginx oficial. Hacer el build
    de la imagen basado en dicho archivo y desplegar el servicio con dicha imagen en el puerto 8081 del host en background.
7. Crear un manifiesto de docker compose "compose_1.yml" con un servicio de metabase y una base de datos mysql. Correr los servicios en primer plano.

## Ejercicios de práctica

8. Desplegar un cloudbeaver en el puerto 8978 del host, en la red docker creada en el paso 2 y en background.
9. Crear un volumen docker con el nombre "data_psql"
10. Desplegar una base de datos postgres con la contraseña "postgres_2022" en el puerto 5400 del host y persistir los datos en el volumen creado
en el ejercicio 9. El servicio debe correr en primer plano. La red del servicio debe ser la creada en el ejercicio 2.
11. Probar la conexión a la base de datos postgres desde el servicio cloudbeaver.
12. Crear un archivo Dockerfile.app que permita instalar las dependencias del proyecto Pokeapp y generar la carpeta dist, luego copiar esa carpeta en una imagen de nginx.
13. Crear una imagen en base al archivo Dockerfile.app creado en el punto anterior con el nombre de su repositorio de Docker Hub y subir su imagen a su repositorio.
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
        - CMD_ALLOW_EMAIL_REGISTER: "false"