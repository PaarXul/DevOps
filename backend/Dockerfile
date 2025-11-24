# Build stage: compila Spring Boot
FROM maven:3.9.6-eclipse-temurin-17 AS build
WORKDIR /app

# Copiar archivos de Maven
COPY pom.xml .
# Descargar dependencias (cacheable)
RUN mvn -B -q -e -DskipTests dependency:go-offline

# Copiar el código fuente
COPY src ./src

# Build del JAR (salida en target)
RUN mvn -B -DskipTests package

# Runtime stage: imagen ligera para ejecutar
FROM eclipse-temurin:17-jre
WORKDIR /app

# Opcional: variables de entorno
ENV JAVA_OPTS=""

# Copia el jar construido
COPY --from=build /app/target/*.jar /app/app.jar

EXPOSE 8080

# Ejecuta la aplicación
ENTRYPOINT ["sh", "-c", "java $JAVA_OPTS -jar /app/app.jar"]
