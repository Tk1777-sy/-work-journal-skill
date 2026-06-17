<div align="center">

# 📓 work-journal

**The Claude Code skill that ends session amnesia.**

Generate structured developer logs, client reports, and session resumption briefs — with one command, in 30 seconds.

[![GitHub stars](https://img.shields.io/github/stars/Tk1777-sy/-work-journal-skill?style=for-the-badge&logo=github&color=FFD700)](https://github.com/Tk1777-sy/-work-journal-skill/stargazers)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-5A67D8?style=for-the-badge&logo=anthropic&logoColor=white)](https://claude.ai/code)
[![4 Modes](https://img.shields.io/badge/4%20Output%20Modes-10B981?style=for-the-badge)](#modes)
[![7 Journal Sections](https://img.shields.io/badge/7%20Journal%20Sections-6366F1?style=for-the-badge)](#full-developer-journal)
[![MIT License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](LICENSE)

[![Works with Cursor](https://img.shields.io/badge/Cursor-✓-black?style=flat-square)](https://cursor.sh)
[![Works with Windsurf](https://img.shields.io/badge/Windsurf-✓-0EA5E9?style=flat-square)](https://codeium.com/windsurf)
[![Works with Claude Code](https://img.shields.io/badge/Claude%20Code-✓-D97706?style=flat-square)](https://claude.ai/code)
[![Works with GitHub Copilot](https://img.shields.io/badge/GitHub%20Copilot-✓-24292E?style=flat-square)](https://github.com/features/copilot)
[![Works with Trae](https://img.shields.io/badge/Trae-✓-8B5CF6?style=flat-square)](https://trae.ai)
[![Language: EN/中文](https://img.shields.io/badge/Language-EN%20%2F%20中文-orange?style=flat-square)](#language)

</div>

---

## The problem every AI developer hits

```
Monday: great session. Fixed the auth bug, added the dashboard, robot now tracks mouse.
Tuesday: new Claude session. Blank slate.
You: "uh... so we have a Next.js project and..."
Claude: "Sure! What stack are you using?"
```

**git diff shows WHAT changed. It never shows WHY.**  
IDE timeline shows file history. It never shows your reasoning.  
There's no tool that snapshots a Claude session so tomorrow's Claude picks up where you left off.

Until now.

---

## What it does

Type `/work-journal` → Claude asks 4 questions → writes everything locally in under 30 seconds.

```
┌─────────────────────────────────────────────────────────┐
│                    /work-journal                         │
│                                                          │
│  ① What do you want to generate?                        │
│     › Full Developer Journal                             │
│       Client Report                                      │
│       Session Resume Only                                │
│       All Three                                          │
│                                                          │
│  ② Any decisions or tradeoffs to highlight?             │
│  ③ Any bugs or blockers to flag?                        │
│  ④ Who is the client report for? (if applicable)        │
│                                                          │
│  ─────────────────────────────────────────────────────  │
│  ✅ work-journal/2026-06-17.md          (developer log) │
│  ✅ RESUME.md                           (next session)  │
│  ✅ work-journal/2026-06-17-client.md   (client report) │
│                                                          │
│  📋 Suggested commit:                                    │
│  feat(landing): add GSAP portrait wall, fix Spline...   │
│                                                          │
│  Run git commit with this message? (Y/n)                 │
└─────────────────────────────────────────────────────────┘
```

---

## Modes

### 📋 Full Developer Journal
A permanent, 7-section technical record of everything that happened in your session.

| Section | What's inside |
|---------|--------------|
| **1. Overview** | Session arc — state before vs. after |
| **2. Requirements** | Every request, in the user's exact words |
| **3. Execution** | Tools used, commands run, files changed, approach reasoning |
| **4. Bugs & Solutions** | Each bug: root cause → fix → prevention |
| **5. File Change Log** | Created / Modified / Deleted table |
| **6. Final Output** | What the product looks and works like now |
| **7. Follow-up** | Must-do / should-do / nice-to-have checklists |

Plus: **Decision Log** (the part git can never show — *why* you chose this over alternatives) and a **git commit message** generated from the session.

---

### 🔄 Session Resume (`RESUME.md`)
The killer feature. Paste this file as your **first message in a new Claude session** and Claude has instant full context — no re-explaining, no lost decisions, no starting over.

```markdown
# Session Resume · 2026-06-17
> Paste this as your first message tomorrow.

## What exists right now
- ✅ Landing page (8 sections, Spline robot, GSAP scroll wall)
- ✅ Dashboard with collapsible sidebar
- ✅ DR lettermarks logo (never use diamond icon)

## Active decisions — do not undo
- Bebas Neue for hero title. Orbitron was rejected as "ugly"
- Spline overlays MUST have pointer-events-none or mouse breaks

## Next session: pick up here
- [ ] Delete deprecated eye-tracking-robot.tsx
- [ ] Set upper limit on brand counter
```

---

### 📊 Client Report
A non-technical summary written in plain language. Tone adapts based on your audience:

- **External client** → friendly, outcome-focused, zero jargon
- **Product manager** → feature-focused, light technical detail  
- **Investor / demo** → punchy, velocity-focused, highlights quality

---

### ⚡ All Three
One command. Everything above. Done.

---

## Installation

### Claude Code (global — works in all projects)

```bash
mkdir -p ~/.claude/skills
curl -o ~/.claude/skills/work-journal.md \
  https://raw.githubusercontent.com/Tk1777-sy/-work-journal-skill/main/work-journal.md
```

Restart Claude Code. Type `/work-journal` in any project.

### Claude Code (project-level)

```bash
mkdir -p .claude/skills
curl -o .claude/skills/work-journal.md \
  https://raw.githubusercontent.com/Tk1777-sy/-work-journal-skill/main/work-journal.md
```

### Cursor / Windsurf / Trae

Copy `work-journal.md` into your project's AI rules/instructions directory, or add the content to your system prompt / `.cursorrules`.

### Manual

Download [`work-journal.md`](work-journal.md) and place it in:
- Claude Code global: `~/.claude/skills/`
- Claude Code project: `.claude/skills/`
- Cursor: `.cursor/rules/`

---

## Usage

```bash
# In Claude Code chat
/work-journal
```

Or trigger with natural language:
```
我下班了
I'm done for the day
Generate today's journal
今天工作结束了，帮我写日志
End of session — write the journal
```

---

## Why not just use git log / IDE history?

| | `git log` | VSCode Timeline | work-journal |
|--|:---------:|:---------------:|:------------:|
| What files changed | ✅ | ✅ | ✅ |
| Why each change was made | ❌ | ❌ | ✅ |
| Original user requirements | ❌ | ❌ | ✅ |
| Bugs encountered + exact fixes | ❌ | ❌ | ✅ |
| Non-technical client summary | ❌ | ❌ | ✅ |
| Context snapshot for next AI session | ❌ | ❌ | ✅ |
| Suggested commit message | ❌ | ❌ | ✅ |
| Decision rationale ("why not X") | ❌ | ❌ | ✅ |

---

## Output file structure

```
your-project/
├── RESUME.md                          ← paste at start of next session
└── work-journal/
    ├── 2026-06-17.md                  ← full developer journal
    ├── 2026-06-17-client.md           ← client-facing report
    ├── 2026-06-18.md
    └── ...
```

---

## Example outputs

Real examples generated from a live coding session:

- [Full Developer Journal](examples/developer-journal.md) — 7-section technical log
- [Client Report](examples/client-report.md) — plain-language update for stakeholders  
- [Session Resume](examples/resume.md) — context brief for next Claude session

---

## Language support

Auto-detects your language from the conversation. Write in Chinese → journal in Chinese. Write in English → journal in English. Code, paths, and commit messages always stay in English.

---

## Requirements

- Any AI coding assistant (Claude Code, Cursor, Windsurf, Trae, GitHub Copilot)
- No API keys
- No packages to install
- No configuration files

---

## Add to .gitignore (optional)

If you don't want work journals in version control:
```gitignore
# Work journals (optional — some teams commit these)
work-journal/
RESUME.md
```

Or commit them — they make great async team documentation.

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) — feature requests and new mode ideas very welcome.

---

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=Tk1777-sy/-work-journal-skill&type=Date)](https://star-history.com/#Tk1777-sy/-work-journal-skill&Date)

---

## License

MIT — use it, fork it, build on it.

---

<div align="center">
  <sub>If this saved you from re-explaining your project for the 10th time — ⭐ would mean a lot.</sub>
</div>
