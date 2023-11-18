FROM maven:3.8.6-openjdk-11 as builder
COPY . .
RUN mvn dependency:resolve
RUN mvn clean package -DskipTests=true

FROM openjdk:11
VOLUME /tmp
COPY --from=builder ./target/*.jar application.jar
EXPOSE 8761

ENTRYPOINT ["java", "-jar", "/application.jar"]