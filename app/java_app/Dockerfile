#Stage 1
# initialize build and set base image for first stage
FROM maven:3.8.5-openjdk-18-slim as stage1
# speed up Maven JVM a bit
ENV MAVEN_OPTS="-XX:+TieredCompilation -XX:TieredStopAtLevel=1"
# set working directory
WORKDIR /app
# copy just pom.xml
COPY pom.xml .
# go-offline using the pom.xml
RUN mvn dependency:go-offline
# copy your other files
COPY ./src ./src
# compile the source code and package it in a jar file
RUN mvn clean package -Dmaven.test.skip=true
#Stage 2
# set base image for second stage
FROM openjdk:18-slim
# set deployment directory
WORKDIR /app
# copy over the built artifact from the maven image
COPY --from=stage1 /app/target/world-0.0.1-SNAPSHOT.jar /app/app.jar
# add information about expose port
EXPOSE 8080
# entrypoint and command that will be run when container starts
ENTRYPOINT []
CMD ["java","-jar","./app.jar"]