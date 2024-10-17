# Stage 1: Build the application
FROM maven:3.8.5-openjdk-17 AS build

# Set the working directory
WORKDIR /app

# Copy the pom.xml and download dependencies
COPY pom.xml .
RUN mvn dependency:go-offline

# Now copy the source code and build the project
COPY src ./src
RUN mvn clean package -DskipTests

# Stage 2: Create the final image
FROM openjdk:17.0.1-jdk-slim
COPY --from=build /app/target/expensetracker-0.0.1-SNAPSHOT.jar expensetracker.jar

# Use the dynamic port provided by Render
ENTRYPOINT ["java", "-jar", "-Dserver.port=${PORT}", "expensetracker.jar"]
