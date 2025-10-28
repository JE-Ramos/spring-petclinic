# DELETE Owner Feature - Cursor AI Demo Script

## Demo Overview
**Duration**: 20 minutes  
**Goal**: Demonstrate three Cursor AI modes by implementing a complete DELETE owner feature  
**Branch**: `demo/delete-owner-feature`

---

## Setup Before Demo

### Pre-Demo Checklist
- [ ] Application is running: `./mvnw spring-boot:run`
- [ ] Access http://localhost:8080 in browser
- [ ] Have OwnerController.java open
- [ ] Have terminal visible in Cursor
- [ ] Clean git state on demo branch
- [ ] Backup: `git stash` if needed to restart

### Files You'll Touch
1. `src/main/java/org/springframework/samples/petclinic/owner/OwnerController.java`
2. `src/main/resources/templates/owners/ownerDetails.html`
3. `src/test/java/org/springframework/samples/petclinic/owner/OwnerControllerTests.java` (new)

---

## ACT 1: Cmd+K Inline Editing (5-7 minutes)

### Demo Goal
Show how Cmd+K is perfect for quick, focused edits within a single file.

---

### Step 1.1: Add Basic DELETE Endpoint

**File**: `OwnerController.java`

**Action**:
1. Open `OwnerController.java`
2. Scroll to line ~110 (after `processUpdateOwnerForm` method)
3. Click at the end of the method (after closing brace)
4. Press Enter twice to create space
5. **Press Cmd+K**

**Exact Prompt**:
```
Add a DELETE endpoint to remove an owner by ID. Use @GetMapping with path /owners/{ownerId}/delete, delete the owner from repository, and redirect to the owners list page.
```

**Expected AI Output**:
```java
@GetMapping("/owners/{ownerId}/delete")
public String deleteOwner(@PathVariable("ownerId") int ownerId) {
    this.owners.deleteById(ownerId);
    return "redirect:/owners";
}
```

**Talking Points**:
- ✅ AI understood Spring MVC patterns from context
- ✅ Used existing repository field (`this.owners`)
- ✅ Followed redirect pattern from other methods
- ✅ Matched naming conventions (`deleteOwner`)

---

### Step 1.2: Add Validation (Iterative Refinement)

**Action**:
1. Select the ENTIRE `deleteOwner` method you just created
2. **Press Cmd+K**

**Exact Prompt**:
```
Add validation to check if the owner exists before deleting. If not found, add an error message and redirect to owners list. Also check if owner has pets - if yes, prevent deletion and show error message "Cannot delete owner with existing pets"
```

**Expected AI Output**:
```java
@GetMapping("/owners/{ownerId}/delete")
public String deleteOwner(@PathVariable("ownerId") int ownerId, RedirectAttributes redirectAttributes) {
    Owner owner = this.owners.findById(ownerId);
    
    if (owner == null) {
        redirectAttributes.addFlashAttribute("error", "Owner not found");
        return "redirect:/owners";
    }
    
    if (!owner.getPets().isEmpty()) {
        redirectAttributes.addFlashAttribute("error", "Cannot delete owner with existing pets");
        return "redirect:/owners/" + ownerId;
    }
    
    this.owners.deleteById(ownerId);
    redirectAttributes.addFlashAttribute("message", "Owner deleted successfully");
    return "redirect:/owners";
}
```

**Talking Points**:
- ✅ AI added `RedirectAttributes` parameter (Spring pattern)
- ✅ Used existing Owner model methods (`getPets()`, `isEmpty()`)
- ✅ Added both error and success messages
- ✅ Smart redirect (back to owner page if has pets)
- ⚡ **This is iterative refinement - key Cmd+K strength!**

---

### Step 1.3: Add JavaDoc (Quick Enhancement)

**Action**:
1. Place cursor on the line ABOVE `@GetMapping` annotation
2. **Press Cmd+K**

**Exact Prompt**:
```
Add comprehensive JavaDoc for this method
```

**Expected AI Output**:
```java
/**
 * Deletes an owner from the system.
 * Validates that the owner exists and has no associated pets before deletion.
 * 
 * @param ownerId the ID of the owner to delete
 * @param redirectAttributes flash attributes for success/error messages
 * @return redirect to owners list or owner details page
 */
@GetMapping("/owners/{ownerId}/delete")
public String deleteOwner(@PathVariable("ownerId") int ownerId, RedirectAttributes redirectAttributes) {
    // ... rest of method
}
```

