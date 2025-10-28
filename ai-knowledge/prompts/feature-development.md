# Prompt: Feature Development Template

## Purpose
This prompt template helps you implement new features in the Spring PetClinic application (or any project) by following established patterns, conventions, and best practices. It ensures AI assistants have the full context needed to generate high-quality, consistent code.

---

## The Prompt

```
I need to implement a new feature: [FEATURE_NAME]

**Feature Description:**
[Describe what the feature does, user story format is ideal:
"As a [user type], I want to [action] so that [benefit]"]

**Technical Requirements:**
- [List specific technical requirements]
- [Database changes needed]
- [UI/UX requirements]
- [Security/validation requirements]

Please implement this feature following the project's established patterns documented in @AGENTS.md

**Implementation Checklist:**

## 1. Domain Model & Database
- [ ] Create/modify entity classes with proper JPA annotations
- [ ] Add validation annotations (@NotEmpty, @NotNull, @Size, etc.)
- [ ] Define entity relationships (OneToMany, ManyToOne, etc.)
- [ ] Update database schema in ALL database folders:
  - src/main/resources/db/h2/schema.sql
  - src/main/resources/db/mysql/schema.sql
  - src/main/resources/db/postgres/schema.sql
- [ ] Add sample data to data.sql files for each database
- [ ] Follow naming conventions from @AGENTS.md

## 2. Repository Layer
- [ ] Create repository interface extending JpaRepository
- [ ] Add custom query methods following Spring Data naming conventions
- [ ] Add JavaDoc documentation
- [ ] Follow package-by-feature structure

## 3. Controller Layer
- [ ] Create or update controller class with @Controller annotation
- [ ] Implement GET endpoints for displaying forms/data
- [ ] Implement POST endpoints for form submissions
- [ ] Add proper validation with @Valid and BindingResult
- [ ] Use RedirectAttributes for flash messages
- [ ] Follow RESTful URL patterns
- [ ] Add proper error handling
- [ ] Use constructor-based dependency injection

## 4. View Layer (Thymeleaf)
- [ ] Create templates in src/main/resources/templates/[domain]/
- [ ] Use layout fragments: th:replace="~{fragments/layout :: layout}"
- [ ] Use form input fragments for consistency
- [ ] Add proper form validation display
- [ ] Follow Bootstrap 5 styling conventions
- [ ] Ensure mobile responsiveness
- [ ] Add proper Thymeleaf expressions and attributes

## 5. Testing
- [ ] Create controller tests with @WebMvcTest
- [ ] Test all endpoints (GET and POST)
- [ ] Test validation scenarios
- [ ] Test error handling
- [ ] Follow naming convention: [ClassName]Tests.java
- [ ] Ensure tests pass: ./mvnw test

## 6. Code Quality
- [ ] Apply code formatting: ./mvnw spring-javaformat:apply
- [ ] Run checkstyle: ./mvnw checkstyle:check
- [ ] Add proper JavaDoc comments
- [ ] Follow Spring Java Format conventions
- [ ] No compiler warnings or errors
- [ ] Add license headers to new files

## 7. Integration & Verification
- [ ] Run full build: ./mvnw clean verify
- [ ] Test manually in browser
- [ ] Test with H2, MySQL, and PostgreSQL profiles
- [ ] Verify data persistence
- [ ] Check for proper error messages and validation

**Additional Context:**
@AGENTS.md - Project conventions and patterns
@[relevant entity files]
@[relevant controller files]
@[relevant template files]

Please implement this feature step-by-step, explaining your decisions and ensuring all checklist items are completed.
```

---

## How to Use This Prompt

### Step 1: Define Your Feature
Before using the prompt, clearly define:
- **What**: What functionality are you adding?
- **Why**: What problem does it solve?
- **Who**: Which users will use it?
- **How**: What's the user interaction flow?

### Step 2: Gather Context
Attach relevant files in Cursor with @ mentions:
- `@AGENTS.md` - Always include for project conventions
- `@[similar-entity].java` - Reference similar existing features
- `@[similar-controller].java` - Reference similar controller patterns
- `@[similar-template].html` - Reference similar UI patterns

### Step 3: Customize the Prompt
Fill in the placeholders:
- `[FEATURE_NAME]`: e.g., "Delete Owner", "Bulk Import Pets", "Appointment Scheduling"
- `[Feature Description]`: User story or detailed description
- `[Technical Requirements]`: Specific requirements for this feature

