FROM eclipse-temurin:21-jre
WORKDIR /projectA
COPY target/*.jar projectA.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","projectA.jar"]
