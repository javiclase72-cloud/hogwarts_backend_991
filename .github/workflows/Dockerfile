# ====== Etapa 1: Fase de construcción (es temporal) ======
# Usamos una imagen de Maven con JDK 23 para construir la aplicación
FROM maven:3.9-eclipse-temurin-23 AS imagen_construccion

WORKDIR /app

# Copiamos el pom y el código fuente
COPY pom.xml .
COPY src ./src

# Construimos el proyecto omitiendo los tests para acelerar el proceso
RUN mvn clean package -DskipTests

# ====== Etapa 2: Fase de ejecución (la imagen final) ======
# Usamos solo el JRE para que la imagen sea mucho más ligera
FROM eclipse-temurin:23-jre AS imagen_ejecucion

WORKDIR /app

# Copiamos el .jar generado en la etapa anterior y lo renombramos a app.jar
COPY --from=imagen_construccion /app/target/*.jar app.jar

# Puerto típico de Spring Boot
EXPOSE 8080

# Comando de arranque apuntando al jar simplificado
ENTRYPOINT ["java", "-jar", "app.jar"]
