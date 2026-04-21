# AI Specifications & Development Rules

This repository contains a comprehensive set of development rules, standards, and AI agent configurations designed to work seamlessly with multiple AI coding copilots. The setup is portable and can be imported into any project to provide consistent, high-quality AI-assisted development.

It's highly recommended to be used along with Spec-Driven Development frameworks like [OpenSpec](https://github.com/Fission-AI/OpenSpec)

## 📁 Repository Structure

```
.
├── docs/                        # Development standards and specifications
│   ├── base-standards.mdc       # Core development rules (single source of truth)
│   ├── backend-standards.mdc
│   ├── frontend-standards.mdc
│   ├── documentation-standards.mdc
│   ├── api-spec.yml             # OpenAPI specification
│   ├── data-model.md            # Database and domain models
│   ├── development_guide.md
│   └── plans/                   # Fallback plan location (when OpenSpec is not installed)
├── ai-specs/
│   ├── .commands/               # Reusable command prompts (plan, develop, enrich, etc.)
│   └── .agents/                 # Agent role definitions (backend, frontend, analyst, etc.)
│
├── AGENTS.md                    # Generic agent configuration
├── CLAUDE.md                    # Claude-specific configuration
├── codex.md                     # GitHub Copilot/Codex configuration
└── GEMINI.md                    # Gemini-specific configuration
```

## 🤖 Multi-Copilot Support

This repository uses **symbolic links** or **naming conventions** to support multiple AI coding copilots without duplication:

- **`AGENTS.md`** → Generic agent rules (works with most copilots)
- **`CLAUDE.md`** → Optimized for Claude/Cursor
- **`codex.md`** → Optimized for GitHub Copilot/Codex
- **`GEMINI.md`** → Optimized for Google Gemini

All these files reference the same core rules in `docs/base-standards.mdc`, ensuring consistency across different AI tools while allowing copilot-specific customizations.

### Why This Approach?

✅ **Single Source of Truth**: Core rules maintained in one place (`base-standards.mdc`)  
✅ **Copilot Compatibility**: Each AI tool finds its configuration using its preferred naming convention  
✅ **Zero Configuration**: Import into a new project and it works immediately  
✅ **Easy Updates**: Update rules once, all copilots benefit  
✅ **Portable**: Copy this structure to any project  

## 🚀 Quick Start

### 1. (Recommended) Install and Initialize OpenSpec

OpenSpec works great with this repository and is recommended for a spec-driven workflow.

Quick Start requirements from OpenSpec official docs:

- Node.js `20.19.0` or higher

Install OpenSpec globally:

```bash
npm install -g @fission-ai/openspec@latest
```

Then navigate to your project and initialize:

```bash
cd your-project
openspec init
```

### 2. Point `config.yml` to Your `docs` Folder

After `openspec init`, update your project's `config.yml` to include your technical context from `docs`.

Example (`config.yml`):

```yml
context: |
  Tech stack: TypeScript, Node.js, Express, Prisma, Domain-Driven Design (DDD)
  Architecture: Clean Architecture with Domain, Application, and Presentation layers
  We use conventional commits
  Domain: LTI (Leadership. Technology. Impact) ATS platform
  All code, comments, documentation, and technical artifacts must be in English

  Project specs (single source of truth): All artifact creation and implementation MUST follow the project's technical context in ai-specs/. Read and apply these when creating or implementing:
  - docs/base-standards.mdc — core principles, TDD, language standards, links to backend/frontend/docs standards
  - docs/backend-standards.mdc — API, database, testing, security (backend changes)
  - docs/frontend-standards.mdc — React, UI/UX (frontend changes)
  - docs/api-spec.yml — API contracts and endpoint definitions
  - docs/data-model.md — domain and data model
  - docs/documentation-standards.mdc — docs structure and maintenance
  For implementation: adopt the relevant agent from ai-specs/.agents/ (e.g. backend-developer.md for backend, frontend-developer.md for frontend). Use ai-specs/.commands/ (e.g. develop-backend.md, develop-frontend.md) for workflow guidance when applicable.

# Per-artifact rules (optional)
# Add custom rules for specific artifacts.
rules:
  # Global: apply ai-specs when creating any artifact
  _global:
    - Before creating any artifact, read and apply docs/base-standards.mdc
    - For backend-related artifacts, read docs/backend-standards.mdc and adopt guidelines from ai-specs/.agents/backend-developer.md
    - For frontend-related artifacts, read docs/frontend-standards.mdc and adopt guidelines from ai-specs/.agents/frontend-developer.md
    - Use docs/api-spec.yml and docs/data-model.md for API and data consistency in specs and tasks
```

