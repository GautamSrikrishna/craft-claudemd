# CLAUDE.md Best Practices

## Table of Contents
- [The WHY/WHAT/HOW Framework](#the-whywhathow-framework)
- [What to Include](#what-to-include)
- [What to Exclude](#what-to-exclude)
- [Instruction Budget](#instruction-budget)
- [Anti-Patterns](#anti-patterns)
- [Progressive Disclosure](#progressive-disclosure)
- [Emphasis Rules](#emphasis-rules)
- [Maintenance](#maintenance)

---

## The WHY/WHAT/HOW Framework

Every instruction in CLAUDE.md should answer at least one of:

- **WHY**: Context Claude can't infer (architectural decisions, trade-offs, constraints)
  - Example: "We use server-side rendering because our user base has slow connections"
- **WHAT**: Non-obvious facts about the project (conventions, gotchas, invariants)
  - Example: "All database migrations must be backward-compatible (we do rolling deploys)"
- **HOW**: Commands and workflows Claude can't guess from code alone
  - Example: "Run tests with `make test-integration` (not `npm test`, which only runs unit tests)"

If an instruction doesn't fit any of these, it probably doesn't belong.

---

## What to Include

Include things Claude **cannot figure out** by reading the code:

- **Build/test/lint commands**, especially non-obvious ones (`make dev` vs `npm start`)
- **Non-default code style rules**: only those NOT enforced by linters/formatters
- **Testing instructions**: test runner, how to run a single test, special env setup
- **Repo etiquette**: PR conventions, branch naming, commit message format
- **Architectural decisions**: why things are structured a certain way
- **Dev environment quirks**: required env vars, local services, setup steps
- **Gotchas**: things that trip up new contributors, non-obvious behaviors
- **Critical invariants**: rules that if broken cause silent data corruption or outages

---

## What to Exclude

Exclude things Claude **can figure out** or that don't belong:

- **Standard conventions**: "use camelCase in JavaScript" (Claude already knows)
- **Things inferable from code**: file structure, import patterns, framework idioms
- **Detailed API docs**: link to them instead of reproducing them
- **Frequently changing info**: specific ticket numbers, current sprint goals
- **File-by-file descriptions**: Claude can read the files itself
- **Self-evident practices**: "write clean code", "handle errors", "add tests"
- **README content**: don't duplicate what's already in README.md
- **Tool descriptions**: don't describe what `git`, `npm`, or `python` do

---

## Instruction Budget

CLAUDE.md files are loaded into Claude's system prompt, which has finite capacity:

- **Total effective capacity**: ~150-200 instructions across all CLAUDE.md files
- **System prompt overhead**: ~50 instructions are used by Claude Code itself
- **"May or may not be relevant" caveat**: Claude treats CLAUDE.md content as potentially relevant context, not absolute commands, so instructions must earn their place
- **Diminishing returns**: After ~100 instructions, compliance drops as context competes

**Budget by location:**
| Location | Line Budget | Why |
|----------|-------------|-----|
| Home (~/.claude/CLAUDE.md) | <30 lines | Personal defaults, loaded everywhere |
| Project root (./CLAUDE.md) | <60 lines | Primary project instructions |
| Parent directory | <40 lines | Shared conventions for monorepo siblings |
| Child directory | <30 lines | Module-specific overrides |

These are total lines including headers and blank lines. Every line counts.

---

## Anti-Patterns

### 1. Verbose Tool Descriptions
Bad:
```
When using git, make sure to use `git add` to stage files, then `git commit -m "message"` to commit.
Use `git push` to push to the remote repository.
```
Claude already knows how git works. Only mention git workflows if yours are non-standard.

### 2. Restating README
Bad:
```
This project is a web application built with React and Node.js.
It uses PostgreSQL for the database and Redis for caching.
```
Claude can read your README and package.json. Don't duplicate.

### 3. Task-Specific Instructions in Root
Bad:
```
When working on the auth module, make sure to update the JWT expiry...
When fixing pagination bugs, check the cursor implementation...
```
These belong in child-directory CLAUDE.md files or not at all.

### 4. CLAUDE.md as Linter
Bad:
```
ALWAYS use semicolons. NEVER use var. ALWAYS use const over let.
Use 2-space indentation. Maximum line length is 100 characters.
```
Use ESLint/Prettier and hooks instead. Claude will respect formatter output.

### 5. Over-Emphasis
Bad:
```
IMPORTANT: YOU MUST ALWAYS use TypeScript.
CRITICAL: NEVER forget to add types.
YOU MUST ALWAYS write tests for every function.
```
When everything is emphasized, nothing is. Reserve emphasis for truly critical rules.

### 6. File-by-File Descriptions
Bad:
```
src/auth.ts - Contains authentication logic
src/db.ts - Contains database connection
src/routes/ - Contains API route handlers
```
Claude can explore the codebase. Only describe architecture that isn't obvious from structure.

---

## Progressive Disclosure

Use `@path` imports to keep the root CLAUDE.md lean while providing depth where needed:

```markdown
# Project CLAUDE.md (root)
@docs/claude/testing.md    # Detailed testing guide
@docs/claude/api.md        # API conventions
```

**When to use @imports:**
- When a topic needs >10 lines of detail
- When instructions are module-specific
- When content is only relevant some of the time

**Structure referenced files:**
- Start with a one-line summary of what the file covers
- Use headers to make content scannable
- Keep each referenced file focused on one topic
- Referenced files still count against the total instruction budget

---

## Emphasis Rules

- Use `IMPORTANT:` or `YOU MUST` sparingly. Maximum 2-3 per CLAUDE.md.
- Reserve emphasis for rules where violation causes **silent failures, data loss, or security issues**
- For everything else, explain the *why*. Claude responds better to reasoning than commands.
- If you find yourself emphasizing many rules, your CLAUDE.md is probably too long

Good emphasis:
```
IMPORTANT: Never commit .env files. They contain production secrets that rotate on exposure.
```

Bad emphasis:
```
IMPORTANT: Always use descriptive variable names.
```

---

## Maintenance

- **Prune regularly.** Review every 2-4 weeks, remove outdated instructions.
- **Test with fresh sessions.** Start a new Claude Code session and see if it follows your CLAUDE.md correctly.
- **Check into git.** CLAUDE.md is project configuration; treat it like code.
- **Remove instructions that became code.** If you added a linter rule or hook, remove the corresponding CLAUDE.md instruction.
- **Watch for drift.** If Claude consistently ignores an instruction, it's either redundant or poorly worded.
