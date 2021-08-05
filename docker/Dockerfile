##Imagen para compilar el proyecto de git
FROM alpine:20210804 as builder
MAINTAINER un@mail.com
RUN apk add maven git
RUN git clone https://github.com/Mencrypto/demoSpringBootDockerMySQLAWS.git && cd demoSpringBootDockerMySQLAWS && mvn clean install -Dmaven.test.skip=true

#Imagen final que copia el jar generado en el paso anterior
FROM frolvlad/alpine-oraclejre8:slim
MAINTAINER un@mail.com
COPY --from=builder ["/demoSpringBootDockerMySQLAWS/target/*.jar", "/app.jar"]
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
EXPOSE 8081