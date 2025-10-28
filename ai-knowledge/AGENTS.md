# Spring PetClinic - AGENTS.md

> **Last Updated**: October 28, 2025  
> **Version**: 4.0.0-SNAPSHOT  
> **Purpose**: AI assistant knowledge base for Spring PetClinic application

---

## 1. Setup Commands

### Prerequisites
- **Java**: JDK 25+ (for building) or JDK 17+ (for running)
- **Git**: Latest version
- **Maven**: Included via wrapper (`./mvnw`)
- **Gradle**: Included via wrapper (`./gradlew`) - alternative to Maven
- **Docker**: Optional, for running databases
- **IDE**: IntelliJ IDEA, Eclipse, STS, or VS Code

### Installation & Setup
```bash
# Clone the repository
git clone https://github.com/JE-Ramos/spring-petclinic
cd spring-petclinic

# Build with Maven
./mvnw package

# Or build with Gradle
./gradlew build
```

### Running the Application

#### Option 1: Run JAR directly
```bash
# Maven
./mvnw package
java -jar target/*.jar

# Gradle
./gradlew build
java -jar build/libs/*.jar
```

#### Option 2: Run via Maven/Gradle plugin (with hot reload)
```bash
# Maven (recommended for development)
./mvnw spring-boot:run

# Gradle
./gradlew bootRun
```

#### Option 3: Run from IDE
- **IntelliJ IDEA**: Right-click `PetClinicApplication` → Run
- **Eclipse/STS**: Right-click project → Run As → Java Application

**Access the application**: http://localhost:8080/

### Database Setup

#### Default: H2 (in-memory)
```bash
# No setup needed - works out of the box
./mvnw spring-boot:run

# Access H2 Console: http://localhost:8080/h2-console
# JDBC URL: jdbc:h2:mem:<uuid> (check console startup logs for UUID)
```

#### MySQL
```bash
# Start MySQL with Docker
docker run -e MYSQL_USER=petclinic -e MYSQL_PASSWORD=petclinic \
  -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=petclinic \
  -p 3306:3306 mysql:9.2

# Or use docker-compose
docker compose up mysql

# Run application with MySQL profile
./mvnw spring-boot:run -Dspring-boot.run.profiles=mysql

# Or set environment variable
export SPRING_PROFILES_ACTIVE=mysql
./mvnw spring-boot:run
```

#### PostgreSQL
```bash
# Start PostgreSQL with Docker
docker run -e POSTGRES_USER=petclinic -e POSTGRES_PASSWORD=petclinic \
  -e POSTGRES_DB=petclinic -p 5432:5432 postgres:18.0

# Or use docker-compose
docker compose up postgres

# Run application with PostgreSQL profile
./mvnw spring-boot:run -Dspring-boot.run.profiles=postgres
```

### Testing
```bash
# Run all tests (Maven)
./mvnw test

# Run all tests (Gradle)
./gradlew test

# Run specific test class
./mvnw test -Dtest=OwnerControllerTests

# Run tests with coverage (generates report in target/site/jacoco)
./mvnw verify
```

### Code Quality & Formatting
```bash
# Check code formatting (Maven)
./mvnw spring-javaformat:validate

# Apply code formatting (Maven)
./mvnw spring-javaformat:apply

# Check code style (Gradle)
./gradlew checkFormatMain

# Apply code formatting (Gradle)
./gradlew formatMain

# Run Checkstyle
./mvnw checkstyle:check
./gradlew checkstyle
```

### CSS Compilation (SCSS → CSS)
```bash
# Compile SCSS to CSS (only with Maven)
./mvnw package -P css

# Location: src/main/scss/ → src/main/resources/static/resources/css/
```

### Building Container Image
```bash
# Build Docker image using Spring Boot buildpack (no Dockerfile needed)
./mvnw spring-boot:build-image

# Run the container
docker run -p 8080:8080 spring-petclinic:4.0.0-SNAPSHOT
```

### Kubernetes Deployment
```bash
# Apply database deployment
kubectl apply -f k8s/db.yml

# Apply application deployment
kubectl apply -f k8s/petclinic.yml
```

---

## 2. Project Structure

