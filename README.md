# claude-code-bootstrap

**Stop vibe-coding. Start spec-coding.**

Two drop-in `CLAUDE.md` wizards that turn Claude Code into a structured brainstorm partner — and output the 5 spec files your AI agent actually needs to build production software.

> 97.5% of AI agents fail on real-world freelance projects.
> The difference? Structure. — [Remote Labor Index, Scale AI](https://www.remotelabor.ai/paper.pdf)

---

## The Problem

You open Claude Code. You type "build me an app." You get… something. It looks right. It doesn't work.

Or it works, but:
- No tests
- No clear architecture
- No way to verify what was built
- Next session, Claude forgets everything and starts over

The missing piece isn't better AI. It's **better specs**.

## The Solution

Two wizard files that guide Claude Code through a **structured brainstorm** with you before writing a single line of code. The output is 5 files that give any AI agent the context it needs:

| File | Purpose |
|------|---------|
| `CLAUDE.md` | Agent rules — stack, commands, style, workflow (root) |
| `ai/PRD.md` | What we're building and what we're NOT |
| `ai/ARCHITECTURE.md` | How the code is organized |
| `ai/AI_RULES.md` | Conventions, testing, git, anti-patterns |
| `ai/PLAN.md` | Step-by-step implementation with verification |

> **Why `ai/`?** CLAUDE.md must live in the root (Claude Code reads it from there). The other 4 files go in `ai/` to keep your project root clean. CLAUDE.md references them via `@ai/PRD.md` syntax — Claude loads them on demand, without bloating every conversation.

## Two Wizards

### 🚀 `CLAUDE-WIZARD.md` — New Project

For when you're starting from scratch. Drop it in an empty folder, open Claude Code, and it walks you through:

```
Phase 0: Language selection (brainstorm in any language)
Phase 1: Vision — what are we building and for whom?
Phase 2: Stack — which technologies and why?
Phase 3: Rules — coding style, testing, git workflow
Phase 4: Architecture — folder structure, data model, API
Phase 5: Plan — phased roadmap with verification commands
Phase 6: Generate CLAUDE.md (root) + ai/ spec files
```

### 🔍 `CLAUDE-ONBOARDING.md` — Existing Project

For when you have a codebase but never used Claude Code on it. Drop it in your project root and it:

1. **Silently analyzes** your codebase (package.json, folder tree, configs, schema, CI/CD, git history)
2. **Presents what it found** and asks "did I get this right?"
3. **Asks only what the code can't tell it** — the why, the direction, the scope
4. **Generates spec files that match reality** — not aspirational, descriptive

```
Phase 0: Automatic discovery + language selection
Phase 1: Vision — why does this project exist?
Phase 2: Stack confirmation — "I found Next.js 14, is that right?"
Phase 3: Architecture mapping — documents what IS, not what should be
Phase 4: Rules — what should Claude always/never do?
Phase 5: Plan — next steps, tech debt, new features
Phase 6: Generate CLAUDE.md (root) + ai/ spec files
```

## Quick Start

### New project

```bash
mkdir my-new-project && cd my-new-project
curl -O https://raw.githubusercontent.com/tommasinigiovanni/claude-code-bootstrap/main/CLAUDE-WIZARD.md
mv CLAUDE-WIZARD.md CLAUDE.md
claude
```

### Existing project

```bash
cd my-existing-project
curl -O https://raw.githubusercontent.com/tommasinigiovanni/claude-code-bootstrap/main/CLAUDE-ONBOARDING.md
mv CLAUDE-ONBOARDING.md CLAUDE.md
claude
```

That's it. Claude Code reads the `CLAUDE.md` and starts the wizard automatically.

> After the wizard finishes, it replaces itself with your project's actual `CLAUDE.md`.

## Example Session

Here's what a wizard session looks like (abbreviated):

```
🚀 Project Bootstrap Wizard

Hi! I'm your project bootstrap wizard.
First things first: what language should we brainstorm in?

> Italiano

Perfetto! Descrivimi il progetto in 2-3 frasi:
cosa vuoi costruire e per chi?

> Un sistema di prenotazione per stampanti 3D in un FabLab.
> Gli utenti devono poter vedere gli slot liberi e prenotare.

Chiaro. Domanda: è un MVP per validare l'idea o deve andare in produzione?

> MVP, ma con fondamenta solide per poi scalare.

...

[after 5 phases of brainstorm]

✅ Tutte le fasi confermate. Genero i 5 file:

  ✓ CLAUDE.md (70 lines)
  ✓ ai/PRD.md
  ✓ ai/ARCHITECTURE.md
  ✓ ai/AI_RULES.md
  ✓ ai/PLAN.md (6 phases)

Il wizard CLAUDE.md è stato sostituito col CLAUDE.md del progetto.
Usa /compact e inizia dalla Fase 1 del piano. Buon lavoro!
```

## Example Output

After the wizard completes, your project root looks like this:

```
my-project/
├── CLAUDE.md              ← 70 lines, agent rules (references @ai/ files)
├── ai/
│   ├── PRD.md             ← what we build, what we don't
│   ├── ARCHITECTURE.md    ← folder tree, data model, API design
│   ├── AI_RULES.md        ← code style, testing, git, anti-patterns
│   └── PLAN.md            ← 6 implementation phases with verification
└── (your code here)
```

See the [`examples/`](examples/) folder for complete sample outputs.

## Design Principles

### One question at a time
No walls of text, no question dumps. The wizard asks one thing, listens, adapts.

### Analyze first, ask later (onboarding)
The onboarding wizard reads your code before asking anything. It won't ask "what framework do you use?" when there's a `package.json` right there.

### Descriptive, not aspirational
`CLAUDE.md` documents the project **as it is**. Improvements go in `PLAN.md`. An aspirational CLAUDE.md that doesn't match reality causes more harm than no CLAUDE.md at all.

### < 150 lines for CLAUDE.md
[Research shows](https://www.humanlayer.dev/blog/writing-a-good-claude-md) that Claude can follow ~150-200 instructions reliably. Claude Code's system prompt already uses ~50. Your CLAUDE.md gets the rest. Keep it tight.

### Language flexibility
Instructions for Claude are in English (better instruction-following). The brainstorm and generated files are in whatever language you choose.

### Verification at every step
Every phase in `PLAN.md` includes a verification command. If you can't verify it, it didn't happen.

## Compatibility

These wizards are compatible with:

- **[GSD](https://github.com/gsd-build/get-shit-done)** — Get Shit Done framework
- **[Superpowers](https://github.com/obra/superpowers)** — TDD workflow with subagents
- **[BMAD](https://github.com/bmad-code-org/BMAD-METHOD)** — Multi-persona workflow
- **[Claude Code Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)** — Official Anthropic skills system

The generated files serve as a foundation. Layer any framework on top.

## FAQ

**Q: Can I use this with Cursor / Copilot / other AI tools?**
A: The wizard itself requires Claude Code. But the 5 output files work with any AI coding tool — rename `CLAUDE.md` to `AGENTS.md` for Cursor/Zed/OpenCode compatibility.

**Q: What if I want to skip some phases?**
A: Tell the wizard "skip this phase" — it'll use reasonable defaults and flag what was assumed.

**Q: What if my project is a mess?**
A: The onboarding wizard is designed for this. It'll be honest about what it finds and propose a cleanup plan in `PLAN.md`. No judgment, just structure.

**Q: Do I need to keep the wizard file after it's done?**
A: No. The wizard replaces itself with your project's CLAUDE.md. Keep the original wizard file somewhere if you want to bootstrap other projects.

**Q: Why 5 files and not just CLAUDE.md?**
A: CLAUDE.md must stay under 150 lines to be effective. The other 4 files live in `ai/` and are referenced via `@ai/PRD.md` syntax — Claude loads them on demand when relevant, without bloating every conversation.

## Resources

- [Everything Claude Code](https://github.com/affaan-m/everything-claude-code) — Complete config collection
- [CLAUDE.md Masterclass](https://newsletter.claudecodemasterclass.com/p/claudemd-masterclass-from-) — Deep dive on CLAUDE.md
- [Writing a Good CLAUDE.md](https://www.humanlayer.dev/blog/writing-a-good-claude-md) — Best practices and research
- [Claude Code Best Practices](https://code.claude.com/docs/en/best-practices) — Official Anthropic docs
- [Awesome Claude Code](https://github.com/hesreallyhim/awesome-claude-code) — Curated list of tools and configs
- [Remote Labor Index](https://www.remotelabor.ai/paper.pdf) — Why 97.5% of AI agents fail

## Contributing

PRs welcome. If you've found patterns that make the wizard better — different phase ordering, better questions, additional output files — open an issue or PR.

## License

Apache 2.0 — use it, fork it, adapt it.

---

**Author:** [Giovanni Tommasini](https://giovannitommasini.it) · [evoseed](https://evoseed.io/eng) / [AI Fabric](https://aifabric.it/en)

Built with the same spec-driven approach this repo teaches. 🔄
