# Base image for essential tools
FROM alpine:3.18 AS base
RUN apk update && \
    apk upgrade --available && \
    apk add --no-cache \
    busybox \
    openssl \
    zlib

# Build stage with Maven and OpenJDK
FROM openjdk:17-jdk-alpine AS build
RUN apk update && \
    apk upgrade --available && \
    apk add --no-cache maven

WORKDIR /workspace
COPY pom.xml .        
COPY src /workspace/src 

# Build the application, skipping tests
RUN mvn clean package -DskipTests

# Verify build outputs
RUN ls -l /workspace/target

# Final stage with OpenJDK to run the application
FROM openjdk:17-jdk-alpine
RUN apk update && \
    apk upgrade --available

WORKDIR /app
COPY --from=build /workspace/target/*.jar app.jar

# Expose the application port
EXPOSE 8080

# Run the application
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