```
spring-petclinic/
├── src/
│   ├── main/
│   │   ├── java/org/springframework/samples/petclinic/
│   │   │   ├── model/              # Base entity classes
│   │   │   │   ├── BaseEntity.java
│   │   │   │   ├── NamedEntity.java
│   │   │   │   └── Person.java
│   │   │   ├── owner/              # Owner domain (Controller + Repository)
│   │   │   │   ├── Owner.java
│   │   │   │   ├── OwnerController.java
│   │   │   │   ├── OwnerRepository.java
│   │   │   │   ├── Pet.java
│   │   │   │   ├── PetController.java
│   │   │   │   ├── PetType.java
│   │   │   │   ├── PetTypeRepository.java
│   │   │   │   ├── Visit.java
│   │   │   │   └── VisitController.java
│   │   │   ├── vet/                # Vet domain
│   │   │   │   ├── Vet.java
│   │   │   │   ├── VetController.java
│   │   │   │   ├── VetRepository.java
│   │   │   │   └── Specialty.java
│   │   │   ├── system/             # System/infrastructure components
│   │   │   │   ├── CacheConfiguration.java
│   │   │   │   ├── WelcomeController.java
│   │   │   │   └── WebConfiguration.java
│   │   │   └── PetClinicApplication.java
│   │   ├── resources/
│   │   │   ├── application.properties        # Main config
│   │   │   ├── application-mysql.properties  # MySQL profile
│   │   │   ├── application-postgres.properties
│   │   │   ├── db/                          # Database scripts
│   │   │   │   ├── h2/
│   │   │   │   ├── mysql/
│   │   │   │   ├── postgres/
│   │   │   │   └── hsqldb/
│   │   │   ├── messages/           # i18n message bundles
│   │   │   ├── static/             # Static resources (CSS, images, fonts)
│   │   │   │   └── resources/
│   │   │   │       ├── css/
│   │   │   │       ├── fonts/
│   │   │   │       └── images/
│   │   │   └── templates/          # Thymeleaf templates
│   │   │       ├── error.html
│   │   │       ├── welcome.html
│   │   │       ├── fragments/      # Reusable template fragments
│   │   │       ├── owners/
│   │   │       ├── pets/
│   │   │       └── vets/
│   │   └── scss/                   # SCSS source files
│   │       └── petclinic.scss
│   └── test/
│       ├── java/org/springframework/samples/petclinic/
│       │   ├── owner/              # Controller tests
│       │   ├── vet/
│       │   ├── system/
│       │   ├── MySqlIntegrationTests.java
│       │   ├── PostgresIntegrationTests.java
│       │   └── PetClinicIntegrationTests.java
│       └── jmeter/                 # Performance test plans
├── pom.xml                         # Maven build configuration
├── build.gradle                    # Gradle build configuration
├── docker-compose.yml              # Docker services for databases
├── k8s/                           # Kubernetes manifests
│   ├── db.yml
│   └── petclinic.yml
└── ai-knowledge/                  # AI assistant documentation
    └── AGENTS.md
```

---

## 3. Architecture & Patterns

### Technology Stack
- **Framework**: Spring Boot 4.0.0-M3
- **Java**: 25 (build), 17+ (runtime)
- **Persistence**: Spring Data JPA with Hibernate
- **Databases**: H2 (default), MySQL 9.2, PostgreSQL 18.0
- **Template Engine**: Thymeleaf
- **Web**: Spring MVC, Spring Web
- **Validation**: Jakarta Bean Validation (Hibernate Validator)
- **Caching**: Caffeine Cache
- **CSS Framework**: Bootstrap 5.3.8
- **Icons**: Font Awesome 4.7.0
- **Build Tools**: Maven 3.9+ / Gradle 8.14+
- **Testing**: JUnit 5, Spring Test, MockMvc, Testcontainers
- **Code Quality**: Checkstyle, Spring Java Format, ErrorProne, NullAway

### Design Patterns & Architecture

#### 1. **Layered Architecture (Simplified)**
```
Controller → Repository → Database
     ↓
   Entity
```

**Note**: This application uses a **simplified 2-layer architecture** without an explicit Service layer. Controllers interact directly with Repositories. This is appropriate for simple CRUD applications.

#### 2. **Domain-Driven Design (Lite)**
- **Aggregates**: Owner (root) → Pet → Visit
- **Package-by-Feature**: Code organized by domain (`owner/`, `vet/`, `system/`)
- **Domain entities** contain business logic when appropriate

#### 3. **Repository Pattern**
- All data access via Spring Data JPA repositories
- Interfaces extend `JpaRepository<Entity, ID>`
- Query methods follow Spring Data naming conventions
- Example: `findByLastNameStartingWith(String lastName, Pageable pageable)`