**Talking Points**:
- ✅ Cmd+K is FAST for documentation
- ✅ No need to type boilerplate
- ✅ Consistent JavaDoc style

---

## ACT 2: Terminal Integration (3-5 minutes)

### Demo Goal
Show how to test changes using terminal within Cursor.

---

### Step 2.1: Build Application

**Action**:
1. Open terminal in Cursor (Cmd+J or bottom panel)
2. Run build command

**Command**:
```bash
./mvnw clean compile
```

**Talking Points**:
- ✅ Verify no compilation errors
- ✅ Terminal integrated in workspace
- ✅ Quick feedback loop

---

### Step 2.2: Test the Endpoint Manually

**Action**:
1. Ensure application is running
2. Open browser to http://localhost:8080
3. Navigate to "Find Owners"
4. Search for "Franklin"
5. Click on "George Franklin"
6. Manually test by going to: http://localhost:8080/owners/1/delete

**Expected Behavior**:
- Should show error: "Cannot delete owner with existing pets"
- Should redirect back to owner details

**Find an owner without pets**:
- Search for owners with no pets (check database)
- Or create a new owner without pets
- Test deletion - should succeed

**Talking Points**:
- ✅ Real-world testing
- ✅ Validation works as expected
- ✅ Error messages display correctly

---

### Step 2.3: Use Cmd+K in Terminal (Optional - if time permits)

**Action**:
1. In terminal, **press Cmd+K**
2. Type prompt

**Exact Prompt**:
```
Write a curl command to test the delete endpoint for owner ID 1
```

**Expected Output**:
```bash
curl -X GET http://localhost:8080/owners/1/delete -L
```

**Talking Points**:
- ✅ Cmd+K works in terminal too!
- ✅ Generate test commands quickly
- ✅ `-L` flag follows redirects

---

## ACT 3: Full Agent/Chat Mode (7-10 minutes)

### Demo Goal
Show when to switch from Cmd+K to Chat for complex, multi-file changes.

---

### Step 3.1: Architectural Discussion

**Action**:
1. Open Cursor Chat (Cmd+L)
2. Start with context

**Exact Prompt**:
```
@OwnerController @AGENTS.md I want to improve the delete functionality. Instead of permanent deletion, I'd like to implement soft delete where we just mark owners as deleted but keep them in the database. What changes do I need to make across the codebase?
```

**Expected AI Response** (Key Points):
1. Add `deleted` boolean field to Owner entity
2. Add `deletedAt` timestamp field (optional)
3. Update repository to filter out deleted owners
4. Modify delete method to set flag instead of removing
5. Update queries to exclude deleted records
6. Consider: Admin view to see deleted owners

**Talking Points**:
- ✅ Chat is perfect for architectural questions
- ✅ AI provides multi-file change plan
- ✅ References your project conventions via @AGENTS.md
- ⚡ **This is when you SWITCH from Cmd+K to Chat!**

---

### Step 3.2: Add Delete Button to UI

**Action**: Continue in Chat

**Exact Prompt**:
```
@ownerDetails.html I need to add a delete button to the owner details page. Show me the Thymeleaf code to add a delete button with a confirmation dialog. The button should be styled consistently with existing buttons and only show if the owner has no pets.
```

**Expected AI Response**:
```html
<!-- Add to ownerDetails.html after the Edit Owner button -->
<a th:if="${owner.pets.isEmpty()}" 
   class="btn btn-danger" 
   th:href="@{/owners/{ownerId}/delete(ownerId=${owner.id})}"
   onclick="return confirm('Are you sure you want to delete this owner?');">
  Delete Owner
</a>

<div th:if="${!owner.pets.isEmpty()}" class="alert alert-warning">
  Cannot delete owner with existing pets. Please remove or transfer pets first.
</div>
```

**Talking Points**:
- ✅ AI generates view code matching existing patterns
- ✅ Adds JavaScript confirmation (onclick)
- ✅ Conditional rendering based on pets
- ✅ Helpful warning message for users

**Action**: Copy this code and add it to the template (show file)

---

### Step 3.3: Generate Comprehensive Tests

**Action**: Continue in Chat

**Exact Prompt**:
```
@OwnerController Generate comprehensive unit tests for the deleteOwner method. Include test cases for: successful deletion, owner not found, owner has pets, and verify redirect URLs and flash messages.
```

