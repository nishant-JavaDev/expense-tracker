# Stage 1: Build the application
FROM maven:3.8.5-openjdk-17 AS build

# Set the working directory inside the container
WORKDIR /app

# Copy the pom.xml and download dependencies
COPY expensetracker/pom.xml .  # Copy pom.xml to /app
RUN mvn dependency:go-offline    # Download all dependencies

# Now copy the source code to the container
COPY expensetracker/src ./src      # Copy expensetracker/src to /app/src

# Build the project and skip tests
RUN mvn clean package -DskipTests

# Stage 2: Create the final image
FROM openjdk:17.0.1-jdk-slim

# Copy the JAR file from the build stage
COPY --from=build /app/target/expensetracker-0.0.1-SNAPSHOT.jar expensetracker.jar

# Set the entrypoint to run the JAR file with the dynamic port
ENTRYPOINT ["java", "-jar", "-Dserver.port=${PORT}", "expensetracker.jar"]
