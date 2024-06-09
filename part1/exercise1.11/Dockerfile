# Install java 8
FROM amazoncorretto:8

# Set workdir
WORKDIR /usr/src

# Copy files from project to docker image
COPY . .

# Build maven package
RUN ./mvnw package

# Run
CMD ["java", "-jar", "./target/docker-example-1.1.3.jar"]

# Open port 8080
EXPOSE 8080