**Expected AI Response**:
```java
@WebMvcTest(OwnerController.class)
class OwnerControllerDeleteTests {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private OwnerRepository owners;

    @Test
    void testDeleteOwner_Success() throws Exception {
        Owner owner = new Owner();
        owner.setId(1);
        owner.setFirstName("John");
        owner.setLastName("Doe");
        
        given(this.owners.findById(1)).willReturn(owner);
        
        mockMvc.perform(get("/owners/1/delete"))
            .andExpect(status().is3xxRedirection())
            .andExpect(redirectedUrl("/owners"))
            .andExpect(flash().attribute("message", "Owner deleted successfully"));
        
        verify(this.owners).deleteById(1);
    }

    @Test
    void testDeleteOwner_NotFound() throws Exception {
        given(this.owners.findById(99)).willReturn(null);
        
        mockMvc.perform(get("/owners/99/delete"))
            .andExpect(status().is3xxRedirection())
            .andExpect(redirectedUrl("/owners"))
            .andExpect(flash().attribute("error", "Owner not found"));
        
        verify(this.owners, never()).deleteById(anyInt());
    }

    @Test
    void testDeleteOwner_HasPets() throws Exception {
        Owner owner = new Owner();
        owner.setId(1);
        Pet pet = new Pet();
        owner.addPet(pet);
        
        given(this.owners.findById(1)).willReturn(owner);
        
        mockMvc.perform(get("/owners/1/delete"))
            .andExpect(status().is3xxRedirection())
            .andExpect(redirectedUrl("/owners/1"))
            .andExpect(flash().attribute("error", "Cannot delete owner with existing pets"));
        
        verify(this.owners, never()).deleteById(anyInt());
    }
}
```

**Talking Points**:
- ✅ AI generates complete test class
- ✅ Follows Spring Boot test conventions
- ✅ Covers all edge cases
- ✅ Uses MockMvc and proper assertions
- ⚡ **Chat excels at generating new files!**

**Action**: Create new test file and paste code (show file creation)

---

### Step 3.4: Refactoring Suggestion

**Action**: Continue in Chat

**Exact Prompt**:
```
Looking at this implementation, what improvements would you suggest? Consider error handling, security, and best practices.
```

**Expected AI Suggestions**:
1. **POST instead of GET**: DELETE should use POST/DELETE method for safety
2. **CSRF protection**: Ensure CSRF token included
3. **Authorization**: Check if user has permission to delete
4. **Logging**: Add audit logging for deletions
5. **Transaction**: Wrap in @Transactional
6. **Service layer**: Move logic to OwnerService

**Talking Points**:
- ✅ Chat provides architectural feedback
- ✅ Identifies security concerns
- ✅ Suggests best practices
- ✅ Educational - learn as you build!

---

### Step 3.5: Implement One Improvement (POST Method)

**Action**: Continue in Chat

**Exact Prompt**:
```
Show me how to convert the delete endpoint to use POST method with proper CSRF protection and update the HTML form accordingly.
```

**Expected AI Response**:

**Controller**:
```java
@PostMapping("/owners/{ownerId}/delete")
public String deleteOwner(@PathVariable("ownerId") int ownerId, 
                          RedirectAttributes redirectAttributes) {
    // ... same validation logic ...
}
```

**HTML Form**:
```html
<form th:if="${owner.pets.isEmpty()}" 
      th:action="@{/owners/{ownerId}/delete(ownerId=${owner.id})}" 
      method="post"
      onsubmit="return confirm('Are you sure you want to delete this owner?');">
  <input type="hidden" th:name="${_csrf.parameterName}" th:value="${_csrf.token}"/>
  <button type="submit" class="btn btn-danger">Delete Owner</button>
</form>
```

**Talking Points**:
- ✅ Changed to POST (more secure)
- ✅ Added CSRF token
- ✅ Better HTTP semantics
- ✅ Form-based approach

---

## Summary & Teaching Moments

### When to Use Each Mode

#### ✅ Cmd+K (Inline Editing)
**Best For**:
- Single method additions/modifications
- Quick refactoring in one location
- Adding documentation
- Error handling improvements
- Iterative refinement of small sections

**Advantages**:
- ⚡ FAST - immediate visual feedback
- 🎯 Focused - stays in context
- 🔄 Iterative - easy to refine
- 👀 Visual - see changes instantly

**Demo Examples**:
- Adding deleteOwner method
- Adding validation
- Adding JavaDoc

