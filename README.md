# Kubernetes (K8s) 🚀  

Antes de mais nada, é importante entender que o **Kubernetes** é um **orquestrador de containers**. Podemos compará-lo a um maestro que gerencia diversos músicos em uma orquestra, garantindo que cada um desempenhe seu papel corretamente.  

Para gerenciar containers, primeiro precisamos **criar os containers dos microserviços**. O primeiro passo para isso é criar um **Dockerfile**, que servirá como base para construir nossos containers e executar o código dos microserviços.  

## 📝 Anotações - Docker  

### 📌 1. Criando o Dockerfile  

O primeiro passo é criar um **Dockerfile** genérico para nossa aplicação:  

```dockerfile
# Etapa de construção do container
# O 'AS build' é utilizado para criar uma fase de build otimizada, reduzindo a imagem final
FROM maven:3.9.9-eclipse-temurin-17-focal AS build 

# Define o diretório de trabalho dentro do container
WORKDIR /app

# Copia o arquivo pom.xml (contém informações do projeto, dependências e plugins)
COPY pom.xml .

# Copia o código-fonte para o diretório do container
COPY src ./src

# Compila o projeto, pulando os testes para agilizar o processo
RUN mvn clean package -DskipTests

# Criação da imagem final com apenas o JRE necessário para rodar a aplicação
FROM eclipse-temurin:17.0.14_7-jre-focal

# Define o diretório de trabalho do container
WORKDIR /app

# Copia o arquivo .jar gerado na etapa de build
COPY --from=build /app/target/<artifactId>-<version>.jar .

# Define o comando para executar a aplicação
CMD ["java", "-jar", "<artifactId>-<version>.jar"]
```

💡 **Observação**: Estamos lidando com arquivos `.jar` (Java). Portanto, para identificar corretamente o **`artifactId`** e a **`version`**, é necessário consultar o arquivo `pom.xml` do projeto.  

### 📌 2. Construindo e publicando a imagem no Docker Hub  

Após criar o **Dockerfile** para cada microserviço, precisamos **construir a imagem** e **publicá-la** em um repositório.  

#### 🔨 Construindo a imagem  

```sh
docker build -t lucena5378/java-server-k8s:v1 server/.
```

#### 📤 Enviando a imagem para o Docker Hub  

```sh
docker push lucena5378/java-server-k8s:v1
```

Após isso, no  ```Docker Desktop```, a imagem aparecerá da seguinte forma:

![Docker Desktop](.\utils\img\docker_desktop_2.png)