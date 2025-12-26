# Build stage
FROM maven:3.8.4-openjdk-17 AS build
WORKDIR /app

# Copy only the pom.xml file and download dependencies
COPY pom.xml .
RUN mvn dependency:go-offline

# Copy the rest of the source code
COPY src ./src

# Run the build, using cached dependencies if they exist
RUN mvn clean package -DskipTests

# Run stage
FROM eclipse-temurin:21-jdk

# Install Maven in the runtime image
RUN apt-get update && apt-get install -y maven && apt-get clean

WORKDIR /app
COPY --from=build /app/target/*.jar /app/myapp.jar
#EXPOSE 8088
ENTRYPOINT ["java","-jar","/app/myapp.jar"]