### Step 4: Run in Cursor Chat
1. Open Cursor Chat (Cmd+L or Ctrl+L)
2. Paste the customized prompt
3. Attach relevant files with @ mentions
4. Send and review the implementation plan
5. Ask follow-up questions to clarify

### Step 5: Implement & Iterate
- Review each generated file
- Test as you go
- Ask for modifications if needed
- Run tests and formatting tools
- Iterate until complete

---

## Example Use Cases

### Example 1: Delete Operations for Owners

```
I need to implement a new feature: Delete Owner

**Feature Description:**
As a clinic administrator, I want to delete owner records so that I can remove 
inactive or duplicate owners from the system.

**Technical Requirements:**
- Add "Delete" button on owner details page
- Require confirmation before deletion
- Cascade delete to associated pets and visits (or prevent if pets exist)
- Show success message after deletion
- Redirect to owners list after successful deletion
- Add proper error handling if deletion fails

Please implement this feature following the project's established patterns documented in @AGENTS.md

[... rest of checklist ...]

**Additional Context:**
@AGENTS.md
@Owner.java
@OwnerController.java
@ownerDetails.html
```

### Example 2: New Feature - Appointment Scheduling

```
I need to implement a new feature: Appointment Scheduling

**Feature Description:**
As a clinic receptionist, I want to schedule appointments for pets so that we can 
manage the clinic's daily schedule efficiently.

**Technical Requirements:**
- Create new Appointment entity with date, time, pet, vet, and reason fields
- Appointments should be linked to existing pets and vets
- Add appointment list view showing upcoming appointments
- Add create appointment form with date/time picker
- Add edit and cancel appointment functionality
- Validate that appointment time is in the future
- Prevent double-booking of vets at the same time
- Show appointments on owner details page

Please implement this feature following the project's established patterns documented in @AGENTS.md

[... rest of checklist ...]

**Additional Context:**
@AGENTS.md
@Pet.java
@Visit.java
@VisitController.java
```

### Example 3: Enhancement - Search & Filter

```
I need to implement a new feature: Advanced Pet Search

**Feature Description:**
As a clinic staff member, I want to search for pets by name, type, or owner so that 
I can quickly find specific pet records.

**Technical Requirements:**
- Add search form with fields: pet name, pet type, owner name
- Support partial matching (case-insensitive)
- Add pagination to search results
- Show pet details with owner information in results
- Maintain search criteria after pagination
- Add "Clear filters" button

Please implement this feature following the project's established patterns documented in @AGENTS.md

[... rest of checklist ...]

**Additional Context:**
@AGENTS.md
@PetController.java
@OwnerController.java (reference pagination pattern)
@PetRepository.java
```

---

## Feature Type Templates

### CRUD Feature Template
For basic Create, Read, Update, Delete operations:

**Checklist Focus:**
- ✅ Entity with validation
- ✅ Repository with custom queries
- ✅ Controller with all CRUD endpoints
- ✅ Forms for create/edit
- ✅ List view with pagination
- ✅ Delete with confirmation
- ✅ Comprehensive tests

### Enhancement Feature Template
For improving existing features:

**Checklist Focus:**
- ✅ Identify existing code to modify
- ✅ Maintain backward compatibility
- ✅ Update existing tests
- ✅ Add new test cases
- ✅ Update related documentation

### Integration Feature Template
For connecting external services:

**Checklist Focus:**
- ✅ Add dependencies to pom.xml
- ✅ Create configuration properties
- ✅ Create service classes
- ✅ Add error handling for external failures
- ✅ Mock external services in tests
- ✅ Add integration tests

### UI Enhancement Template
For frontend-only changes:

**Checklist Focus:**
- ✅ Update Thymeleaf templates
- ✅ Update or add SCSS if needed
- ✅ Ensure mobile responsiveness
- ✅ Test across browsers
- ✅ Maintain accessibility standards

---

## Best Practices

### ✅ DO:
- Always reference `@AGENTS.md` for project conventions
- Start with the database schema and work up
- Write tests as you implement features
- Follow existing patterns in the codebase
- Run formatting and tests before considering complete
- Add proper validation and error handling
- Use meaningful commit messages
- Test with multiple database profiles

### ❌ DON'T:
- Skip writing tests
- Ignore code formatting guidelines
- Forget to update all database schema files
- Add features without validation
- Copy-paste code without understanding
- Create inconsistent naming patterns
- Forget error handling
- Skip manual testing

