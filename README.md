# Config Service

A centralized configuration server for the Polar Bookshop microservices ecosystem, built with Spring Cloud Config Server.

## Overview

This service provides externalized configuration˚ management for distributed applications. It serves configuration properties from a Git repository, enabling dynamic configuration updates without requiring application restarts.

## Features

- **Centralized Configuration**: Single source of truth for all application configurations
- **Git-backed Storage**: Configuration stored in version control for easy tracking and rollback
- **Multiple Environment Support**: Manage configurations for different environments (dev, staging, production)
- **Real-time Updates**: Applications can refresh their configuration without downtime
- **RESTful API**: Simple HTTP-based access to configuration properties

## Technology Stack

- **Java**: 21
- **Spring Boot**: 3.5.7
- **Spring Cloud Config Server**: 2025.0.0
- **Build Tool**: Gradle

## Prerequisites

- Java 21 or higher
- Git

## Configuration

The service is configured to:
- Run on port **8888**
- Fetch configurations from: `https://github.com/fisnikshabani/config-repo`
- Use the `main` branch as the default label
- Clone the repository on startup
- Force pull updates from the remote repository

### Server Configuration

```yaml
server:
  port: 8888
  tomcat:
    connection-timeout: 2s
    keep-alive-timeout: 15s
    threads:
      max: 50
      min-spare: 5
```

## Getting Started

### Build the Project

```bash
./gradlew build
```

### Run the Application

```bash
./gradlew bootRun
```

Or run the JAR directly:

```bash
java -jar build/libs/config-service-0.0.1-SNAPSHOT.jar
```

### Verify the Service

Once the application is running, you can access configurations at:

```
http://localhost:8888/{application}/{profile}/{label}
```

Example:
```
http://localhost:8888/catalog-service/default/main
```

## API Endpoints

The Spring Cloud Config Server exposes several endpoints to retrieve configuration:

- `/{application}/{profile}[/{label}]`
- `/{application}-{profile}.yml`
- `/{application}-{profile}.properties`
- `/{label}/{application}-{profile}.yml`
- `/{label}/{application}-{profile}.properties`

Where:
- `{application}`: The name of the client application
- `{profile}`: The active profile (e.g., dev, prod)
- `{label}`: The Git branch/tag (defaults to main)

## Configuration Repository Structure

Configuration files should be organized in the Git repository as:

```
config-repo/
├── application.yml          # Common configuration for all applications
├── catalog-service.yml      # Configuration for catalog service
├── catalog-service-dev.yml  # Development profile for catalog service
└── catalog-service-prod.yml # Production profile for catalog service
```

## Testing

Run the test suite:

```bash
./gradlew test
```

## Project Structure

```
config-service/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/polarbookshop/configservice/
│   │   │       └── ConfigServiceApplication.java
│   │   └── resources/
│   │       └── application.yml
│   └── test/
│       └── java/
│           └── com/polarbookshop/configservice/
│               └── ConfigServiceApplicationTests.java
├── build.gradle
├── settings.gradle
└── README.md
```

## Development

### Gradle Wrapper

The project includes Gradle wrapper scripts:
- Unix/Mac: `./gradlew`
- Windows: `gradlew.bat`

No need to install Gradle separately.

## License

This project is part of the Polar Bookshop microservices system.

## Related Repositories

- [Config Repository](https://github.com/fisnikshabani/config-repo) - Git repository containing the configuration files

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request