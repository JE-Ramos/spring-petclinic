# Spring PetClinic - Project Guide

## Setup Commands

### Prerequisites
- Java 25 or later (build requirement)
- Java 17 or newer (runtime requirement)
- Maven (wrapper included: `./mvnw`)
- SDKMAN (recommended): `sdk install java 25.0.1-open`

### Development Commands
```bash
# Build the application
./mvnw clean package

# Run the application (with auto-reload)
./mvnw spring-boot:run

# Run tests
./mvnw test

# Build Docker image
./mvnw spring-boot:build-image

# Run with specific database profile
./mvnw spring-boot:run -Dspring-boot.run.profiles=mysql
./mvnw spring-boot:run -Dspring-boot.run.profiles=postgres
```

### Database Setup
```bash
# Start MySQL with Docker
docker run -e MYSQL_USER=petclinic -e MYSQL_PASSWORD=petclinic \
  -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=petclinic \
  -p 3306:3306 mysql:9.2

# Or use docker-compose
docker compose up mysql

# Start PostgreSQL
docker compose up postgres
```

### Access Points
- Application: http://localhost:8080
- H2 Console: http://localhost:8080/h2-console
- API Endpoints: http://localhost:8080/owners, /vets, /pets

## Project Structure

```
spring-petclinic/
├── src/main/java/org/springframework/samples/petclinic/
│   ├── model/          # Domain entities (BaseEntity, NamedEntity, Person)
│   ├── owner/          # Owner, Pet, Visit controllers & repositories
│   ├── vet/            # Veterinarian controllers & repositories
│   └── system/         # Configuration, cache, web setup
├── src/main/resources/
│   ├── db/             # Database schemas (H2, MySQL, PostgreSQL)
│   ├── templates/      # Thymeleaf HTML templates
│   └── static/         # CSS, images, fonts
└── src/test/java/      # Unit and integration tests
```

## Architecture & Patterns

### Technology Stack
- **Framework**: Spring Boot 4.0.0-M3
- **Web**: Spring MVC + Thymeleaf
- **Data**: Spring Data JPA
- **Database**: H2 (default), MySQL, PostgreSQL
- **Cache**: Caffeine (via Spring Cache)
- **Build**: Maven or Gradle
- **Testing**: JUnit 5, MockMvc, Testcontainers

### Key Design Patterns
- **Repository Pattern**: `OwnerRepository`, `VetRepository`, `PetTypeRepository`
- **MVC Pattern**: Controllers handle HTTP, Services contain business logic
- **Entity Mapping**: JPA entities with relationships (`@OneToMany`, `@ManyToOne`)
- **Validation**: Bean Validation (`@NotEmpty`, custom validators)
- **Formatter**: Custom formatters for type conversion (`PetTypeFormatter`)

## Code Style & Conventions

### Java Code Style
- Use Spring Boot conventions and annotations
- Entities in `model/` package
- Controllers in feature packages (`owner/`, `vet/`)
- Repository interfaces extend `Repository<T, ID>`
- Service layer when business logic is complex

### Naming Conventions
- Controllers: `*Controller` (e.g., `OwnerController`)
- Repositories: `*Repository` (e.g., `OwnerRepository`)
- Entities: Plain nouns (e.g., `Owner`, `Pet`, `Visit`)
- DTOs: `*s` for collections (e.g., `Vets` wrapper class)

### REST Endpoints
- Follow RESTful conventions
- Use proper HTTP methods (GET, POST, PUT, DELETE)
- Return appropriate status codes
- Use `@GetMapping`, `@PostMapping`, etc.

### Testing
- Unit tests: `*Tests.java`
- Integration tests: `*IntegrationTests.java`
- Use `@WebMvcTest` for controller tests
- Use `@DataJpaTest` for repository tests
- Use Testcontainers for database integration tests

## Common Development Tasks

### Adding a New Entity
1. Create entity class in appropriate package (model/ or feature/)
2. Add JPA annotations (`@Entity`, `@Table`, `@Id`)
3. Create repository interface
4. Add controller with CRUD operations
5. Create Thymeleaf templates
6. Write unit and integration tests

### Adding a New Endpoint
1. Add method in appropriate controller
2. Use `@GetMapping` or `@PostMapping` with path
3. Follow existing patterns for model attributes
4. Return template name or redirect
5. Create/update corresponding template
6. Add controller tests

### Database Migration
1. Update schema in `src/main/resources/db/{database}/schema.sql`
2. Update data in `data.sql`
3. Test with each supported database profile

## Troubleshooting

### Java Version Issues
```bash
# Check current Java version
java -version

# Install Java 25 with SDKMAN
sdk install java 25.0.1-open
sdk use java 25.0.1-open
```

### Port Already in Use
```bash
# Find and kill process on port 8080
lsof -ti:8080 | xargs kill -9
```

### Database Connection Issues
- Check Docker containers are running
- Verify connection properties in `application-{profile}.properties`
- Check database logs

## Additional Resources
- [Spring Boot Documentation](https://spring.io/projects/spring-boot)
- [Spring Data JPA](https://spring.io/projects/spring-data-jpa)
- [Thymeleaf Documentation](https://www.thymeleaf.org/)
