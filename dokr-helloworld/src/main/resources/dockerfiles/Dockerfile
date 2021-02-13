# use base image as openjdk version 8
FROM openjdk:8
# Copy jar file from local to docker image
COPY "maven/target/dokr-helloworld-1.0-SNAPSHOT-spring-boot.jar" /dokr-helloworld.jar
# set the startup command to execute the jar
CMD ["java", "-jar", "/dokr-helloworld.jar"]