#### 4. **MVC Pattern**
- **Model**: JPA entities in `model/`, `owner/`, `vet/` packages
- **View**: Thymeleaf templates in `src/main/resources/templates/`
- **Controller**: Spring MVC `@Controller` classes

#### 5. **Dependency Injection**
- Constructor-based injection (preferred)
- Example:
  ```java
  private final OwnerRepository owners;
  
  public OwnerController(OwnerRepository owners) {
      this.owners = owners;
  }
  ```

### Data Flow

#### Read Flow (Owner Details)
```
User Request → OwnerController.showOwner()
           → OwnerRepository.findById()
           → JPA/Hibernate → Database
           → Entity → ModelAndView
           → Thymeleaf Template → HTML Response
```

#### Write Flow (Create Owner)
```
User Form Submission → OwnerController.processCreationForm()
                    → @Valid validation
                    → OwnerRepository.save()
                    → JPA/Hibernate → Database
                    → Redirect to owner details
```

### Entity Relationships
```
Owner (1) ──< (*) Pet (1) ──< (*) Visit
Vet (*) ──< (*) Specialty
PetType (1) ──< (*) Pet
```

### Configuration Strategy
- **Profiles**: `default` (H2), `mysql`, `postgres`
- **Profile-specific config**: `application-{profile}.properties`
- **Database initialization**: SQL scripts in `src/main/resources/db/{database}/`

### Caching
- **Framework**: Caffeine Cache
- **Config**: `CacheConfiguration.java`
- **Cached data**: Vet list
- **Annotations**: `@Cacheable`, `@CacheEvict`

---

## 4. Code Style & Conventions

### Java Conventions
- **Style Guide**: Spring Java Format (based on Google Java Style)
- **Formatting**: Enforced via `spring-javaformat-maven-plugin`
- **Line Length**: 120 characters
- **Indentation**: Tabs for indentation, spaces for alignment
- **Encoding**: UTF-8

### File & Package Naming
- **Packages**: lowercase, no underscores (`owner`, `vet`, `system`)
- **Classes**: PascalCase (`OwnerController`, `PetRepository`)
- **Interfaces**: No "I" prefix (use `OwnerRepository`, not `IOwnerRepository`)
- **Tests**: `*Tests.java` (e.g., `OwnerControllerTests.java`)

### Class Naming Conventions
- **Controllers**: `*Controller` (e.g., `OwnerController`)
- **Repositories**: `*Repository` (e.g., `OwnerRepository`)
- **Entities**: Domain name (e.g., `Owner`, `Pet`, `Vet`)
- **Validators**: `*Validator` (e.g., `PetValidator`)
- **Formatters**: `*Formatter` (e.g., `PetTypeFormatter`)

### Method Naming
- **CRUD operations**: `save()`, `findById()`, `findAll()`, `delete()`
- **Query methods**: Follow Spring Data conventions (`findByLastNameStartingWith`)
- **Controller handlers**: HTTP verb + action (`initCreationForm`, `processCreationForm`)
- **Test methods**: `shouldDoSomethingWhenCondition()` or descriptive names

### Code Organization
- **One public class per file**
- **Package-private classes** when only used within package (common for Controllers)
- **Order within class**:
  1. Static constants
  2. Fields
  3. Constructor
  4. Public methods
  5. Private methods

### Imports
- **No wildcard imports**: Use explicit imports
- **Order**: 
  1. Java standard library
  2. Third-party libraries (Spring, Jakarta)
  3. Application packages
- **Unused imports**: Remove them

### Documentation Standards
- **JavaDoc**: Required for public APIs and repositories
- **Format**:
  ```java
  /**
   * Brief description.
   * <p>
   * Detailed description if needed.
   * </p>
   * @param paramName Description
   * @return Description
   * @throws ExceptionType When this happens
   */
  ```
- **License header**: Apache 2.0 license header on all `.java` files

### Annotations Style
- **One annotation per line** for class/method annotations
- **Constructor injection**: No `@Autowired` needed (implicit)
- **Null safety**: Use JSpecify `@Nullable` for nullable parameters/returns

### Example: Well-Formatted Controller Method
```java
@PostMapping("/owners/new")
public String processCreationForm(@Valid Owner owner, BindingResult result, 
                                   RedirectAttributes redirectAttributes) {
    if (result.hasErrors()) {
        redirectAttributes.addFlashAttribute("error", "There was an error in creating the owner.");
        return VIEWS_OWNER_CREATE_OR_UPDATE_FORM;
    }
    
    this.owners.save(owner);
    redirectAttributes.addFlashAttribute("message", "New Owner Created");
    return "redirect:/owners/" + owner.getId();
}
```

