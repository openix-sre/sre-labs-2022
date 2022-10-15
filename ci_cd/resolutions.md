# Resolución de los ejercicios

En este apartado se detalla los comandos para la resolución de los ejercicios de la unidad "CI/CD"

## Ejercicios previos - Parte I

2. Configurar un pipeline con 3 stages (test, build, deploy) y un job por cada stage con un script

```sh
echo "This is the stage <change-name-stage>"
```

```yaml
stages:
  - test
  - build
  - deploy

test-job:
  stage: test
  script:
    - echo "This is the stage test"

build-job:
  stage: build
  script:
    - echo "This is the stage build"

deploy-job:
  stage: deploy
  script:
    - echo "This is the stage deploy"
```

2. En el job de __test__ configurar un script para disparar el linter configurado en el proyecto con el commando:

```sh
npm run lint
```

```yaml
stages:
  - test
  - build
  - deploy

test-job:
  stage: test
  image: node:14
  script:
    - echo "This is the stage test"
    - cd app
    - npm install
    - npm run lint

build-job:
  stage: build
  script:
    - echo "This is the stage build"

deploy-job:
  stage: deploy
  script:
    - echo "This is the stage deploy"
```

8. En el job de __build__ configurar los scripts necesarios para hacer un build de la imagen docker de __app__ y subirlo a DockerHub en el repositorio creado en el paso __6__.

```yaml
variables:
  DOCKER_HOST: tcp://docker:2376
  DOCKER_TLS_CERTDIR: "/certs"

services:
  - docker:19.03.12-dind

stages:
  - test
  - build
  - deploy

test-job:
  stage: test
  image: node:14
  script:
    - echo "This is the stage test"
    - cd app
    - npm install
    - npm run lint

build-job:
  stage: build
  image: docker:19.03.12
  script:
    - echo "This is the stage build"
    - cd app
    - docker login -u <user-account> -p <user-token>
    - docker build -f Dockerfile.dev -t docker.io/<user-account>/pokeapp_2022:$CI_COMMIT_SHA .
    - docker push docker.io/<user-account>/pokeapp_2022:$CI_COMMIT_SHA

deploy-job:
  stage: deploy
  script:
    - echo "This is the stage deploy"
```