# -----------------------
# Etapa 1: Construir la app
# -----------------------
FROM maven:3.9.5-eclipse-temurin-21 AS build

# Directorio de trabajo dentro del contenedor
WORKDIR /app

# Copiar pom.xml y descargar dependencias offline para acelerar la construcción
COPY pom.xml .
RUN mvn dependency:go-offline

# Copiar todo el proyecto
COPY . .

# Construir el proyecto y saltar los tests para acelerar
RUN mvn clean package -DskipTests

# -----------------------
# Etapa 2: Ejecutar el JAR
# -----------------------
FROM eclipse-temurin:21-jdk

WORKDIR /app

# Copiar el JAR generado en la etapa de build
COPY --from=build /app/target/*.jar app.jar

# Puerto en el que el backend escuchará
EXPOSE 8080

# Comando para ejecutar el backend
CMD ["java", "-jar", "app.jar"]

