# craft-claudemd

A Claude Code skill that generates or updates highly effective CLAUDE.md files following research-backed best practices.

## What it does

- **Creates** new CLAUDE.md files by auto-detecting your project's tools, frameworks, and conventions
- **Updates** existing CLAUDE.md files by diagnosing bloat, redundancy, and anti-patterns
- Works at any level: home (`~/.claude/CLAUDE.md`), project root, parent, or child directory
- Enforces line budgets, cuts what Claude can figure out on its own, and suggests `@imports` for progressive disclosure

## Installation

### Global install (recommended - available in all projects)

```bash
git clone https://github.com/GautamSrikrishna/craft-claudemd.git
mkdir -p ~/.claude/skills/craft-claudemd
cp -r craft-claudemd/SKILL.md craft-claudemd/references ~/.claude/skills/craft-claudemd/
```

### Project-level install (available only in one project)

```bash
git clone https://github.com/GautamSrikrishna/craft-claudemd.git
mkdir -p /path/to/your/project/.claude/skills/craft-claudemd
cp -r craft-claudemd/SKILL.md craft-claudemd/references /path/to/your/project/.claude/skills/craft-claudemd/
```

### Verify installation

Start a new Claude Code session and run:

```
/craft-claudemd
```

You should see Claude begin the CLAUDE.md creation workflow.

## Usage

The skill triggers automatically when you ask Claude to:

- "Create a CLAUDE.md for this project"
- "Improve my CLAUDE.md"
- "My CLAUDE.md is too long"
- "Set up Claude Code for this repo"

Or invoke it manually with `/craft-claudemd`.

## What's included

```
craft-claudemd/
├── SKILL.md                    # Core workflow (CREATE/UPDATE modes)
└── references/
    ├── best-practices.md       # Do's, don'ts, anti-patterns, instruction budget
    ├── location-guide.md       # What belongs at each CLAUDE.md location
    └── templates.md            # Starter templates by project type
```

## Uninstall

For a global install:
```bash
rm -rf ~/.claude/skills/craft-claudemd
```

For a project-level install:
```bash
rm -rf /path/to/your/project/.claude/skills/craft-claudemd
```
