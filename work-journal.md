---
name: work-journal
description: Interactive daily work journal for Claude Code. Generates developer logs, client reports, session resumption briefs, and git commit messages. Asks what you need before writing anything.
---

You are an expert technical writer and session analyst. When this skill is invoked, follow the exact steps below.

---

## STEP 1 — Ask clarifying questions before writing anything

Use the `AskUserQuestion` tool to ask the following questions. Do not generate any journal content until you have the answers.

Ask these questions (all in one call, using multiSelect where noted):

**Question 1 — Mode**
> What do you want to generate today?

Options:
- `Full Developer Journal` — 7-section detailed log + session resume + commit message (for yourself, next-day context)
- `Client Report` — Non-technical summary of what was accomplished (for clients, managers, or stakeholders)
- `Quick Resume Only` — Just a RESUME.md context snapshot to paste into tomorrow's Claude session
- `All Three` — Generate full journal + client report + resume brief

**Question 2 — Highlights** (free text / Other)
> Any specific decisions, tradeoffs, or breakthroughs you want highlighted in the journal?
Options:
- `No, just summarize the session`
- `Yes — I'll describe them` (use Other field)

**Question 3 — Known issues** (free text / Other)
> Any bugs or incomplete items to explicitly flag in the follow-up section?
Options:
- `None that I can think of`
- `Yes — I'll list them` (use Other field)

**Question 4 — Audience (only for Client Report or All Three)**
> Who is the client report for?
Options:
- `Direct client / external stakeholder`
- `Product manager / internal team`
- `Investor / demo audience`

---

## STEP 2 — Determine output files

Based on the chosen mode, you will write one or more files. Always use today's actual date (YYYY-MM-DD) for filenames.

Create the `work-journal/` directory if it doesn't exist:
```bash
mkdir -p work-journal
```

| Mode | Files to write |
|------|---------------|
| Full Developer Journal | `work-journal/YYYY-MM-DD.md` + `RESUME.md` (project root) |
| Client Report | `work-journal/YYYY-MM-DD-client.md` |
| Quick Resume Only | `RESUME.md` (project root, overwrite) |
| All Three | All of the above |

---

## STEP 3 — FULL DEVELOPER JOURNAL template

Write this when mode is `Full Developer Journal` or `All Three`.

```markdown
# Work Journal · YYYY-MM-DD

**Project:** [name + one-line description from context]
**Stack:** [all tech used in this session]
**Session length:** [approximate — short/medium/long]

---

## 1. Overview

[3–5 sentences. What problem was being solved? What was the state at start vs end of session?
Make it dense — someone reading this cold should immediately understand the arc of the day.]

---

## 2. Requirements & Requests

[For every request the user made, write a block:]

### [Short title]

**User's exact words:**
> "[verbatim quote from the conversation, in their original language]"

**Desired outcome:** [what they wanted to see/feel/use]

**Reference material sent:** [code snippets, images, URLs, or "None"]

---

## 3. Execution — Methods & Tools

[For each meaningful task:]

### [Task title]

**Approach:** [why this method, what library/pattern, any alternatives considered and why rejected]

**Commands run:**
\`\`\`bash
[exact shell commands]
\`\`\`

**Files created:**
- `path/to/new-file.tsx` — [one-line description]

**Files modified:**
- `path/to/changed-file.tsx` — [what changed and why]

**Files deleted / deprecated:**
- `path/to/old-file.tsx` — [why it was removed]

**Key code change:**
\`\`\`[language]
// Before
[old code or "N/A — new file"]

// After
[new code]
\`\`\`

**Decision log:** [If a non-obvious choice was made — why THIS approach over alternatives. This is the part git can never show.]

---

## 4. Bugs & Solutions

| Bug | Root cause | Fix applied | Prevention |
|-----|-----------|------------|------------|
| [what broke] | [why] | [exact fix] | [how to avoid next time] |

---

## 5. File Change Log

| Action | Path | Notes |
|--------|------|-------|
| Created | `src/...` | |
| Modified | `src/...` | |
| Deleted | `src/...` | |

---

## 6. Final Output

**What works now:**
[Describe the user-facing result. What can someone see, click, use?
Be concrete — mention specific UI, interactions, data flows.]

**What it looks like:**
[Describe the visual/functional state if relevant]

**Performance / quality notes:**
[Any regressions, improvements, or things to monitor]

---

## 7. Follow-up & Known Issues

### Must do next session
- [ ] [critical item]

### Should do soon
- [ ] [important item]

### Nice to have
- [ ] [optional improvement]

### Known limitations
- [ ] [technical debt or accepted tradeoff]

---

## Suggested git commit message

\`\`\`
[type]([scope]): [concise summary under 72 chars]

[Body: 2–4 lines explaining WHY, not what.
Reference any issue numbers if applicable.]
\`\`\`

---

*Generated: YYYY-MM-DD · work-journal Claude Code skill · github.com/[author]/work-journal-skill*
```

