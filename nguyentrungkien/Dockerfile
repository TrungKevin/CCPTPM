FROM maven:3.9.9-eclipse-temurin-21 AS build
WORKDIR /app

# Copy build metadata first to leverage Docker layer caching.
COPY pom.xml .
RUN mvn -B -DskipTests dependency:go-offline

# Copy source and build Spring Boot jar.
COPY src ./src
RUN mvn -B -DskipTests clean package

FROM eclipse-temurin:21-jre
WORKDIR /app

COPY --from=build /app/target/*.jar app.jar

EXPOSE 8080
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
