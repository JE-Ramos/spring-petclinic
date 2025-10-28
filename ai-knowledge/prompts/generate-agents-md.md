# Prompt: Generate AGENTS.md for Your Project

## Purpose
This prompt helps you create a comprehensive `AGENTS.md` file that serves as the single source of truth for AI assistants working on your codebase.

---

## The Prompt

```
I need you to create a comprehensive AGENTS.md file for my project that will serve as the primary knowledge base for AI coding assistants like Cursor, GitHub Copilot, and other LLM-based tools.

Please analyze my codebase and create an AGENTS.md that includes:

## 1. Setup Commands
- Prerequisites (languages, frameworks, tools, versions)
- Installation steps
- Build commands
- Run/start development server commands
- Test commands
- Common utility commands (lint, format, deploy, etc.)

## 2. Project Structure
- Directory tree showing main folders and their purposes
- Where to find key components (models, controllers, views, services, etc.)
- Configuration file locations
- Test structure

## 3. Architecture & Patterns
- Technology stack (frameworks, libraries, databases)
- Design patterns used (MVC, Repository, Service layer, etc.)
- Key architectural decisions
- Data flow and relationships
- Authentication/authorization approach (if applicable)

## 4. Code Style & Conventions
- Language-specific conventions
- Naming conventions for files, classes, functions, variables
- Code organization rules
- Import/dependency management
- Documentation standards (comments, docstrings, JSDoc, JavaDoc, etc.)

## 5. Development Workflow
- Git workflow (branching strategy, commit messages)
- PR/MR process
- How to add new features/endpoints/components
- Database migrations process
- Environment management (dev, staging, prod)

## 6. Testing Guidelines
- Testing frameworks used
- Test file naming and location conventions
- What to test (unit, integration, e2e)
- How to run specific test suites
- Coverage expectations

## 7. Common Development Tasks
Step-by-step guides for:
- Adding a new entity/model
- Adding a new endpoint/route
- Adding a new UI component
- Database schema changes
- Adding dependencies

## 8. Troubleshooting
- Common issues and solutions
- Debugging tips
- Port conflicts
- Database connection issues
- Build failures
- Environment setup problems

## 9. API/Endpoints Documentation (if applicable)
- Key endpoints and their purposes
- Authentication requirements
- Common request/response patterns

## 10. Additional Resources
- Links to framework documentation
- Internal wiki or confluence pages
- Design documents
- API documentation
- Deployment guides

---

Please make it:
✅ Concise but comprehensive
✅ Code-focused with actual commands and examples
✅ Practical (include copy-pastable commands)
✅ Structured for easy scanning by both humans and AI
✅ Focused on what developers need to know to be productive immediately

Analyze these files to understand the project:
@[list key files: package.json, pom.xml, requirements.txt, README.md, etc.]
@[key source directories]
```

---

## How to Use This Prompt

### Step 1: Prepare Context
Open your project in Cursor and select relevant files to give context:
- Package manager files: `@package.json`, `@pom.xml`, `@requirements.txt`, `@Gemfile`, etc.
- Configuration: `@README.md`, `.env.example`, config files
- Key source files: Controllers, models, main entry points

### Step 2: Customize the Prompt
Adjust sections based on your project type:
- **Web API**: Emphasize endpoints, middleware, database
- **Frontend**: Focus on components, state management, styling
- **CLI Tool**: Highlight commands, flags, configuration
- **Library**: Document public API, usage examples

### Step 3: Run in Cursor Chat
1. Open Cursor Chat (Cmd+L)
2. Attach relevant files with @ mentions
3. Paste the customized prompt
4. Review and refine the output

### Step 4: Iterate
Ask follow-up questions to improve:
- "Add more details about the authentication flow"
- "Include examples for common REST patterns we use"
- "Add troubleshooting for Docker issues"

### Step 5: Save and Maintain
- Save as `ai-knowledge/AGENTS.md`
- Update when architecture changes
- Add new sections as patterns emerge
- Keep commands up-to-date

---

## Example Variations by Project Type

### For a React/Next.js Project
```
Focus on:
- Component structure and naming
- State management approach (Redux, Context, Zustand)
- Styling conventions (CSS modules, Tailwind, styled-components)
- API integration patterns
- Routing conventions
```

### For a Python Django/Flask Project
```
Focus on:
- Virtual environment setup
- Database migrations (Django ORM, Alembic)
- View/serializer patterns
- Testing with pytest
- Environment variables and configuration
```

### For a Java Spring Boot Project
```
Focus on:
- Maven/Gradle commands
- Application profiles (dev, test, prod)
- JPA entity relationships
- Controller/Service/Repository layers
- Testing with JUnit and MockMvc
```

