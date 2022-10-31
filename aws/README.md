# Practicas - Parte I

## Ejercicios Previos**

1. Navegar por la consola de AWS ver servicios, dashboard de Pagos y cambio de Regiones.
2. Crear un Usuario con la policy __AdministratorAccess__ y con el nombre de usuario __nombre_usuario-admin__ con el servicio IAM con permisos para acceder por la consola de AWS y con acceso programatico.
3. Log out como usuario root.
4. Acceder con el Usuario creado en el punto 2.
5. Configurar aws cli con el access y secret key creado en el punto 2.
6. Verificar acceso a los servicios mediante aws cli.
7. Crear un Usuario con la policy __AmazonS3FullAccess__ y con el nombre de usuario __terraform-dev__ con el servicio IAM con permisos para acceder solo con acceso programatico.
8. Configurar aws cli con un perfil llamado __terraform-dev__.
9. Crear una instancia EC2.

## Ejercicios de Práctica

10. Log in con usuario Root y verificar full access navegando por la consola de AWS.
11. Crear un Usuario con la policy __AdministratorAccess__ y con el nombre de usuario __nombre_usuario-admin__ con el servicio IAM con permisos para acceder por la consola de AWS y con acceso programatico.
12. Acceder a la consola de AWS con el Usuario creado en el punto 11.
13. Configurar aws cli con el access y secret key creado en el punto 11 con el perfil __nombre_usuario-admin__.
14. Verificar acceso a los servicios de S3 listando los bucket, y describiendo las instancias actuales.
15. Crear una instancia EC2 con las siguientes especificaciones:
    a. Cambiar a la region us-east-2 US EAST (Ohio).

    b. Nombre de la instancia __ec2-nombre_usuario-test__.

    c. Tipo de AMI __Ubuntu 18.04__.

    d. Tipo de instancia __t2.micro__

    e. Crear un nuevo __key-pair__ con el nombre __key-pair-nombre_usuario__.

    f. Crear un nuevo __security-group__ con acceso al puerto 22 SSH con permisos de acceso solo para su IP.

    g. Tamaños de Disco __8GB y SSD general purpose__.

    h. Usar __user-data__ para crear una carpeta con el __nombre_usuario-user-data__.

    i. Acceder a la instancia y verificar si la carpeta fue creada.
    