---

## 5. Development Workflow

### Git Workflow
- **Branches**: Feature branches from `main`
- **Branch naming**: `feature/description`, `bugfix/issue-number`
- **Commits**: Descriptive messages, include issue number if applicable
- **Example**: `Add validation for pet age (fixes #123)`

### Pull Request Process
1. Create feature branch
2. Make changes, write tests
3. Run tests and formatting: `./mvnw verify`
4. Commit with **signed-off-by** trailer: 
   ```bash
   git commit -s -m "Your commit message"
   ```
5. Push and create PR
6. Address review comments
7. Merge to main after approval

### Adding New Features

#### Add a New Entity/Domain
1. Create package: `src/main/java/org/springframework/samples/petclinic/{domain}/`
2. Create entity class (extends `BaseEntity` or `NamedEntity`)
3. Add JPA annotations (`@Entity`, `@Table`, `@Column`)
4. Create repository interface (extends `JpaRepository`)
5. Create controller class (`@Controller`)
6. Add validation annotations (`@NotEmpty`, `@Size`, etc.)
7. Create Thymeleaf templates in `templates/{domain}/`
8. Add database schema in `src/main/resources/db/{database}/schema.sql`
9. Add test data in `src/main/resources/db/{database}/data.sql`
10. Write tests

#### Add a New Endpoint/Controller Method
1. Add method to appropriate Controller
2. Use `@GetMapping` or `@PostMapping`
3. Follow RESTful URL patterns: `/resource/{id}`, `/resource/new`, `/resource/{id}/edit`
4. Return view name (String) or `ModelAndView`
5. Add validation with `@Valid` and `BindingResult`
6. Use `RedirectAttributes` for flash messages
7. Create or update Thymeleaf template
8. Write controller test with MockMvc

#### Add a New Thymeleaf Template
1. Create HTML file in `src/main/resources/templates/{domain}/`
2. Use layout fragment: `th:replace="~{fragments/layout :: layout (...)}`
3. Use form fragments for consistency: `th:replace="~{fragments/inputField}"`
4. Follow Bootstrap 5 structure
5. Use Thymeleaf expressions: `${owner.firstName}`, `th:href`, `th:if`

### Database Migrations
- **No automated migrations**: Use SQL scripts
- **Schema changes**:
  1. Update `src/main/resources/db/{database}/schema.sql` for ALL databases
  2. Update data scripts if needed
  3. Test with each database profile
- **DDL mode**: `spring.jpa.hibernate.ddl-auto=none` (Hibernate does NOT manage schema)

### Environment Management
- **Development**: Use H2 (default) or Docker databases
- **Testing**: H2 in-memory or Testcontainers
- **Production**: External MySQL or PostgreSQL

### Configuration
- **Profiles**: Activate via `SPRING_PROFILES_ACTIVE` environment variable
- **Local overrides**: Create `application-local.properties` (git-ignored)
- **Sensitive data**: Never commit passwords; use environment variables

---

## 6. Testing Guidelines

### Testing Stack
- **Framework**: JUnit 5 (Jupiter)
- **Spring Support**: `@SpringBootTest`, `@WebMvcTest`, `@DataJpaTest`
- **Mocking**: Mockito (included in Spring Boot Test)
- **Web Testing**: MockMvc, RestClient
- **Database Testing**: Testcontainers, H2 in-memory
- **Coverage**: JaCoCo (reports in `target/site/jacoco/`)

### Test Types & Organization

#### 1. **Unit Tests** (Controller/Component Tests)
- **Location**: `src/test/java/org/springframework/samples/petclinic/{package}/`
- **Naming**: `{ClassName}Tests.java`
- **Annotation**: `@WebMvcTest` for controllers
- **Example**: `OwnerControllerTests.java`

```java
@WebMvcTest(OwnerController.class)
class OwnerControllerTests {
    @Autowired
    private MockMvc mockMvc;
    
    @MockBean
    private OwnerRepository owners;
    
    @Test
    void shouldShowOwner() throws Exception {
        // Test implementation
    }
}
```

#### 2. **Integration Tests**
- **Default database**: `PetClinicIntegrationTests.java` (H2, with DevTools)
- **MySQL**: `MySqlIntegrationTests.java` (Testcontainers)
- **PostgreSQL**: `PostgresIntegrationTests.java` (Docker Compose)
- **Annotation**: `@SpringBootTest`

