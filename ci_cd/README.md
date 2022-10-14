# Practicas - Parte I

## Ejercicios Previos**

1. Crear un repositorio en gitlab con el nombre __ci_cd_parte_1_pokeapp__, sin README previo.
2. Configurar un pipeline con 3 stages (test, build, deploy) y un job por cada stage con un script

```sh
echo "This is the stage <change-name-stage>"
```


3. Modificar los jobs para que se disparen solo cuando se realice un cambio en la rama develop. ¿Qué sucede con el pipeline?
4. Copiar el proyecto __PokeApp__ de la práctica de __docker__ y renombrar la carpeta como __app__.
5. Crear un token en DockerHub con los permisos de lectura/escritura.
6. Crear un repositorio en DockerHub con el nombre __pokeapp_2022__.
7. En el job de __test__ configurar un script para disparar el linter configurado en el proyecto con el commando:

```sh
npm run lint
```

8. En el job de __build__ configurar los scripts necesarios para hacer un build de la imagen docker de __app__ y subirlo a DockerHub en el repositorio creado en el paso __6__. Utilizar el hahs del commit como tag para la imagen docker.
9. Al terminar de realizar los ejercicios, eliminar el token de dockerhub.


__** Al final de cada punto a partir de este, debe realizarse un commit y hacer un push de los cambios al repositorio.__