---

## Testing Your Feature

After implementation, test with these verification steps:

### 1. **Build & Format Test**
```bash
./mvnw spring-javaformat:apply
./mvnw checkstyle:check
./mvnw clean verify
```

### 2. **Manual Test Scenarios**
Create a checklist of user actions:
- [ ] Happy path: Feature works as expected
- [ ] Validation: Invalid input shows proper errors
- [ ] Edge cases: Empty data, duplicates, etc.
- [ ] Navigation: All links work correctly
- [ ] Mobile: Responsive design works

### 3. **Database Test**
Test with all supported databases:
```bash
# H2 (default)
./mvnw spring-boot:run

# MySQL
./mvnw spring-boot:run -Dspring-boot.run.profiles=mysql

# PostgreSQL
./mvnw spring-boot:run -Dspring-boot.run.profiles=postgres
```

### 4. **Integration Test**
- [ ] Application starts without errors
- [ ] All existing features still work
- [ ] New feature integrates seamlessly
- [ ] No console errors or warnings

---

## Common Feature Patterns

### Pattern 1: Adding Delete Functionality

**Steps:**
1. Add DELETE endpoint to controller
2. Add confirmation modal to view
3. Handle cascade deletes or foreign key constraints
4. Add success/error flash messages
5. Test deletion and data integrity
6. Add controller test for delete endpoint

**Example Files to Reference:**
- Existing delete patterns in the codebase
- Cascade delete configurations in entities

### Pattern 2: Adding Validation Rules

**Steps:**
1. Add validation annotations to entity
2. Create custom validator if needed (see `PetValidator`)
3. Add validation in controller with `@Valid` and `BindingResult`
4. Display errors in Thymeleaf template
5. Test validation in controller tests

**Example Files to Reference:**
- `@Pet.java` - Validation annotations
- `@PetValidator.java` - Custom validator
- `@PetController.java` - Validation handling

### Pattern 3: Adding Relationships

**Steps:**
1. Define relationship in entity with JPA annotations
2. Update database schema with foreign keys
3. Update forms to select related entities
4. Add repository methods to fetch with relationships
5. Update views to display related data
6. Test relationship integrity

**Example Files to Reference:**
- `@Owner.java` - OneToMany example
- `@Pet.java` - ManyToOne example
- `@Vet.java` - ManyToMany example

### Pattern 4: Adding Pagination

**Steps:**
1. Update repository method to use `Pageable` parameter
2. Update controller to handle page parameter
3. Use `Page<Entity>` instead of `List<Entity>`
4. Add pagination controls in Thymeleaf template
5. Test navigation between pages

**Example Files to Reference:**
- `@OwnerController.java` - Pagination implementation
- `@ownersList.html` - Pagination UI

---

## Quick Start: Delete Feature Example

Here's a complete example for adding delete functionality to Owners:

```
I need to implement a new feature: Delete Owner

**Feature Description:**
As a clinic administrator, I want to delete owner records so that I can remove 
inactive or test owners from the system. The system should prevent deletion if 
the owner has any pets.

**Technical Requirements:**
- Add DELETE endpoint: POST /owners/{ownerId}/delete
- Check if owner has pets before allowing deletion
- If pets exist, show error message and prevent deletion
- If no pets, delete owner and show success message
- Add "Delete Owner" button on owner details page
- Add confirmation dialog before deletion
- Redirect to owners list after successful deletion

Please implement this feature following the project's established patterns documented in @AGENTS.md

**Implementation Checklist:**

## 1. Domain Model & Database
- [x] No entity changes needed (using existing Owner entity)
- [x] No database schema changes needed

## 2. Repository Layer
- [x] No new repository methods needed (using existing OwnerRepository)

## 3. Controller Layer
- [ ] Add deleteOwner() method to OwnerController
- [ ] Check if owner has pets using owner.getPets().isEmpty()
- [ ] If pets exist, add error flash message and redirect back
- [ ] If no pets, call ownerRepository.delete(owner)
- [ ] Add success flash message
- [ ] Redirect to /owners/find

## 4. View Layer (Thymeleaf)
- [ ] Add delete button to ownerDetails.html
- [ ] Add Bootstrap confirmation modal
- [ ] Style button as danger (btn-danger)
- [ ] Position button appropriately on page

## 5. Testing
- [ ] Add testDeleteOwnerWithNoPets() - should succeed
- [ ] Add testDeleteOwnerWithPets() - should fail with error
- [ ] Add testDeleteNonExistentOwner() - should handle gracefully

## 6. Code Quality
- [ ] Run ./mvnw spring-javaformat:apply
- [ ] Run ./mvnw checkstyle:check
- [ ] Add JavaDoc comment for deleteOwner method
- [ ] No warnings or errors

## 7. Integration & Verification
- [ ] Test manually: delete owner without pets - should work
- [ ] Test manually: delete owner with pets - should show error
- [ ] Run all tests: ./mvnw verify

**Additional Context:**
@AGENTS.md
@OwnerController.java
@Owner.java
@ownerDetails.html
@OwnerControllerTests.java

Please implement this feature step-by-step, showing the code changes for each file.
```