---

## STEP 4 — SESSION RESUME template

Write this to `RESUME.md` in the project root. This file is designed to be copy-pasted as the **first message** of tomorrow's Claude Code session to restore full context instantly.

```markdown
# Session Resume · YYYY-MM-DD

> Paste this entire file as your first message in a new Claude Code session to restore full context.

---

## Project

**Name:** [project name]
**Type:** [web app / CLI / API / library / etc.]
**Stack:** [key technologies]
**Repo:** [path or URL]

## What exists right now

[Bullet list of major completed features/components. Present tense. What Claude should know is already built and working.]

- ✅ [feature 1]
- ✅ [feature 2]
- ✅ [feature 3]

## Current state (as of YYYY-MM-DD)

[2–3 sentences describing where the project stands — what's stable, what's in progress, what's broken]

## Key files to know

| File | Purpose |
|------|---------|
| `src/...` | [what it does] |
| `src/...` | [what it does] |

## Ongoing decisions & constraints

[Things Claude must know to not undo previous decisions]

- We use [X] instead of [Y] because [reason]
- [Component] is intentionally [behavior] due to [constraint]

## Next session: pick up here

- [ ] [First thing to tackle]
- [ ] [Second thing]
- [ ] [Third thing]

## Known issues (do not re-investigate, already diagnosed)

- [issue] → [diagnosis] — fix is [approach]

---

*Resume file generated by work-journal skill · Update each session*
```

---

## STEP 5 — CLIENT REPORT template

Write this when mode is `Client Report` or `All Three`. Adjust tone based on the audience answer.

For `Direct client / external stakeholder` — friendly, outcome-focused, no jargon  
For `Product manager / internal team` — concise, feature-focused, light technical detail  
For `Investor / demo audience` — punchy, progress-focused, highlight velocity and quality

```markdown
# Project Update · YYYY-MM-DD

**Project:** [name]
**Prepared by:** [infer from context or leave as "Development Team"]

---

## What we completed today

[3–6 bullet points. Each one = one user-visible outcome. No technical terms.
Bad: "Refactored the BackgroundPaths component to increase strokeOpacity"
Good: "Added flowing animated lines to the homepage background — gives the site a modern, dynamic feel"]

- ✅ [outcome 1]
- ✅ [outcome 2]
- ✅ [outcome 3]

## What it looks like now

[1–2 sentences describing the current state of the product from a user's perspective]

## What's coming next

- 🔜 [next milestone]
- 🔜 [next milestone]

## Notes

[Any decisions that required client input, or items that may come up in review]

---

*Prepared: YYYY-MM-DD*
```

---

## STEP 6 — Generate git commit message

At the end of every mode (except Quick Resume Only), suggest a git commit message using this format:

```
[conventional commit type]([scope]): [what changed, under 72 chars]

- [bullet: key change 1]  
- [bullet: key change 2]
- [bullet: key change 3]

[Optional: why this change was made, or what problem it solves]
```

Types: `feat` `fix` `refactor` `style` `docs` `chore` `perf`

Then ask:
> Want me to run `git add -A && git commit -m "..."` with this message? (Confirm before executing — never commit without user approval.)

---

## STEP 7 — Confirm output

After writing all files, tell the user:

1. Which files were written and their paths
2. The suggested git commit message (formatted as a code block)
3. Any items in the "must do next session" list
4. If RESUME.md was generated: remind them to paste it at the start of tomorrow's session

---

## Language rule

Match the user's primary language from the conversation. Code, paths, and commit messages always stay in English. Narrative sections follow the user's language.

---

## Quality standard

Every section must be specific enough that:
- A developer who wasn't in the session can understand exactly what changed and why
- You could hand this to a new team member as an onboarding doc for today's work
- Your future self can read this in 6 months and understand every decision

Vague entries like "fixed some bugs" or "improved performance" are not acceptable. Name the bug, name the file, name the fix.
