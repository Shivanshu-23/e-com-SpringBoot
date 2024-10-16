# Use official openjdk 21 image from the Docker Hub as a base image for building
FROM openjdk:21-slim AS build

# Set the working directory inside the container
WORKDIR /app

# Install Maven
RUN apt-get update && apt-get install -y maven

# Copy the Maven project files into the container
COPY pom.xml .

# Download and install dependencies (this is optional but helps cache dependencies)
RUN mvn dependency:go-offline

# Copy the entire project into the container
COPY src ./src

# Build the Spring Boot JAR file
RUN mvn clean package -DskipTests

# Use the same openjdk 21 slim image for running the app
FROM openjdk:21-slim

# Set the working directory inside the container
WORKDIR /app

# Copy the JAR file from the build stage
COPY --from=build /app/target/ecom-proj-0.0.1-SNAPSHOT.jar /app/app.jar

# Expose the port that your Spring Boot application runs on
EXPOSE 8080

# Command to run the Spring Boot application
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
