# CLAUDE.md Location Guide

## Location Overview

| Location | Path | Line Budget | Content Focus |
|----------|------|-------------|---------------|
| **Home** | `~/.claude/CLAUDE.md` | <30 lines | Personal defaults, global preferences |
| **Project root** | `./CLAUDE.md` | <60 lines | Build/test/lint, architecture, gotchas |
| **Parent directory** | `../CLAUDE.md` | <40 lines | Shared conventions across monorepo siblings |
| **Child directory** | `./module/CLAUDE.md` | <30 lines | Module-specific overrides |

---

## Home CLAUDE.md (`~/.claude/CLAUDE.md`)

**Loaded in every project.** Keep it minimal and universally applicable.

**Good content:**
- Preferred language/framework defaults
- Communication style preferences (concise vs verbose, emoji usage)
- Git commit message format you prefer across all projects
- Personal coding conventions (e.g., "I prefer early returns")
- Default behavior preferences (e.g., "always run tests after changes")

**Bad content:**
- Project-specific instructions (use project root instead)
- Anything that varies by project
- Instructions longer than a sentence (these aren't universally important)

---

## Project Root CLAUDE.md (`./CLAUDE.md`)

**The primary location.** Most projects only need this one file.

**Good content:**
- Build, test, lint commands, especially non-obvious ones
- Architecture overview: only high-level, non-obvious structure
- Key conventions: things Claude can't infer from code/config
- Dev environment setup: required services, env vars
- Gotchas: things that break silently or confuse newcomers
- Critical invariants: rules where violation causes real harm

**Structure suggestion:**
```markdown
# Build & Test
<commands>

# Architecture
<1-3 sentences>

# Conventions
<non-obvious rules>

# Gotchas
<things that trip people up>
```

---

## Parent Directory CLAUDE.md (`../CLAUDE.md`)

**For monorepos.** Conventions shared across sibling projects/packages.

**Good content:**
- Shared coding standards across packages
- Monorepo-wide tooling commands (workspace commands, shared scripts)
- Cross-package dependency rules
- Shared CI/CD conventions

**When to use:**
- You have a monorepo with 2+ packages that share conventions
- Instructions would otherwise be duplicated across sibling CLAUDE.md files

---

## Child Directory CLAUDE.md (`./module/CLAUDE.md`)

**For module-specific overrides.** Use sparingly; prefer `@path` imports from root.

**Good content:**
- Module-specific test commands
- Module-specific conventions that differ from root
- Domain-specific gotchas

**When to use:**
- The module has genuinely different conventions from the rest of the project
- There are module-specific gotchas worth documenting

**Prefer @path imports instead** when the content supplements (rather than overrides) root conventions.

---

## How Levels Interact

1. **All levels merge.** Claude sees content from all applicable CLAUDE.md files simultaneously.
2. **No explicit override mechanism.** If instructions conflict, Claude uses judgment (closer-to-code usually wins).
3. **Budget is shared.** Total instructions across ALL levels should stay under ~150-200.
4. **Closer = more specific.** Use home for universal preferences, child for narrow overrides.
5. **Don't repeat across levels.** If it's in root, don't also put it in child directories.