---

#### 💻 Terminal Integration
**Best For**:
- Building and compiling
- Running tests
- Generating commands
- Checking logs
- Quick verification

**Advantages**:
- 🔧 Integrated workflow
- ⚡ Quick feedback
- 🧪 Test immediately

**Demo Examples**:
- Compiling code
- Manual testing in browser
- Generating curl commands

---

#### 🤖 Chat/Agent Mode
**Best For**:
- Architectural decisions
- Multi-file changes
- Generating new files (tests, configs)
- Understanding existing code
- Planning refactoring
- Security/best practice reviews

**Advantages**:
- 🧠 Holistic view
- 📚 Educational
- 🗺️ Planning
- 📁 Multi-file operations
- 🔍 Code review

**Demo Examples**:
- Soft delete architecture discussion
- Generating test files
- Creating UI components
- Security improvements
- Refactoring suggestions

---

## Decision Tree for Audience

```
Need to make a change?
│
├─ Single method/function?
│  └─> Use Cmd+K (Inline)
│
├─ Need to test/run commands?
│  └─> Use Terminal
│
├─ Multi-file or need architecture advice?
│  └─> Use Chat/Agent Mode
│
└─ Complex refactoring spanning many files?
   └─> Start with Chat (plan), then Cmd+K (execute)
```

---

## Cleanup After Demo

### Reset for Next Run
```bash
# Stash all changes
git stash

# Or commit as example
git add .
git commit -m "Demo: DELETE owner feature complete"

# Switch back to main
git checkout main
```

### Keep Demo Branch
```bash
# Push demo branch for reference
git push origin demo/delete-owner-feature
```

---

## Troubleshooting

### If Cmd+K Doesn't Work As Expected
1. Try rephrasing the prompt
2. Add more context: "Following the pattern in processUpdateOwnerForm..."
3. Select more surrounding code for context
4. Check if file is saved

### If Chat Gives Wrong Suggestions
1. Use @ mentions to add more context
2. Reference specific files: @OwnerController
3. Point to conventions: @AGENTS.md
4. Be more specific about requirements

### If Code Doesn't Compile
1. Check imports were added
2. Verify method signatures match
3. Run `./mvnw clean compile` to see exact errors
4. Use Chat to debug: "I'm getting error X, how do I fix it?"

---

## Q&A Preparation

### Expected Questions

**Q: Why use GET for delete in the first version?**
A: To show progression! Start simple, then improve. Real-world: always use POST/DELETE.

**Q: Can Cursor write tests automatically?**
A: Yes via Chat, but you must review! AI is a pair programmer, not autopilot.

**Q: What if AI suggests bad code?**
A: Always review! AI follows patterns - if patterns are bad, AI will copy them. Keep clean codebase.

**Q: Does this work offline?**
A: No, Cursor requires internet. But responses are fast (usually < 2 seconds).

**Q: How much does Cursor cost?**
A: Free tier available. Pro tier ~$20/month. Worth it for productivity boost.

**Q: Can whole team share .cursor configs?**
A: Yes! Commit .cursor/ and .cursorignore to git. Everyone benefits.

---

## Post-Demo Actions

### Share with Attendees
1. ✅ GitHub repo link with all commits
2. ✅ This demo script
3. ✅ CURSOR_DEMO_TOPICS.md
4. ✅ Link to Cursor documentation

### Follow-up Resources
- Cursor Discord community
- MCP documentation
- Spring Boot best practices
- More complex examples in repo

---

## Time Management

| Section | Time | Cumulative |
|---------|------|------------|
| Act 1: Cmd+K (3 steps) | 5-7 min | 7 min |
| Act 2: Terminal | 3-5 min | 12 min |
| Act 3: Chat/Agent (5 steps) | 7-10 min | 22 min |
| **Total Demo** | **15-22 min** | |
| Buffer for Q&A during demo | +5 min | 27 min |

**Recommendation**: 
- Practice to hit 20 minutes
- Save 5 minutes for questions
- Total = 25 minutes of Part 4

---

## Success Criteria

By end of demo, audience should understand:
- ✅ When to use Cmd+K vs Chat
- ✅ How to iterate with inline editing
- ✅ How to leverage project context
- ✅ Terminal integration benefits
- ✅ Chat's strength for complex tasks
- ✅ Practical, real-world workflow

**Most Important**: They should WANT to try it themselves!

