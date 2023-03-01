# java-spring-hello
An example of Java project with Spring Boot. Included:
- [actuator module](https://docs.spring.io/spring-boot/docs/2.5.0/reference/htmlsingle/#actuator)

# Requirements
- java 18 and later
- maven 3.8 and later

## Docker

### Build
Build container from [Dockerfile](Dockerfile) and set the tag `java-spring-hello`.
```
docker build -t java-spring-hello .
```

### Run
Use `java-spring-hello` image to start a container named java-spring-hello and map `8080` port inside the container to the `8080` port on my host machine.
```
docker run -p 8080:8080 --rm --name=java-spring-hello java-spring-hello
```

Test in an other terminal window
```
$ curl localhost:8080
Hello world!%
$ curl localhost:8080/actuator/prometheus
```