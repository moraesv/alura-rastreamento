# Etapa de Build
FROM maven:3.8.6-openjdk-11 AS builder

WORKDIR /usr/src/app

# Copia os arquivos do projeto e faz o build
COPY . .
RUN mvn clean package -DskipTests

# Etapa Final (Imagem Leve)
FROM openjdk:11-jre-slim

WORKDIR /usr/src/myapp

# Define variável para o Agente OpenTelemetry
ARG OLTP=opentelemetry/opentelemetry-javaagent.jar

# Copia o JAR do build
COPY --from=builder /usr/src/app/target/*.jar api-cursos.jar

# Copia o Agente OpenTelemetry
COPY ${OLTP} opentelemetry-javaagent.jar

# Define Timezone
RUN ln -sf /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime

RUN apt install curl -y

# Comando de inicialização
CMD ["java", "-Xms256M", "-Xmx256M", "-XX:MetaspaceSize=128m", "-XX:MaxMetaspaceSize=256m", "-javaagent:opentelemetry-javaagent.jar", "-jar", "api-cursos.jar"]