### For a Mobile App (React Native, Flutter)
```
Focus on:
- Simulator/emulator commands
- Platform-specific code
- Navigation structure
- State management
- Build and release process
```

### For a Microservices Architecture
```
Focus on:
- Service communication patterns
- API gateway configuration
- Service discovery
- Deployment strategy
- Inter-service dependencies
```

---

## What Makes a Great AGENTS.md?

### ✅ DO:
- Use actual commands that can be copy-pasted
- Include file paths and directory structure
- Document patterns consistently used
- Add code snippets showing conventions
- Link to relevant external docs
- Update when architecture changes
- Keep it focused and scannable

### ❌ DON'T:
- Write generic descriptions without specifics
- Include outdated commands or paths
- Document every single file
- Write long paragraphs (use lists!)
- Forget to update when codebase evolves
- Include sensitive information (keys, passwords)

---

## Testing Your AGENTS.md

After creating AGENTS.md, test it with these prompts in Cursor:

1. **Setup Test**:
   ```
   @AGENTS.md How do I set up this project for local development?
   ```

2. **Feature Addition Test**:
   ```
   @AGENTS.md Following project conventions, add a new [entity/endpoint/component]
   ```

3. **Architecture Test**:
   ```
   @AGENTS.md Explain the architecture of this application
   ```

4. **Troubleshooting Test**:
   ```
   @AGENTS.md I'm getting error X, how do I fix it?
   ```

If the AI gives accurate, helpful responses using your AGENTS.md, it's working!

---

## Maintenance Schedule

### When to Update AGENTS.md:

📅 **Immediately**:
- Major architecture changes
- New frameworks or libraries added
- Development workflow changes
- New deployment process

📅 **Regularly** (monthly):
- Verify commands still work
- Update version numbers
- Add new troubleshooting tips learned

📅 **As Needed**:
- New team member onboarding feedback
- Common questions in PRs/Slack
- Repeated issues found

---

## Integration with .cursor/rules

Create a rule file that points to AGENTS.md:

**File**: `.cursor/rules/project-context.mdc`
```markdown
---
alwaysApply: true
---
To understand this project's architecture, conventions, and development workflow, 
always reference @AGENTS.md in ai-knowledge/AGENTS.md
```

This ensures AI assistants always have access to your project knowledge.

---

## Advanced: Multiple AGENTS Files

For large projects, consider splitting by domain:

```
ai-knowledge/
├── AGENTS.md                  # Main overview
├── AGENTS-BACKEND.md          # Backend specific
├── AGENTS-FRONTEND.md         # Frontend specific
├── AGENTS-INFRASTRUCTURE.md   # DevOps, deployment
└── AGENTS-TESTING.md          # Testing strategies
```

Then reference them in specific .cursor rules for different directories.

---

## Examples from Real Projects

### Minimal AGENTS.md (Small Project)
- Setup: 3-4 commands
- Structure: Simple tree
- Patterns: 2-3 key patterns
- Common tasks: 3-5 tasks
- ~100 lines total

### Comprehensive AGENTS.md (Large Project)
- Setup: Multiple environments
- Structure: Detailed with examples
- Patterns: 10+ with code samples
- Architecture diagrams (Mermaid)
- Extensive troubleshooting
- ~300-500 lines total

---

## Pro Tips

1. **Use Code Blocks**: Make commands copy-pastable
2. **Add Comments**: Explain WHY, not just WHAT
3. **Include Examples**: Show actual file names and paths
4. **Version Control**: Keep AGENTS.md in git
5. **Link Related Docs**: Point to ADRs, wikis, diagrams
6. **Get Team Buy-in**: Make it a living document
7. **Test with AI**: Regularly verify AI can use it effectively

---

## Success Metrics

Your AGENTS.md is successful if:
- ✅ New developers can set up in < 30 minutes
- ✅ AI generates code following your conventions
- ✅ Common questions are answered in the doc
- ✅ PR reviews cite it for standards
- ✅ Team regularly references it
- ✅ Onboarding time decreased significantly

---

## Related Files

After creating AGENTS.md, consider creating:
- `CONTRIBUTING.md` - For human contributors
- `ARCHITECTURE.md` - Deep-dive system design
- `ADR/` folder - Architecture Decision Records
- `.cursor/rules/` - AI-specific rules
- `.cursorignore` - Optimize AI context

---

## Template Repository

For a quick start, use this template structure:

```markdown
# [Project Name] - AGENTS.md

## Setup Commands
[Copy-paste commands here]

## Project Structure
[Directory tree]

## Architecture & Patterns
[Tech stack and patterns]

## Code Style & Conventions
[Your conventions]

## Common Development Tasks
[Step-by-step guides]

## Troubleshooting
[Known issues and solutions]

## Resources
[External links]
```

Then fill in each section using the detailed prompt above.

