# CLAUDE.md Starter Templates

Use these as starting points. Always customize based on auto-detected project context.
Remove sections that don't apply. Add project-specific gotchas.

---

## Python Project (pyproject.toml / requirements.txt)

```markdown
# Build & Run
pip install -e ".[dev]"   # install with dev dependencies
python -m pytest           # run all tests
python -m pytest tests/test_foo.py::test_bar  # run single test
python -m mypy .           # type check

# Architecture
<brief description of project structure and key design decisions>

# Conventions
- Type hints required on all public functions
- Use `pathlib.Path` over `os.path`
- Async functions suffixed with `_async`

# Gotchas
- <project-specific gotchas>
```

---

## Node/TypeScript Project (package.json / tsconfig.json)

```markdown
# Build & Test
npm run build              # compile TypeScript
npm test                   # run all tests
npm test -- --grep "name"  # run single test
npm run lint               # lint with ESLint
npm run lint:fix           # auto-fix lint issues

# Architecture
<brief description of project structure and key design decisions>

# Conventions
- Exports: follow pattern in src/utils/index.ts (named exports, no defaults)
- API responses: follow envelope shape in src/types/api.ts `{ data, error }`
- Database queries go through the repository layer, never direct SQL in handlers

# Gotchas
- <project-specific gotchas>
```

---

## Go Project (go.mod)

```markdown
# Build & Test
go build ./...                    # build all packages
go test ./...                     # run all tests
go test ./pkg/foo -run TestBar    # run single test
go vet ./...                      # static analysis
golangci-lint run                 # lint

# Architecture
<brief description of project structure and key design decisions>

# Conventions
- Errors are returned, not panicked. Only use `panic` in `init()` or truly unrecoverable cases.
- Context is always the first parameter
- Interface definitions live in the consuming package, not the implementing one

# Gotchas
- <project-specific gotchas>
```

---

## Rust Project (Cargo.toml)

```markdown
# Build & Test
cargo build                       # debug build
cargo test                        # run all tests
cargo test test_name              # run single test
cargo clippy -- -D warnings       # lint
cargo fmt --check                 # check formatting

# Architecture
<brief description of project structure and key design decisions>

# Conventions
- Use `thiserror` for library errors, `anyhow` for application errors
- Prefer `&str` over `String` in function parameters
- All public items have doc comments

# Gotchas
- <project-specific gotchas>
```

---

## Generic / Polyglot Project

```markdown
# Build & Test
<primary build command>
<primary test command>
<how to run a single test>
<lint/format command>

# Architecture
<brief description of project structure, especially non-obvious parts>

# Conventions
<2-5 non-obvious conventions that differ from standard practices>

# Gotchas
<things that break silently or confuse newcomers>
```

---

## Monorepo Root

```markdown
# Workspace
<workspace tool and how to use it, e.g. npm workspaces, turborepo, bazel>

# Shared Conventions
<conventions that apply across all packages>

# Package Structure
<brief map of packages and their relationships, only non-obvious structure>

# CI
<how CI works, especially if it's not standard>
```

---

## Home CLAUDE.md (`~/.claude/CLAUDE.md`)

```markdown
# Preferences
- Be concise in explanations
- Use early returns over nested conditionals
- Prefer composition over inheritance

# Git
- Commit messages: imperative mood, <72 chars, no period
- Always create feature branches, never commit to main

# Testing
- Run tests after making changes
- Prefer integration tests over mocking
```

Note: Home CLAUDE.md should be <30 lines. Only include preferences that apply to ALL your projects.

---

## Child/Subsystem CLAUDE.md

```markdown
# <Subsystem Name>

## Purpose
<one-line: what this subsystem does and its boundary within the larger project>

## Entry Points
- <primary file or function where execution enters this subsystem>
- <secondary entry point, if any>

## Patterns
- Request validation: follow pattern in handlers/users.ts
- Error handling: follow pattern in handlers/errors.ts
- <other key patterns, pointing to canonical files>

## Anti-Patterns
- Do NOT call <other subsystem> directly; use the event bus
- <other things that seem reasonable but are wrong here>

## Invariants
- <rules that must hold true or things break silently>

## Pitfalls
- <non-obvious things that have caused bugs before>
```

Use this for `packages/*/CLAUDE.md`, `src/subsystem/CLAUDE.md`, or any directory that has its own conventions. Stay under 30 lines.

---

## Tips for All Templates

1. **Delete sections you don't need.** Empty sections are worse than no sections.
2. **Replace placeholders.** Every `<angle bracket>` item must be filled in or removed.
3. **Add your gotchas.** Templates can't know your project's unique pain points.
4. **Stay under budget.** Count your lines against the location budget in the location guide.
5. **Test it.** Start a fresh Claude Code session and see if it follows your instructions.
6. **Point to files, not prose.** For code style and pattern rules, reference a canonical file instead of describing the convention in words. Pointers stay accurate as code evolves; prose rots.
