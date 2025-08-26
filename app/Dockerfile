

# ------------------------------
#      Build Stage (Maven)
# ------------------------------
FROM maven:3.9.9-eclipse-temurin-21-jammy AS builder

# Set working directory inside build container
WORKDIR /app

# Copy pom.xml separately to leverage layer caching for dependencies
COPY pom.xml .

# Copy full source code and build the WAR file (skip tests for speed)
COPY src/ ./src/
RUN mvn install


# ------------------------------
#     Runtime Stage (Tomcat)
# ------------------------------
FROM tomcat:10.1-jdk21-temurin

# Set working directory inside the runtime container
WORKDIR /usr/local/tomcat

# Remove default Tomcat applications (clean slate)
RUN rm -rf webapps/*

# Copy the WAR file built in the previous stage into ROOT.war (auto-deployed by Tomcat)
COPY --from=builder /app/target/vprofile-v2.war webapps/ROOT.war

# Expose default Tomcat port
EXPOSE 8080

# Start Tomcat when container runs
CMD ["catalina.sh", "run"]