---

## Troubleshooting Feature Development

### Issue: Tests Failing After Implementation

**Solution:**
1. Read the test error message carefully
2. Check that mock beans are set up correctly
3. Verify test data matches validation rules
4. Ensure proper MockMvc expectations
5. Run tests individually to isolate issues

### Issue: Validation Not Working

**Solution:**
1. Verify `@Valid` annotation on controller parameter
2. Check `BindingResult` is immediately after `@Valid` parameter
3. Ensure validation annotations on entity are correct
4. Check that error messages are displayed in template
5. Review `th:errors` attributes in Thymeleaf

### Issue: Database Changes Not Applied

**Solution:**
1. Verify schema.sql updated for ALL databases (h2, mysql, postgres)
2. Check `spring.jpa.hibernate.ddl-auto=none` in application.properties
3. Drop and recreate database if using Docker
4. Clear H2 in-memory database by restarting app
5. Check SQL syntax for specific database

### Issue: Template Not Rendering

**Solution:**
1. Check template path matches controller return value
2. Verify template uses correct layout fragment
3. Check for Thymeleaf syntax errors
4. Review browser console for errors
5. Enable debug logging for Thymeleaf

---

## Success Metrics

Your feature is successfully implemented if:
- ✅ All tests pass (`./mvnw verify` succeeds)
- ✅ Code follows formatting standards (no checkstyle errors)
- ✅ Feature works in browser as expected
- ✅ Works with all database profiles (H2, MySQL, PostgreSQL)
- ✅ Validation prevents invalid input
- ✅ Error handling covers edge cases
- ✅ UI is responsive and follows Bootstrap conventions
- ✅ Code follows patterns documented in AGENTS.md
- ✅ Documentation/JavaDoc is clear
- ✅ No compiler warnings or linter errors

---

## Pro Tips

1. **Start Small**: Implement minimal viable feature first, then enhance
2. **Reference Existing**: Find similar features in codebase as templates
3. **Test Early**: Write tests as you implement, not after
4. **Use AI Iteratively**: Break large features into smaller tasks
5. **Follow Patterns**: Consistency > cleverness
6. **Read Error Messages**: They usually tell you exactly what's wrong
7. **Commit Often**: Small commits make it easy to rollback
8. **Ask for Reviews**: Use `@AGENTS.md` to guide code review

---

## Related Prompts

After feature development, consider:
- **Code Review Prompt**: "Review this feature implementation against @AGENTS.md conventions"
- **Test Coverage Prompt**: "Add comprehensive tests for [feature] following testing guidelines in @AGENTS.md"
- **Documentation Prompt**: "Update @AGENTS.md with new patterns introduced by [feature]"
- **Refactoring Prompt**: "Refactor [feature] to better follow patterns in @AGENTS.md"

---

## Template Checklist

Copy this checklist for quick reference:

```markdown
## Feature: [FEATURE_NAME]

- [ ] 1. Entity & Database Schema (all DBs)
- [ ] 2. Repository Interface
- [ ] 3. Controller Endpoints
- [ ] 4. Thymeleaf Templates
- [ ] 5. Validation & Error Handling
- [ ] 6. Unit Tests (Controller)
- [ ] 7. Manual Testing (all DB profiles)
- [ ] 8. Code Formatting (./mvnw spring-javaformat:apply)
- [ ] 9. Checkstyle (./mvnw checkstyle:check)
- [ ] 10. Full Build (./mvnw clean verify)
- [ ] 11. Integration Testing
- [ ] 12. Documentation Updated
```

---

**End of Feature Development Template**  
*Use this template to ensure consistent, high-quality feature implementations that follow your project's conventions.*

