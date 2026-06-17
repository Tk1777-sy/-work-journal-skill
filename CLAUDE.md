# CLAUDE.md — Development Guidelines for work-journal-skill

This file is for contributors working on the `work-journal` skill itself.

## What this repo is

A single-file Claude Code skill (`work-journal.md`) that generates structured work journals from AI coding sessions. Everything that matters lives in that one file.

## File structure

```
work-journal-skill/
├── work-journal.md        ← THE SKILL. This is what matters.
├── skill.json             ← Metadata (name, version, compatibility)
├── README.md              ← GitHub page
├── CONTRIBUTING.md        ← How to contribute
├── CLAUDE.md              ← This file
├── LICENSE                ← MIT
├── examples/
│   ├── developer-journal.md   ← Sample full journal output
│   ├── client-report.md       ← Sample client report output
│   └── resume.md              ← Sample RESUME.md output
└── .github/
    ├── ISSUE_TEMPLATE/
    │   ├── bug_report.md
    │   └── feature_request.md
    └── workflows/
        └── (CI if added later)
```

## How to edit the skill

`work-journal.md` is a prompt engineering file. It contains:

1. **Frontmatter** — `name` and `description` fields read by Claude Code
2. **Step 1** — The 4 questions to ask (interactive mode)
3. **Step 2** — Output file determination logic
4. **Step 3** — Full Developer Journal template (7 sections)
5. **Step 4** — SESSION RESUME template
6. **Step 5** — CLIENT REPORT template (3 audience variants)
7. **Step 6** — Git commit message generation
8. **Step 7** — Confirmation output to user

When editing, keep sections clearly numbered and titled. Claude follows the numbered steps in order.

## Testing changes

```bash
# Install to your project
cp work-journal.md /path/to/test-project/.claude/skills/

# Or install globally
cp work-journal.md ~/.claude/skills/

# Restart Claude Code
# Type /work-journal and go through all 4 modes
```

Test with:
- A short session (< 30 min of conversation)
- A long session with multiple bugs and decisions
- A session in Chinese to verify language detection
- Client report for all 3 audience types

## What makes a good prompt improvement

- More specific templates → more specific output
- Clearer section headers → Claude follows structure more reliably
- Concrete bad examples in the quality bar section → Claude avoids vague output
- Adding a new mode → follow the pattern of existing Step 3/4/5 blocks

## Version bumping

Update `skill.json` version field when making significant changes:
- Bug fix: `1.0.0` → `1.0.1`
- New section or improved template: `1.0.0` → `1.1.0`
- New mode or major redesign: `1.0.0` → `2.0.0`

Update the `## Changelog` section in README if adding one.
