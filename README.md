# README

## Requisitos

Se debe tener una máquina preferentemente Linux con docker y docker-compose 
instalados y una cuenta de Docker Hub y clonar este repositorio

## Ejecución

Se requiere una máquina con docker y docker-compose 
    
Se exporta una variable de ambiente con el nombre de tu usuario de Docker Hub y se 
crea una imagen ligera de jdk8 en la que se compila el proyecto:
https://github.com/Mencrypto/demoSpringBootDockerMySQLAWS
El docker file tiene dos fases, en la primera en una imagen de alpine, se instala mvn y git y en ella
se compila el proyecto y en la segunda fase, en una imagen ligera se agrega solo el jar producto de la primer fase

    export user=Mencrypto
    docker build -f docker/Dockerfile ./docker
    docker tag $(docker image ls -qa | head -n 1) ${user}/demospringboot:v0
Una vez creada la imagen se deben autenticar para conectar al repositorio Docker Hub

    docker login
        Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
        Username: ${user}
        Password:
        WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
        Configure a credential helper to remove this warning. See
        https://docs.docker.com/engine/reference/commandline/login/#credentials-store

        Login Succeeded


Posterior se debe hacer push de la imagen

    docker push ${user}/demospringboot:v0 

## Visualización

Al iniciar el stack de docker-compose se debe levantar una base de datos mysql, dos instancias del proyecto de Spring Boot
y una instancia de balanceo de docker cloud

    docker-compose -f docker-compose/docker-compose.yml up -d

Para conectarse a la aplicación se puede realizar por el puerto 80 y los logs del balanceo se pueden consultar con:

    docker-compose -f docker-compose/docker-compose.yml logs -f

## Bajar los contenedores

Para evitar que los contenedores sigan corriendo y consumiento recursos se deben dar de baja

    docker-compose -f docker-compose/docker-compose.yml down
    