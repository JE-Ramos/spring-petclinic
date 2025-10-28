# Cursor AI Basics Demo - 50 Minutes

## Demo Overview
Making Spring PetClinic "Cursor AI Native" - A hands-on guide to setting up and using Cursor AI effectively.

---

## Part 1: Setting Up a Cursor-Native Project (15-20 min)

### 1.1 What Makes a Project "Cursor AI Native"?
- Understanding AI context and project structure
- Why explicit documentation helps AI assistants
- The `.cursor` folder and its purpose

### 1.2 Essential Setup Files

#### `.cursorignore`
- **Why**: Exclude noise from AI context (build artifacts, dependencies)
- **What to ignore**: 
  - Build outputs (`target/`, `build/`)
  - Dependencies (`node_modules/`, `.m2/`)
  - IDE files (`.idea/`, `.vscode/`)
  - Large binary files
- **Demo**: Show before/after context window efficiency

#### `.cursor/rules/` - Project Rules
- **Purpose**: Always-applied context for AI
- **What to include**:
  - Project structure pointers
  - Coding conventions
  - Common patterns
- **Example**: `project-context.mdc` pointing to AGENTS.md

#### `ai-knowledge/AGENTS.md` - Project Knowledge Base
- **Purpose**: Central source of truth for AI
- **What to include**:
  - Setup commands (build, run, test)
  - Project architecture
  - Development workflows
  - Code style and conventions
- **Demo**: Show AI using this in responses

#### `.cursor/mcp.json` - MCP Server Configuration
- **What is MCP**: Model Context Protocol for extended capabilities
- **Example**: GitHub integration for repo operations
- **Advanced**: Other useful MCPs (filesystem, database, etc.)

### 1.3 Git Workflow for Demo
- One commit per topic (makes git history a tutorial)
- Clear commit messages for teaching

---

## Part 2: Cmd+K Inline Editing (15-20 min)

### 2.1 Basic Inline Edits
- **Trigger**: Cmd+K (Mac) / Ctrl+K (Windows)
- **Use cases**:
  - Quick refactoring
  - Adding error handling
  - Writing boilerplate
  - Updating variable names

### 2.2 Context-Aware Edits
- **Demo**: How Cursor uses surrounding code
- **Example**: Adding a new endpoint that follows existing patterns
- **Show**: AI inferring:
  - Naming conventions
  - Return types
  - Error handling patterns
  - Documentation style

### 2.3 Multi-line Selections
- Select code block + Cmd+K
- **Examples**:
  - Refactor method to use streams
  - Add validation to controller
  - Convert to builder pattern
  - Add comprehensive logging

### 2.4 Using Project Rules in Edits
- **Demo**: Show how `.cursor/rules` influence suggestions
- **Example**: Code style automatically applied
- **Show**: Consistency across edits

### 2.5 Common Patterns
- "Add javadoc"
- "Add error handling"
- "Refactor to use dependency injection"
- "Add unit test for this method"
- "Make this method more defensive"

---

## Part 3: Using Context and Prompts (10-15 min)

### 3.1 @ Mentions - Explicit Context
- **@Files**: Reference specific files
- **@Folders**: Include entire directory context
- **@Code**: Reference specific functions/classes
- **@Docs**: Reference documentation
- **Demo**: Building a feature with multiple file context

### 3.2 Using AGENTS.md in Prompts
- **Example**: "Following @AGENTS.md, add a new controller"
- **Show**: AI following project conventions
- **Demo**: Consistency in generated code

### 3.3 Chat vs Inline Editing
- **When to use Chat**:
  - Architectural questions
  - Multi-file changes
  - Learning about codebase
  - Planning refactoring
- **When to use Inline (Cmd+K)**:
  - Quick, focused edits
  - Single method/function changes
  - Immediate visual feedback needed

### 3.4 Iterative Development
- **Demo**: Building a feature step-by-step
  1. Ask AI to plan the approach
  2. Generate skeleton code
  3. Use Cmd+K to refine each part
  4. Use rules to ensure consistency

### 3.5 Best Practices
- Be specific in prompts
- Reference project conventions
- Use existing code as examples
- Iterate on AI suggestions
- Review and understand generated code

---

## Part 4: Practical Demo - Adding a Feature (If Time Permits)

### 4.1 Example Feature: Add a "Pet Grooming" Service
- Plan with AI using project context
- Generate model classes (Cmd+K)
- Add controller following patterns
- Write tests using conventions
- Show how rules maintain consistency

### 4.2 Debugging with AI
- Explain error to AI
- Get context-aware solutions
- Use inline edits to fix

---

## Key Takeaways

### For Attendees:
1. ✅ Set up `.cursorignore` to optimize AI context
2. ✅ Create `.cursor/rules/` for consistent AI behavior
3. ✅ Maintain `AGENTS.md` as single source of truth
4. ✅ Use Cmd+K for quick, focused edits
5. ✅ Use @ mentions for explicit context
6. ✅ Leverage MCP for extended capabilities

### Success Metrics:
- AI generates code following project conventions
- Faster development with consistent patterns
- Better onboarding for new developers
- Codebase remains maintainable despite AI assistance

---

## Resources & Next Steps

### Demo Repository
- GitHub: [Your fork URL]
- Commits show progression of setup

### Recommended Plugins
1. Dev Containers (anysphere)
2. Mermaid Chart

### Further Learning
- Cursor documentation
- MCP protocol documentation
- Project-specific conventions

---

## Q&A Topics to Prepare For

1. How does Cursor compare to GitHub Copilot?
2. Does this work with other languages?
3. How to handle sensitive data/secrets?
4. Cost considerations?
5. Offline capabilities?
6. Team sharing of rules and conventions?
7. How to measure productivity improvements?

---

## Demo Checklist

### Before Demo:
- [ ] Clean git state
- [ ] Application runs locally
- [ ] Cursor settings configured
- [ ] Examples prepared
- [ ] Backup code ready

### During Demo:
- [ ] Show `.cursorignore` impact
- [ ] Demonstrate Cmd+K multiple times
- [ ] Use @ mentions effectively
- [ ] Show rules in action
- [ ] Generate real code

### After Demo:
- [ ] Share repository link
- [ ] Provide setup guide
- [ ] Answer questions
- [ ] Collect feedback