### 3. Import Into Your Project

```bash
# Clone or copy this repository into your project
cp -r LIDR-ai-specs/* your-project/

# The AI copilot will automatically detect the relevant configuration file
```

### 4. Verify Configuration

Your AI copilot will automatically load:
- **Claude/Cursor**: `CLAUDE.md` → `docs/base-standards.mdc`
- **GitHub Copilot**: `codex.md` → `docs/base-standards.mdc`
- **Gemini**: `GEMINI.md` → `docs/base-standards.mdc`

All paths and rules are configured to work seamlessly without manual adjustments.

## 💡 Usage: Command-Based Development Workflow

The most efficient way to work with this setup is using a command-based workflow:

### Step 1: Enrich the User Story (Optional)

If your user story lacks detail or acceptance criteria, use the **`enrich-us`** command to enhance it:

```
/enrich-us SCRUM-10
```

This command analyzes the user story and generates:
- Detailed acceptance criteria
- Edge cases and validation rules
- Technical considerations
- Testing scenarios

**Note**: Skip this step if your user story already has sufficient depth and clear requirements.

### Step 2: Plan the Feature

This step is a **manual implementation** of the essential SDD planning phase.
If OpenSpec is installed, prefer running the standard OpenSpec planning commands directly.

Use **`plan-ticket`** commands to generate detailed implementation plans:

```
plan-backend-ticket SCRUM-10
```

or

```
plan-frontend-ticket SCRUM-15
```

This creates a comprehensive, step-by-step implementation plan in OpenSpec's default `changes/` directory.
If OpenSpec is not installed, use the fallback location `docs/plans/`.

### Step 3: Implement the Feature

This step is a **manual implementation** of the essential SDD execution phase.
If OpenSpec is installed, prefer running the standard OpenSpec implementation commands directly.

Reference the generated plan and execute:

```
develop-backend @SCRUM-10_backend.md
```

or

```
develop-frontend @SCRUM-15_frontend.md
```

The AI will follow the plan precisely, implementing each step with TDD, proper testing, and documentation updates.

### Example: Implementing SCRUM-10 (Position Update Feature)

#### Step 1: Enrich the User Story (Optional)

**You say:**
```
/enrich-us SCRUM-10
```

**AI enhances** the user story with detailed acceptance criteria and technical considerations (skip if already detailed).

#### Step 2: Generate the Plan

**You say:**
```
/plan-backend-ticket SCRUM-10
```

**AI generates:**
- Analyzes the ticket requirements
- Creates `changes/SCRUM-10_backend.md` with (or `docs/plans/SCRUM-10_backend.md` as fallback):
  - Architecture context
  - Step-by-step implementation instructions
  - Complete test specifications (validation, service, controller layers)
  - API documentation updates
  - Validation rules
  - Error handling strategies

#### Step 3: Implement Following the Plan

**You say:**
```
/develop-backend @SCRUM-10_backend.md
```

**AI executes:**
1. Creates feature branch `feature/SCRUM-10-backend`
2. Implements validation function with comprehensive rules
3. Implements service layer with business logic
4. Implements controller with HTTP handling
5. Adds route configuration
6. Writes 90%+ test coverage across all layers
7. Updates API documentation
8. Runs tests and verifies implementation
9. Commits and pushes (configurable to wait until confirmation)

### 📝 Generated Plan Artifacts

Implementation plans and enriched user story artifacts are generated in `changes/` when using OpenSpec.
If OpenSpec is not installed, store them in `docs/plans/`.

Use these files as the single reference for implementation details, testing checklists, and documentation updates required by each ticket.

## 📖 Core Development Rules

All development follows principles defined in `docs/base-standards.mdc`:

### Key Principles

1. **Small Tasks, One at a Time**: Baby steps, never skip ahead
2. **Test-Driven Development (TDD)**: Write failing tests first
3. **Type Safety**: Fully typed code (TypeScript)
4. **Clear Naming**: Descriptive variables and functions
5. **English Only**: All code, comments, documentation, and messages in English
6. **90%+ Test Coverage**: Comprehensive testing across all layers
7. **Incremental Changes**: Focused, reviewable modifications

