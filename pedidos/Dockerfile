FROM maven:3.9.9-eclipse-temurin-17-focal AS build 

# Definindo um diretório de trabalho dentro do nosso container
WORKDIR /app

# Copiando esse arquivo para o diretório de trabalho app - Isso é feito passando o nome do arquivo + .
# Esse arquivo pom.xml é o arquivo que contém as informações do projeto, como as dependências, plugins, etc.
COPY pom.xml .

# Copiando o código que está em src para o diretório de trabalho app/src
COPY src ./src

# Comando para rodar o maven com essas informações
RUN mvn clean package -DskipTests

FROM eclipse-temurin:17.0.14_7-jre-focal
WORKDIR /app
COPY --from=build /app/target/pedidos-0.0.1-SNAPSHOT.jar .
CMD ["java", "-jar", "pedidos-0.0.1-SNAPSHOT.jar"]