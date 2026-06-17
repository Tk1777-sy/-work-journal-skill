# Contributing to work-journal

Thanks for wanting to improve this skill. Here's how.

## Ways to contribute

- **Bug reports** — something generates wrong output, a template section is missing, etc.
- **New templates** — a new output mode that would be useful (e.g. PR description generator, standup notes)
- **Language improvements** — better prompting that produces more accurate/useful output
- **Translations** — the README in other languages

## How to submit changes

1. Fork the repo
2. Edit `work-journal.md` — the entire skill lives in this one file
3. Test it in Claude Code by placing your edited file in `.claude/skills/`
4. Open a PR with a clear description of what changed and why

## Testing your changes

```bash
# Copy to your project's skill directory
cp work-journal.md /path/to/your-project/.claude/skills/

# Or install globally
cp work-journal.md ~/.claude/skills/

# Restart Claude Code, then test with:
# /work-journal
```

Include a before/after example of what your change improves in the PR description.

## What makes a good contribution

- Changes should make the output **more specific and useful**, not more generic
- New modes should solve a real problem that comes up in real dev sessions
- Prompts should produce consistent output across different types of projects

## Reporting bugs

Open an issue with:
- What you typed / what mode you selected
- What Claude generated
- What you expected instead
- Your Claude Code version (run `claude --version`)
