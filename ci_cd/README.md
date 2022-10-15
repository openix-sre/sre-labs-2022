# Practicas - Parte I

## Ejercicios Previos**

1. Crear un repositorio en gitlab con el nombre __ci_cd_parte_1_pokeapp__, sin README previo.
2. Configurar un pipeline con 3 stages (test, build, deploy) y un job por cada stage con un script

```sh
echo "This is the stage <change-name-stage>"
```


3. Modificar los jobs para que se disparen solo cuando se realice un cambio en la rama __develop__. ¿Qué sucede con el pipeline?. Luego modificar para que se dispare en la rama __main__.
4. Copiar el proyecto __PokeApp__ de la práctica de __docker__ y renombrar la carpeta como __app__. Ademas, copiar el __Dockerfile.dev__ creado para dicho proyecto.
5. Crear un token en DockerHub con los permisos de lectura/escritura.
6. Crear un repositorio en DockerHub con el nombre __pokeapp_2022__.
7. En el job de __test__ configurar un script para disparar el linter configurado en el proyecto con el commando:

```sh
npm run lint
```

8. En el job de __build__ configurar los scripts necesarios para hacer un build de la imagen docker de __app__ y subirlo a DockerHub en el repositorio creado en el paso __6__. Utilizar el hash del commit como tag para la imagen docker.
9. Al terminar de realizar los ejercicios, eliminar el token de dockerhub.


__** Al final de cada punto a partir de este, debe realizarse un commit y hacer un push de los cambios al repositorio.__

## Ejercicios de Práctica

10. Modificar el siguiente fragmento de código en __app/src/app/components/poke-detail/poke-detail.component.ts__. Realizar el commit correspondiente y analizar el comportamiento del pipeline.

```js
// line 16 - old code
  constructor(
    private activatedRouter: ActivatedRoute,
    private pokemonService: PokemonService
  ) {

    this.activatedRouter.params.subscribe(
      params => {
        this.getPokemon(params.id);
      }
    );
  }

// new code
  constructor(private activatedRouter: ActivatedRoute,
    private pokemonService: PokemonService) {

    this.activatedRouter.params.subscribe(
      params => {
        this.getPokemon(params.id);
      }
    );
  }
```

11. Deshacer el último commit realizado para recuperar un estado anterior y realizar el push de los cambios. ¿Cuál es el estado final del pipeline (exitoso o con errores)?
12. En nuestros jobs de test y build se repiten algunas líneas de código. Buscar en la documentación de gitlab-ci la sintaxis de __default__ y agregar dichas líneas en esta sección como un __before_script__.