### Specific Standards

- **Backend Standards**: `docs/backend-standards.mdc`
  - API development patterns
  - Database best practices
  - Security guidelines
  - Testing requirements

- **Frontend Standards**: `docs/frontend-standards.mdc`
  - React component patterns
  - UI/UX guidelines
  - State management
  - Component testing

- **Documentation Standards**: `docs/documentation-standards.mdc`
  - Technical documentation structure
  - API documentation (OpenAPI)
  - Code documentation
  - Maintenance guidelines

## 🎯 Benefits

### For Developers
- ✅ **Consistent Code Quality**: AI follows the same standards every time
- ✅ **Comprehensive Testing**: Automatic 90%+ coverage across all layers
- ✅ **Complete Documentation**: API specs updated automatically
- ✅ **Faster Onboarding**: New team members reference the same rules
- ✅ **Reduced Review Time**: Code follows established patterns

### For Teams
- ✅ **Copilot Flexibility**: Team members can use their preferred AI tool
- ✅ **Knowledge Preservation**: Standards documented, not in people's heads
- ✅ **Quality Consistency**: Same standards regardless of who (or what) writes code
- ✅ **Easier Code Reviews**: Clear expectations and patterns
- ✅ **Scalable Practices**: Standards scale with the team

### For Projects
- ✅ **Maintainable Codebase**: Clean architecture and clear separation of concerns
- ✅ **Production-Ready Code**: TDD, error handling, and validation built-in
- ✅ **Living Documentation**: API specs and data models always current
- ✅ **Faster Feature Development**: Autonomous AI implementation from plans
- ✅ **Lower Technical Debt**: Best practices enforced from day one

## 🔧 Customization

### Adapting to Your Project

1. **Update technical context**: Find the different files in `docs` and modify core principles, coding standards, business rules and technical documentation to match your needs:
   - backend/frontend/testing/documentation standards
   - installation guide
   - data model
   - API docs
   - ...
2. **Adapt agents in `ai-specs/.agents`**: Adjust agent definitions to your project's roles and workflows
3. **Extend Commands**: Define battle-tested prompts into commands in `ai-specs/.commands` 
4. **Link Resources**: Reference your project's specific documentation or tasks using MCPs
5. **Keep the symlink structure**: Remember to create relative symlinks from claude and cursor folders to the newly created agents or commands to keep it consistent

### Maintaining Standards

- **Single Source of Truth**: Always update `base-standards.mdc` first
- **Version Control**: Track changes to standards like code
- **Team Review**: Standards changes should be reviewed like pull requests
- **Documentation**: Keep examples current with actual implementation

## 📚 Technical context

### Reference Examples (from LIDR Project)

The following files are included as **reference examples** from the LIDR project. You should create your own versions tailored to your specific project:

- **API Specification**: `docs/api-spec.yml` (OpenAPI 3.0 format)
  - *Create your own API spec documenting your project's endpoints*
- **Data Models**: `docs/data-model.md` (Database schemas, domain models)
  - *Document your database structure and domain entities*
- **Development Guide**: `docs/development_guide.md` (Setup, workflows)
  - *Write setup instructions specific to your tech stack*


## 🤝 Contributing

When contributing to the standards:

1. Update `base-standards.mdc` (single source of truth)
2. Test with multiple AI copilots to ensure compatibility
3. Update generated examples in `changes/` (or `docs/plans/` fallback) if needed
4. Document breaking changes clearly
5. Follow the same standards you're defining!

## 📄 License

Copyright (c) 2025 LIDR.co
Licensed under the MIT License

**English:**

The content of this repository is part of the AI4Devs program by LIDR.co. If you want to learn to code with AI like the pros and get more templates and resources like these, you can find all the information on the official website: https://lidr.co/ia-devs

**Español:**

El contenido de este repositorio es parte del programa AI4Devs de LIDR.co. Si quieres aprender a programar con IA como los pros, y obtener más plantillas y recursos como estos, puedes encontrar toda la información en la página oficial: https://lidr.co/ia-devs

---

**Made with 🤖 by the LIDR community**

For questions, issues, or suggestions, visit [LIDR.co](https://lidr.co/ia-devs)