```java
@SpringBootTest
class PetClinicIntegrationTests {
    @Test
    void contextLoads() {
        // Verify application starts successfully
    }
}
```

#### 3. **Test Applications** (for manual testing during development)
- **H2**: Run `PetClinicIntegrationTests.main()` in IDE
- **MySQL**: Run `MysqlTestApplication.main()` 
- **PostgreSQL**: Run `PostgresIntegrationTests.main()`

### Test File Naming
- **Class under test**: `OwnerController.java`
- **Test class**: `OwnerControllerTests.java` (note: **Tests**, not *Test*)

### What to Test
- **Controllers**: HTTP requests/responses, validation, error handling
- **Repositories**: Custom query methods (Spring Data methods don't need tests)
- **Validators**: Custom validation logic
- **Formatters**: String conversion logic
- **Integration**: Application startup, database connectivity

### Running Tests
```bash
# All tests
./mvnw test

# Specific test class
./mvnw test -Dtest=OwnerControllerTests

# Specific test method
./mvnw test -Dtest=OwnerControllerTests#shouldShowOwner

# Integration tests only
./mvnw test -Dtest=*IntegrationTests

# With coverage
./mvnw verify
# Report: target/site/jacoco/index.html
```

### Coverage Expectations
- **Goal**: High coverage for business logic
- **Tool**: JaCoCo plugin
- **Report**: Generated during `./mvnw verify` phase
- **Location**: `target/site/jacoco/index.html`

### Test Data
- **Production data**: `src/main/resources/db/{database}/data.sql`
- **Test data**: Loaded from production data scripts or created in tests
- **Test builders**: Use `EntityUtils` helper for creating test entities

---

## 7. Common Development Tasks

### Task 1: Add a New Entity (e.g., Appointment)

```java
// Step 1: Create entity class
// File: src/main/java/org/springframework/samples/petclinic/appointment/Appointment.java
@Entity
@Table(name = "appointments")
public class Appointment extends BaseEntity {
    
    @NotNull
    @Column(name = "appointment_date")
    private LocalDate date;
    
    @ManyToOne
    @JoinColumn(name = "pet_id")
    private Pet pet;
    
    // Getters and setters
}

// Step 2: Create repository
// File: src/main/java/org/springframework/samples/petclinic/appointment/AppointmentRepository.java
public interface AppointmentRepository extends JpaRepository<Appointment, Integer> {
    List<Appointment> findByPetId(Integer petId);
}

// Step 3: Add database schema
// File: src/main/resources/db/h2/schema.sql (and mysql, postgres)
CREATE TABLE IF NOT EXISTS appointments (
    id INT GENERATED BY DEFAULT AS IDENTITY PRIMARY KEY,
    pet_id INT NOT NULL,
    appointment_date DATE NOT NULL,
    FOREIGN KEY (pet_id) REFERENCES pets(id)
);

// Step 4: Create controller
@Controller
class AppointmentController {
    private final AppointmentRepository appointments;
    
    public AppointmentController(AppointmentRepository appointments) {
        this.appointments = appointments;
    }
    
    // Add handler methods
}

// Step 5: Write tests
@WebMvcTest(AppointmentController.class)
class AppointmentControllerTests {
    // Add test methods
}
```

### Task 2: Add a New REST Endpoint

```java
// File: src/main/java/org/springframework/samples/petclinic/owner/OwnerController.java

@GetMapping("/owners/{ownerId}/pets")
@ResponseBody  // For REST response
public List<Pet> getOwnerPets(@PathVariable("ownerId") int ownerId) {
    Owner owner = this.owners.findById(ownerId)
        .orElseThrow(() -> new ResponseStatusException(HttpStatus.NOT_FOUND));
    return owner.getPets();
}
```

### Task 3: Add a New Thymeleaf Page

```html
<!-- File: src/main/resources/templates/appointments/appointmentList.html -->
<!DOCTYPE html>
<html xmlns:th="https://www.thymeleaf.org"
      th:replace="~{fragments/layout :: layout (~{::body},'appointments')}">
<body>
    <h2>Appointments</h2>
    <table class="table table-striped">
        <thead>
            <tr>
                <th>Date</th>
                <th>Pet</th>
                <th>Owner</th>
            </tr>
        </thead>
        <tbody>
            <tr th:each="appointment : ${appointments}">
                <td th:text="${#temporals.format(appointment.date, 'yyyy-MM-dd')}"></td>
                <td th:text="${appointment.pet.name}"></td>
                <td th:text="${appointment.pet.owner.firstName + ' ' + appointment.pet.owner.lastName}"></td>
            </tr>
        </tbody>
    </table>
</body>
</html>
```

### Task 4: Add Validation to an Entity

```java
// Add to entity class
@NotEmpty
@Size(min = 1, max = 30)
private String name;

@NotNull
@Min(0)
private Integer age;

@Pattern(regexp = "^[0-9]{10}$", message = "Phone number must be 10 digits")
private String phoneNumber;

// In controller, use @Valid
@PostMapping("/owners/new")
public String processForm(@Valid Owner owner, BindingResult result) {
    if (result.hasErrors()) {
        return "owners/createOrUpdateOwnerForm";
    }
    // Save owner
}
```

### Task 5: Add a New Dependency

```xml
<!-- Maven: Add to pom.xml -->
<dependency>
    <groupId>com.example</groupId>
    <artifactId>library-name</artifactId>
    <version>1.2.3</version>
</dependency>
```

```groovy
// Gradle: Add to build.gradle
dependencies {
    implementation 'com.example:library-name:1.2.3'
}
```

Then rebuild:
```bash
./mvnw clean install  # Maven
./gradlew clean build  # Gradle
```

### Task 6: Update Bootstrap or Web Dependencies

```xml
<!-- Update version in pom.xml -->
<properties>
    <webjars-bootstrap.version>5.3.8</webjars-bootstrap.version>
</properties>
```

```bash
# Rebuild and regenerate CSS
./mvnw clean package -P css
```

### Task 7: Add Caching to a Repository Method

```java
// Step 1: Add cache name to CacheConfiguration.java
@Bean
public CacheManagerCustomizer<CaffeineCacheManager> cacheManagerCustomizer() {
    return cacheManager -> 
        cacheManager.setCacheNames(List.of("vets", "owners")); // Add "owners"
}

// Step 2: Add @Cacheable to repository or controller
@Cacheable("owners")
public Collection<Owner> findAll() {
    return ownerRepository.findAll();
}

// Step 3: Add cache eviction on updates
@CacheEvict(value = "owners", allEntries = true)
public void save(Owner owner) {
    ownerRepository.save(owner);
}
```

---

## 8. Troubleshooting

### Issue: Application Won't Start

#### Symptom: Port 8080 already in use
```
***************************
APPLICATION FAILED TO START
***************************
Web server failed to start. Port 8080 was already in use.
```

**Solution**:
```bash
# Find process using port 8080
lsof -i :8080  # macOS/Linux
netstat -ano | findstr :8080  # Windows

# Kill the process or change port
./mvnw spring-boot:run -Dspring-boot.run.arguments=--server.port=8081
```

#### Symptom: Java version mismatch
```
[ERROR] Failed to execute goal: requires Java 25
```

**Solution**:
```bash
# Check Java version
java -version
javac -version

# Set JAVA_HOME to JDK 25
export JAVA_HOME=/path/to/jdk-25  # macOS/Linux
set JAVA_HOME=C:\path\to\jdk-25   # Windows
```

### Issue: Database Connection Failures

#### MySQL Connection Error
```
com.mysql.cj.jdbc.exceptions.CommunicationsException: Communications link failure
```

**Solution**:
```bash
# Verify MySQL is running
docker ps  # Check if MySQL container is running

# Restart MySQL
docker compose up mysql

# Verify connection details in application-mysql.properties
spring.datasource.url=jdbc:mysql://localhost:3306/petclinic
spring.datasource.username=petclinic
spring.datasource.password=petclinic
```

#### PostgreSQL Connection Error
**Solution**: Similar to MySQL, verify PostgreSQL is running and connection details

### Issue: Build Failures

#### Maven Build Failed: Formatting Issues
```
[ERROR] Found 5 non-compliant files
```

**Solution**:
```bash
# Auto-fix formatting
./mvnw spring-javaformat:apply

# Then rebuild
./mvnw clean package
```

#### Maven Build Failed: Checkstyle Violations
**Solution**:
```bash
# View detailed errors
./mvnw checkstyle:check

# Common fixes:
# - Remove unused imports
# - Fix indentation (use tabs)
# - Ensure license headers are present
```

#### Gradle Build Failed
```
./gradlew build  # View error details
./gradlew build --stacktrace  # Full stack trace
```

### Issue: Tests Failing

#### Testcontainers: Docker Not Running
```
Could not find a valid Docker environment
```

**Solution**:
```bash
# Start Docker Desktop or Docker daemon
# Verify with:
docker ps

# Skip MySQL tests if needed:
./mvnw test -Dtest='!MySqlIntegrationTests'
```

#### H2 Database Lock
```
Caused by: org.h2.jdbc.JdbcSQLException: Database may be already in use
```

**Solution**:
```bash
# Stop other running instances
# Delete H2 database files in temp directory
# Restart tests
```

### Issue: CSS Not Updating

**Problem**: SCSS changes not reflected

**Solution**:
```bash
# Recompile CSS with Maven css profile
./mvnw clean package -P css

# CSS source: src/main/scss/petclinic.scss
# CSS output: src/main/resources/static/resources/css/petclinic.css
```

### Issue: Template Changes Not Reflected

**Problem**: Thymeleaf template changes not showing

**Solution**:
```bash
# Use Spring Boot DevTools (included in test scope)
# Run via IDE or:
./mvnw spring-boot:run

# Templates auto-reload without restart
# Clear browser cache: Ctrl+Shift+R (hard refresh)
```

### Issue: Environment Setup Problems

#### IDE Not Recognizing Project
**IntelliJ IDEA**:
1. File → Invalidate Caches → Invalidate and Restart
2. Re-import Maven/Gradle project
3. Ensure Java SDK 25 is configured

**Eclipse/STS**:
1. Right-click project → Maven → Update Project
2. Enable "Force Update of Snapshots/Releases"

#### Missing Generated Sources
```bash
# Generate sources (Maven)
./mvnw generate-resources

# Generate sources (Gradle)
./gradlew processResources
```

### Debugging Tips

#### Enable Debug Logging
```properties
# Add to application.properties or application-local.properties
logging.level.org.springframework.web=DEBUG
logging.level.org.springframework.data=DEBUG
logging.level.org.hibernate.SQL=DEBUG
logging.level.org.hibernate.type.descriptor.sql.BasicBinder=TRACE
```

#### Debug in IDE
- **IntelliJ**: Set breakpoints, click Debug button
- **Eclipse**: Set breakpoints, Run As → Debug As → Java Application

#### Access Actuator Endpoints
```bash
# Health check
curl http://localhost:8080/actuator/health

# Application info
curl http://localhost:8080/actuator/info

# All actuator endpoints
curl http://localhost:8080/actuator
```

---

## 9. API/Endpoints Documentation

### Web Application Endpoints (HTML)

| URL Pattern | Method | Description | View Template |
|------------|--------|-------------|---------------|
| `/` | GET | Welcome page | `welcome.html` |
| `/owners/find` | GET | Find owners form | `owners/findOwners.html` |
| `/owners` | GET | Search/list owners (with pagination) | `owners/ownersList.html` |
| `/owners/new` | GET | New owner form | `owners/createOrUpdateOwnerForm.html` |
| `/owners/new` | POST | Create owner | Redirect to owner details |
| `/owners/{ownerId}` | GET | Owner details (with pets) | `owners/ownerDetails.html` |
| `/owners/{ownerId}/edit` | GET | Edit owner form | `owners/createOrUpdateOwnerForm.html` |
| `/owners/{ownerId}/edit` | POST | Update owner | Redirect to owner details |
| `/owners/{ownerId}/pets/new` | GET | Add pet form | `pets/createOrUpdatePetForm.html` |
| `/owners/{ownerId}/pets/new` | POST | Create pet | Redirect to owner details |
| `/owners/{ownerId}/pets/{petId}/edit` | GET | Edit pet form | `pets/createOrUpdatePetForm.html` |
| `/owners/{ownerId}/pets/{petId}/edit` | POST | Update pet | Redirect to owner details |
| `/owners/{ownerId}/pets/{petId}/visits/new` | GET | Add visit form | `pets/createOrUpdateVisitForm.html` |
| `/owners/{ownerId}/pets/{petId}/visits/new` | POST | Create visit | Redirect to owner details |
| `/vets.html` | GET | List all vets (HTML) | `vets/vetList.html` |

### Static Resources
- CSS: `/resources/css/petclinic.css`
- Images: `/resources/images/`
- Fonts: `/resources/fonts/`

### Actuator Endpoints (Management)
- Health: `/actuator/health`
- Info: `/actuator/info`
- Metrics: `/actuator/metrics`
- All endpoints: `/actuator`

### Common Request/Response Patterns

#### Form Submission with Validation
```java
@PostMapping("/owners/new")
public String processCreationForm(@Valid Owner owner, BindingResult result, 
                                   RedirectAttributes redirectAttributes) {
    if (result.hasErrors()) {
        // Validation failed - show form again with errors
        redirectAttributes.addFlashAttribute("error", "...");
        return "owners/createOrUpdateOwnerForm";
    }
    
    // Save and redirect with success message
    this.owners.save(owner);
    redirectAttributes.addFlashAttribute("message", "New Owner Created");
    return "redirect:/owners/" + owner.getId();
}
```

#### Pagination
```java
// Request: /owners?page=2&lastName=Smith
@GetMapping("/owners")
public String processFindForm(@RequestParam(defaultValue = "1") int page, 
                               Owner owner, Model model) {
    Page<Owner> ownersResults = findPaginatedForOwnersLastName(page, owner.getLastName());
    model.addAttribute("currentPage", page);
    model.addAttribute("totalPages", ownersResults.getTotalPages());
    return "owners/ownersList";
}
```

---

## 10. Additional Resources

### Official Documentation
- **Spring Boot**: https://docs.spring.io/spring-boot/docs/current/reference/html/
- **Spring Data JPA**: https://docs.spring.io/spring-data/jpa/docs/current/reference/html/
- **Spring MVC**: https://docs.spring.io/spring-framework/docs/current/reference/html/web.html
- **Thymeleaf**: https://www.thymeleaf.org/documentation.html
- **Bootstrap 5**: https://getbootstrap.com/docs/5.3/

### Project-Specific Resources
- **GitHub Repository**: https://github.com/spring-projects/spring-petclinic
- **Issue Tracker**: https://github.com/spring-projects/spring-petclinic/issues
- **Presentation Slides**: https://speakerdeck.com/michaelisvy/spring-petclinic-sample-application
- **Spring Petclinic Forks**: https://spring-petclinic.github.io/docs/forks.html

### Database Documentation
- **H2 Database**: http://www.h2database.com/html/main.html
- **MySQL Setup**: `src/main/resources/db/mysql/petclinic_db_setup_mysql.txt`
- **PostgreSQL Setup**: `src/main/resources/db/postgres/petclinic_db_setup_postgres.txt`

### Testing Resources
- **JUnit 5**: https://junit.org/junit5/docs/current/user-guide/
- **Testcontainers**: https://www.testcontainers.org/
- **Spring Boot Testing**: https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.testing

### Tools & Plugins
- **Spring Boot DevTools**: Auto-restart on code changes
- **Spring Boot Actuator**: Production-ready monitoring
- **JaCoCo**: Code coverage reports
- **Checkstyle**: Code style enforcement
- **ErrorProne**: Static analysis for bug detection

### Code Quality
- **Spring Java Format**: https://github.com/spring-io/spring-javaformat
- **Checkstyle Config**: `src/checkstyle/nohttp-checkstyle.xml`
- **EditorConfig**: `.editorconfig` (for IDE consistency)

### Community
- **Stack Overflow**: Tag `spring-petclinic`
- **Spring Community**: https://spring.io/community
- **Discussions**: GitHub Discussions tab

---

## Quick Reference

### Essential Commands Cheat Sheet
```bash
# Build and run
./mvnw spring-boot:run

# Run with MySQL
./mvnw spring-boot:run -Dspring-boot.run.profiles=mysql

# Run tests
./mvnw test

# Format code
./mvnw spring-javaformat:apply

# Full build with tests and formatting
./mvnw clean verify

# Build Docker image
./mvnw spring-boot:build-image

# Compile CSS
./mvnw package -P css
```

### Key Files to Remember
- Main app: `PetClinicApplication.java`
- Config: `application.properties`, `application-{profile}.properties`
- Database scripts: `src/main/resources/db/{database}/`
- Templates: `src/main/resources/templates/`
- Tests: `src/test/java/`

### Architecture Summary
```
Package by Feature → Controller → Repository → Database
                          ↓
                       Thymeleaf View
```

### Getting Help
1. Check this `AGENTS.md`
2. Search GitHub Issues
3. Check official Spring Boot documentation
4. Review test examples in `src/test/`

---

**End of AGENTS.md**  
*This document should be updated whenever significant architectural or workflow changes are made.*

