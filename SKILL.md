---
name: craft-claudemd
description: >
  Create or update CLAUDE.md files following research-backed best practices.
  Use when asked to: create a new CLAUDE.md, improve/update an existing CLAUDE.md,
  set up Claude Code for a project, write project instructions for Claude,
  or generate a CLAUDE.md at any location (home, project root, parent, child directory).
  Triggers on: "make a CLAUDE.md", "improve my CLAUDE.md", "set up Claude Code for this project",
  "create project instructions", "optimize my CLAUDE.md", "CLAUDE.md is too long",
  "what should go in CLAUDE.md", "help me write a CLAUDE.md", "craft a CLAUDE.md",
  "initialize Claude Code for this repo", "configure Claude for my project".
  Also trigger when users mention CLAUDE.md quality, bloat, or effectiveness issues.
---

# Craft CLAUDE.md

Generate or update highly effective CLAUDE.md files that make Claude Code maximally useful.

## Core Principle

CLAUDE.md is **onboarding for a stateless agent**, not documentation. Every line must answer: "Would a senior engineer who can read the code still need to be told this?" If no, cut it.

## Workflow

### Step 1: Determine Mode

- **Target file already exists?** → UPDATE mode
- **No file at target?** → CREATE mode
- **Ambiguous target?** → Ask the user. Default to `./CLAUDE.md` (project root)

### Step 2: Determine Location

Read `references/location-guide.md` to understand what belongs at each level:
- `~/.claude/CLAUDE.md`: personal defaults (<30 lines)
- `./CLAUDE.md`: project root (<60 lines)
- Parent directory: monorepo shared conventions (<40 lines)
- Child directory: module-specific overrides (<30 lines)

The location determines scope, line budget, and content focus.

---

## CREATE Mode

### 2a. Auto-Detect Project Context

Scan the project for signals. Read files as needed, don't guess:

| Signal | What to look for |
|--------|-----------------|
| **Package manager** | `package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `Gemfile`, `pom.xml` |
| **Build system** | `Makefile`, `Justfile`, `build.gradle`, `CMakeLists.txt`, `Taskfile.yml` |
| **Config files** | `tsconfig.json`, `.eslintrc*`, `.prettierrc*`, `ruff.toml`, `clippy.toml` |
| **Test framework** | `jest.config.*`, `pytest.ini`, `setup.cfg [tool:pytest]`, `.mocharc.*` |
| **CI/CD** | `.github/workflows/`, `.gitlab-ci.yml`, `Jenkinsfile` |
| **Monorepo** | `packages/`, `apps/`, workspace config in package.json, `pnpm-workspace.yaml` |
| **Existing docs** | `README.md`, `CONTRIBUTING.md`, `docs/` |
| **Git conventions** | Recent commit messages (`git log --oneline -20`), branch naming patterns |
| **Existing .claude/** | `.claude/` directory, existing CLAUDE.md files at other levels |

Read `README.md` if it exists. It often contains build/test commands and architecture notes.

### 2b. Interview the User

Ask 2-4 targeted questions about things you **could not auto-detect**:

- "Are there architectural decisions or trade-offs I should know about?"
- "Any gotchas or things that trip up new contributors?"
- "Any non-obvious commands or workflows?" (only if Makefile/scripts don't make them clear)
- For home CLAUDE.md: "What coding preferences do you want applied to all projects?"

Do NOT ask about things you already detected. Keep questions specific and actionable.

### 2c. Generate the CLAUDE.md

1. Select the appropriate template from `references/templates.md` based on detected project type
2. Fill in auto-detected commands and conventions
3. Incorporate user answers from the interview
4. Apply constraints from `references/best-practices.md`:
   - Stay under the line budget for the target location
   - Only include what Claude can't figure out from code
   - Every line must be universally applicable (no task-specific instructions)
   - Use IMPORTANT/MUST sparingly (max 2-3 per file)
   - Don't describe tools Claude already knows
   - Don't duplicate README content
   - For code style/pattern rules, point to canonical files instead of prose descriptions
5. If content exceeds the line budget, use `@path` imports for progressive disclosure
6. Present the draft to the user for review
7. Write the file only after user approval

---

## UPDATE Mode

### 2a. Diagnose the Existing File

Read the existing CLAUDE.md and evaluate against `references/best-practices.md`:

**Check for these problems:**
- **Too long**: exceeds line budget for its location
- **Redundant**: duplicates README, states obvious conventions, describes tools Claude knows
- **Missing critical sections**: no build/test commands, no architecture context
- **Task-specific instructions**: contains instructions for specific features or tickets
- **Linter-as-CLAUDE.md**: style rules that should be in a formatter/linter config
- **Over-emphasized**: too many IMPORTANT/MUST/ALWAYS/NEVER markers
- **Stale**: references files, tools, or conventions that no longer exist
- **File-by-file descriptions**: describes what each file does (Claude can read them)
- **Generic/template content**: instructions copied from a template that don't match the actual project
- **History not guidance**: instructions phrased as past events ("we migrated to X") instead of forward-looking rules ("use X for all new work")

### 2b. Auto-Detect Context Changes

Scan the project (same signals as CREATE mode) to find:
- New tools or frameworks added since the CLAUDE.md was written
- Commands that have changed
- Conventions that are now enforced by tooling (and can be removed from CLAUDE.md)

### 2c. Propose Changes

Present **surgical changes** with rationale for each:
- What to remove and why (e.g., "Remove ESLint style rules, these are enforced by your .eslintrc")
- What to add and why (e.g., "Add `make integration-test`, detected in Makefile but not documented")
- What to restructure and why (e.g., "Move auth-module instructions to `src/auth/CLAUDE.md`")
- Whether to introduce `@path` imports for progressive disclosure

### 2d. Apply After Approval

Only modify the file after the user reviews and approves the changes.

---

## Key Principles (Always Apply)

1. **CLAUDE.md is onboarding for a stateless agent, not documentation.** Claude reads it fresh every session.
2. **Only include what Claude cannot figure out from code.** If it's in package.json, tsconfig.json, or the code itself, don't repeat it.
3. **Every line must be universally applicable.** No task-specific, ticket-specific, or sprint-specific content.
4. **Concise > comprehensive.** A 30-line CLAUDE.md that's followed beats a 200-line one that's ignored.
5. **Use hooks and formatters instead of linting instructions.** Claude respects tool output; don't manually enforce style.
6. **Use @path imports for progressive disclosure.** Keep root lean, add depth through references.
7. **Maintain with the litmus test: "Would this mislead an agent?"** When code changes, ask if an outdated CLAUDE.md instruction would cause Claude to make a mistake. Update when contracts/boundaries change; skip when only implementation details change.

---

## Reference Files

Read these as needed during the workflow:

- `references/best-practices.md`: do's, don'ts, anti-patterns, and the instruction budget framework
- `references/location-guide.md`: what content belongs at each CLAUDE.md location and how levels interact
- `references/templates.md`: starter templates by project type (Python, Node/TS, Go, Rust, generic, home)

---

## Output Format

The generated CLAUDE.md should:
- Use `#` headers to organize sections (Build & Test, Architecture, Conventions, Gotchas)
- Use code blocks for commands
- Use bullet points for rules and conventions
- Include blank lines between sections for readability
- NOT include meta-commentary ("This file was generated by...")
- NOT include the line budget or other meta-instructions in the output
