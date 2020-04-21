#FROM jetty:9.4.27-jdk13-slim
FROM openjdk:13-alpine
LABEL maintainer="Maicon Matos <maicon.matos@zeusagro.com>"

ENV DB_URL jdbc:postgresql://dbagribusiness.cbg4reoktyye.us-east-1.rds.amazonaws.com:5432/conference_app
ENV DB_USER agribusiness
ENV DB_PASSWORD wcofKanban

WORKDIR /api

ARG JAR_FILE=start.jar
COPY ${JAR_FILE} /api/app.jar

EXPOSE 5000

ENTRYPOINT ["java","-jar","/app.jar"]