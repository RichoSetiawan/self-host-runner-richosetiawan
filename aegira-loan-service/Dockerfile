# Build stage - Upgrade to Java 17
FROM maven:3.9.6-eclipse-temurin-17-alpine AS build
 
WORKDIR /app
 
COPY pom.xml .
COPY src ./src
 
# The handshake error should resolve with the updated CA certificates in this image
RUN mvn -q -DskipTests package
 
# Runtime stage - Upgrade to Java 17
FROM eclipse-temurin:17-jre-alpine
 
WORKDIR /app
 
COPY --from=build /app/target/aegira-loan-service-0.0.1-SNAPSHOT.jar app.jar
 
EXPOSE 8080
 
ENTRYPOINT ["java", "-jar", "app.jar"]

# Jlink Super Ringan
# FROM maven:3.9.6-eclipse-temurin-17-alpine AS deps
# WORKDIR /app
# COPY pom.xml .
# RUN mvn dependency:go-offline -B

# FROM maven:3.9.6-eclipse-temurin-17-alpine AS build
# WORKDIR /app
# COPY --from=deps /root/.m2 /root/.m2
# COPY --from=deps /app/pom.xml .
# COPY src ./src
# RUN mvn -q -DskipTests package

# FROM eclipse-temurin:17-jdk-alpine AS jre-builder
# RUN $JAVA_HOME/bin/jlink \
#     --add-modules java.base,java.logging,java.desktop,java.management,java.naming,java.sql,java.security.jgss,java.instrument,java.rmi,java.prefs,jdk.unsupported \
#     --strip-debug \
#     --no-man-pages \
#     --no-header-files \
#     --compress=2 \
#     --output /custom-jre

# FROM alpine:3.20

# RUN addgroup -S spring && adduser -S spring -G spring
# USER spring:spring

# WORKDIR /app

# COPY --from=jre-builder /custom-jre /opt/jre
# COPY --from=build /app/target/aegira-loan-service-0.0.1-SNAPSHOT.jar app.jar

# ENV JAVA_HOME=/opt/jre
# ENV PATH="${JAVA_HOME}/bin:${PATH}"

# EXPOSE 8080

# ENTRYPOINT ["java", "-jar", "app.jar"]