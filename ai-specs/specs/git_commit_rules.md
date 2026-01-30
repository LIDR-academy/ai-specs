# Git Commit Rules and Guidelines

## Commit Types

Always prefix your commit messages with one of these types:

- **feature:** New feature added to the application or regular code maintenance
- **bugfix:** A patch for a bug fix
- **hotfix:** Refactoring a specific section of the codebase or urgent changes applied directly to production
- **style:** Feature or style-related updates (UI/UX changes, formatting, etc.)
- **docs:** Anything related to documentation (README, comments, etc.)

## Commit Message Structure

### Format Template

```text
<type>: <Title in imperative mood>

<Body paragraph explaining what and why>

<Additional context if needed>
```

### Rules for Title (First Line)

1. **Separate title from body** with a blank line
2. **No whitespace errors** - ensure clean formatting
3. **Remove unnecessary punctuation** - keep it clean
4. **Don't end with a period** - titles don't need periods
5. **Capitalize first letter** of the title
6. **Use imperative mood** - "Add feature" not "Added feature" or "Adding feature"
7. **Keep title under 50 characters** for better readability

### Rules for Body (Optional but Recommended)

1. **Capitalize first letter** of each paragraph
2. **Explain what and why** - not just what changed
3. **Don't assume reviewers know the context** - provide necessary background
4. **Don't assume code is self-explanatory** - explain complex logic
5. **Wrap lines at 72 characters** for better readability

## Examples

### Good Commit Messages

```text
feature: Add sensor data validation pipeline

Implement comprehensive validation for incoming sensor readings
from DXM devices. This prevents invalid data from being stored
in QuestDB and improves overall data quality.

The validation includes:
- Range checking for sensor values
- Timestamp validation
- Device ID format verification
```

```text
bugfix: Fix memory leak in data processing goroutine

Resolve goroutine leak that occurred when processing channel
was closed unexpectedly. Added proper cleanup using defer
and context cancellation.

This fixes the memory consumption issue reported in production
where the application would gradually consume more RAM over time.
```

```text
docs: Update API endpoint documentation

Add comprehensive examples for all REST endpoints including
request/response formats and error codes. This addresses
confusion around the /config/device endpoint usage.
```

```text
hotfix: Patch critical database connection timeout

Increase connection timeout from 5s to 30s to handle
network latency issues in production environment.
Applied directly to production due to service downtime.
```

```text
style: Improve device configuration UI layout

Reorganize form elements for better user experience
and add responsive design for mobile devices.
Updated color scheme to match corporate branding.
```

### Bad Commit Messages (Avoid These)

```
❌ Fixed bug.
❌ feature: added new stuff
❌ Update README.md.
❌ WIP: working on sensor parser
❌ Refactored code
❌ feature: Adding validation for sensors and also fixed some bugs
```

## Pre-Commit Checklist

Before committing, ensure:

- [ ] Commit type is specified and correct
- [ ] Title uses imperative mood
- [ ] Title is capitalized and under 50 characters
- [ ] No period at end of title
- [ ] Body explains what and why (if needed)
- [ ] No whitespace errors
- [ ] Code is tested and working
- [ ] Documentation is updated if needed

## Integration with Development Workflow

### For Features

```text
feature: Implement real-time data streaming

Add WebSocket support for real-time sensor data streaming
to dashboard clients. This enables live monitoring of
industrial equipment without page refreshes.

Includes connection management, authentication, and
automatic reconnection handling.
```

### For Bug Fixes

```text
bugfix: Resolve QuestDB connection pool exhaustion

Fix issue where database connections were not being
properly returned to the pool after failed queries.
Added proper error handling and connection cleanup.

This resolves the "too many connections" error that
occurred during high-load scenarios.
```

### For Documentation

```text
docs: Add deployment guide for Docker environment

Create comprehensive guide covering:
- Docker Compose setup
- Environment variable configuration  
- Database initialization steps
- Troubleshooting common issues

This addresses frequent deployment questions from the team.
```

## Tools and Automation

Consider using:

- Git hooks to enforce commit message format
- Conventional Commits specification
- Automated changelog generation
- Commit message linting tools