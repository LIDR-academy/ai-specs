# Engineering Principles & Practices

## 1. Preamble: Our Engineering Manifesto

This document is the single source of truth for the engineering culture inside
our organization. It serves as a guide for every human developer and as a
**foundational instruction set for any AI Copilot or development agent** we employ.

Our goal is not to write code, but to build a reliable, secure, and maintainable
system that our clients love and trust. These principles are the tools we use to
achieve that goal. Every piece of code, documentation, and infrastructure must be
aligned with these practices.

## 2. Core Design Principles: Building for the Long Term

These principles govern the *structure* and *quality* of the code we write.

### Principles

- **SOLID:** Our code must be robust and adaptable.
  - **S**ingle Responsibility: Each class or module does one thing and does it well.
  - **O**pen/Closed: We extend behavior by adding new code, not by changing existing, working code.
  - **L**iskov Substitution: Subtypes can be substituted for their base types without altering correctness.
  - **I**nterface Segregation: Clients should not be forced to depend on interfaces they do not use.
  - **D**ependency Inversion: High-level modules should not depend on low-level modules; both should depend on abstractions.

- **KISS (Keep It Simple, Stupid):** We will always prefer the simplest, most
straightforward solution that correctly solves the problem. We will actively
resist over-engineering and premature optimization.

- **DRY (Don't Repeat Yourself):** Every piece of knowledge or logic must have a
single, unambiguous, authoritative representation within the system. We avoid code
duplication by creating abstractions (functions, classes, services).

- **The Boy Scout Rule:** A mandatory principle summarized as: "Always leave the
code better than you found it." Every time you touch a file, make a small
improvement: clarify a variable name, fix a typo, or break down a complex function.
This ensures the codebase continuously improves over time.

### >> AI Copilot Directives: Design Principles

- When generating code, **YOU MUST** adhere to SOLID principles. Explicitly call
out which principle you are applying if the context is complex.
- **YOU MUST** prioritize simplicity and clarity (KISS). If you propose a complex
design pattern, you must justify why a simpler alternative is insufficient.
- During refactoring tasks, your primary goal is to identify and eliminate
repeated code (DRY) by creating well-named abstractions.
- When modifying a file to fulfill a request, **YOU MUST** also identify and apply
at least one small, unrelated improvement in the surrounding code, such as
improving a comment, clarifying a variable name, or simplifying a logical
expression. Announce the improvement briefly.

---

## 3. Behavior & Testing Methodology: Building with Confidence

This section governs *how* we translate requirements into working, tested code.

### Practices

- **Gherkin for User Stories:** All new features or behaviors begin with a `.feature`
file written in Gherkin (`Given-When-Then`). This file is our **single source of
truth for system behavior** and serves as a living, executable specification that
is understandable by everyone, including non-technical stakeholders.

- **Test-Driven Design (TDD):** We write tests *before* we write implementation code. Our development rhythm follows the "Red-Green-Refactor" cycle:
  1. **Red:** Write a failing test that precisely defines the desired behavior.
  2. **Green:** Write the simplest possible code to make the test pass.
  3. **Refactor:** Clean up and improve the code while ensuring the test remains green.

### >> AI Copilot Directives: Behavior & Testing

- Given a Gherkin feature file and a scenario, your first task **MUST** be to generate the code for a **failing test** that implements that scenario.
- Once a test is provided, your task is to write the **minimum amount of implementation code required to make that test pass**.
- If asked to "implement a feature," your first response **SHOULD** be to ask for the Gherkin feature file or to help generate it. **AVOID** writing implementation code without a corresponding test.

---

## 4. Versioning & Collaboration Strategy: Building as a Team

This section governs *how* we work together and manage our codebase.

### Practices

- **Trunk-Based Development:** We adhere to a strict Trunk-Based Development model.
  - The `main` branch (the trunk) is always stable and deployable.
  - All work is done on short-lived feature branches that are created from `main`.
  - Feature branches **MUST** be merged back into `main` frequently (at least once per day).
  - This practice minimizes merge conflicts and enables continuous integration and delivery (CI/CD).

- **Conventional Commits:** All commit messages **MUST** follow the detailed guidelines
in our [Git Commit Rules](./git_commit_rules.md). This practice is essential for
maintaining a clear, automated, and understandable version history.

### AI Copilot Directives: Versioning & Collaboration

- All commit messages you generate **MUST** follow the specification in
[./git_commit_rules.md](./git_commit_rules.md).
- When asked to create a Pull Request (PR) description, **YOU MUST** provide a
clear summary of the changes, link to the relevant user story or Gherkin feature,
and describe how to test the changes.
- **AVOID** suggesting or creating long-lived feature branches. Your workflow should
always be: `git checkout -b new-feature main` -> `work` -> `commit` ->
`merge into main`.

---

## 5. Documentation Standards: Writing Quality Markdown

This section governs *how* we write and maintain documentation files across the project.

### Practices

- **Formatting and Linting:** All Markdown files **MUST** adhere to the comprehensive
guidelines outlined in our [Markdown Formatting Guidelines](./markdown_formatting.md).
This ensures consistency, readability, and maintainability across all project
documentation.

### AI Copilot Directives: Documentation Standards

- **YOU MUST** ensure all generated or modified markdown files pass all validation
checks defined in the [Markdown Formatting Guidelines](./markdown_formatting.md).
- Before completing any documentation task, **VERIFY** that the markdown follows
our established standards.

---

## 6. How to Use This Guide in AI Prompts

To ensure our AI copilots adhere to these principles, **start your prompts with the following boilerplate**. This boilerplate "primes" the AI with our engineering constitution.

### **Boilerplate for our AI Prompts:**

```Markdown
You are an expert software engineer for the "Current" project. Your work MUST strictly adhere to our established engineering principles.

**Key Principles to Enforce:**
- **Design:** SOLID, KISS, DRY.
- **Testing:** TDD is mandatory; tests are written before implementation. Gherkin is our source of truth.
- **Versioning:** Trunk-Based Development with Conventional Commits.
- **Documentation:** All markdown must pass linting (especially MD030). Use exactly 1 space after list markers.

Reference Document: `specs/engineering-principles-practices.md`

---

**My Request:**

[Your specific request here...]
```
