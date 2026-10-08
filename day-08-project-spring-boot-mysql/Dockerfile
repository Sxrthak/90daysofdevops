#STAGE 1 - Build JAR 
FROM maven:3.9-eclipse-temurin-17 AS builder

WORKDIR /app

COPY . /app

#create JAR file
RUN mvn clean install -DskipTests=true

#Stage 2 - execute JAR file from above stage
FROM eclipse-temurin:17-jre-alpine

WORKDIR /app

COPY --from=builder /app/target/*.jar /app/expenseapp.jar

CMD ["java", "-jar", "expenseapp.jar"]


