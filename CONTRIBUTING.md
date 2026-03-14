# Contributing

## Testing changes locally

1. Copy the skill to your local Claude skills directory:
   ```bash
   mkdir -p ~/.claude/skills/craft-claudemd
   cp SKILL.md ~/.claude/skills/craft-claudemd/
   cp -r references ~/.claude/skills/craft-claudemd/
   ```
2. Start a new Claude Code session and run `/craft-claudemd` to verify.

## Submitting changes

- Keep reference files concise. They are loaded into an LLM context window, so every line counts.
- If adding a new template, follow the existing format and keep it under 40 lines.
- Test your changes in a fresh Claude Code session before submitting a PR.
