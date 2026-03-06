# PROJECT ONBOARDING WIZARD (Existing Project)

> This file guides the onboarding of Claude Code onto an existing project.
> Analyzes the codebase, brainstorms with the developer, generates:
> `CLAUDE.md` (root) + `ai/PRD.md`, `ai/ARCHITECTURE.md`, `ai/AI_RULES.md`, `ai/PLAN.md`.

## INSTRUCTIONS FOR CLAUDE

When the user starts a conversation in a directory with this file
and **existing code but no project CLAUDE.md**, activate the onboarding wizard.

**CRITICAL:** ANALYZE the codebase first, then ASK for confirmations. Never assume your analysis is complete — the developer knows the project better than you.

---

## PHASE 0 — AUTOMATIC DISCOVERY + LANGUAGE

Before speaking to the user, silently analyze the project:

```
Read and analyze (in order):
1. package.json / requirements.txt / go.mod / Cargo.toml (stack and dependencies)
2. Folder structure (tree, 3 levels deep, ignore node_modules/.venv/dist/build)
3. Config files: tsconfig, .eslintrc, prettier, biome, vite.config, next.config, etc.
4. README.md (if exists)
5. .env.example / .env.local (env vars — NEVER read .env)
6. docker-compose.yml / Dockerfile (if they exist)
7. CI/CD: .github/workflows/, .gitlab-ci.yml
8. DB schema: prisma/schema.prisma, drizzle/, migrations/, models/
9. Tests: look for __tests__, test/, spec/, *.test.*, *.spec.*
10. Git: last 10 commits (git log --oneline -10), active branch
```

Then introduce yourself and ask the language question FIRST:

```
🔍 Project Onboarding Wizard

Hi! I've analyzed your codebase. I'll guide you through a brainstorm
to generate the 5 files that control AI agents on your project.

First: what language should we work in?
(English, Italiano, Español, Deutsch, Français, 日本語, 中文, …)
```

After the user picks a language, present the discovery results IN THAT LANGUAGE:

```
Here's what I found:

📦 Stack: [detected stack]
📁 Structure: [detected pattern: monorepo/feature-based/layer-based/other]
🧪 Tests: [detected framework or "no tests found"]
🔧 Build: [detected tool]
📝 Git: [last commit, active branch]

Did I get this right? What am I missing?
```

From this point forward, conduct the ENTIRE conversation in the user's chosen language.
All generated files MUST also be written in that language, EXCEPT:
- Code examples, commands, and file names → always in English
- Variable/function/class names → always in English

---

## PHASE 1 — VISION AND CONTEXT (verify + gather)

The goal is to understand the WHY of the project. Code tells you HOW, not why. Ask ONE question at a time.

**To discover (code doesn't tell you this):**
- What is this project for? (elevator pitch)
- Who uses it? (target user)
- What's the current state? (working MVP / in development / legacy cleanup)
- What do you want to do NEXT? (direction)
- What is out of scope? (what it must NOT become)
- Are there known tech debt items or things you'd like to change?

**Adapt questions to what you found in the code.** If you found Stripe, ask about the business model. If you found auth, ask about user count. If there are no tests, ask if that's intentional or debt.

Recap and confirmation before proceeding.

---

## PHASE 2 — STACK AND CONVENTIONS (confirm what you detected)

Present a detailed recap of what you detected from code and ask for confirmations/corrections:

**Stack (to confirm):**
- Language and version
- Frontend/backend framework
- Database
- Auth
- Hosting/deploy
- Package manager

**Conventions (detected from code):**
- Import style (by looking at existing files)
- Naming conventions (by looking at file names, variables, functions)
- Formatter/linter (from config)
- Component/module structure (from tree)

**Ask explicitly:**
- "I noticed you use [X]. Is that intentional or would you like to change it?"
- "I didn't find [tests/linter/CI]. Should we add it?"
- "Commits don't follow a specific pattern. Want to adopt conventional commits?"

Be specific: cite real files and folders from the project. Show you actually read the code.

Recap and confirmation.

---

## PHASE 3 — ARCHITECTURE (map the existing)

Propose an architecture map based on what you found:

- **Actual folder structure** (tree from codebase)
- **Identified pattern** (MVC, feature-based, layer-based, none clear)
- **Data model** (from schemas or models found)
- **API/Routes** (from routes found in code)
- **Main flows** (from what you understood)
- **External dependencies** (services, third-party APIs)

**Ask:**
- "Is this map accurate? What's missing?"
- "Are there parts of the code you consider 'dirty' or in need of refactoring?"
- "Are there hidden dependencies or external services I didn't find in the code?"
- "Is there documentation elsewhere I should consider?"

Refine with user and confirm.

---

## PHASE 4 — OPERATIONAL RULES (align expectations)

Based on detected conventions + user preferences, define the rules:

**Ask only what you can't deduce from code:**
- Testing: TDD? Minimum coverage? Which runner for which test type?
- Git workflow: how do you handle branches and PRs?
- Code review: who reviews? CI mandatory?
- Deploy: manual or automatic? Staging environment?
- Things Claude must NEVER touch (critical files, production config, etc.)
- Things Claude must ALWAYS do (test before commit, lint, etc.)

Recap and confirmation.

---

## PHASE 5 — NEXT STEPS PLAN (look forward)

Based on the direction from Phase 1 and the code state, propose a plan:

- **If there's tech debt** → first phase is cleanup
- **If tests are missing** → phase for test coverage on critical parts
- **If there are features to add** → feature-by-feature plan

Each phase must:
- Be completable in a single Claude Code session
- Have a verifiable deliverable
- Specify files to create/modify
- Include verification commands
- Not break anything existing (backward compatibility)

Propose, discuss, confirm.

---

## PHASE 6 — FILE GENERATION

ONLY after explicit confirmation of all phases, generate all 5 files.
Write in the user's chosen language (except code/commands/names → English).

**File locations:**
- `CLAUDE.md` → project root (Claude Code reads it from here)
- `ai/PRD.md`, `ai/ARCHITECTURE.md`, `ai/AI_RULES.md`, `ai/PLAN.md` → `ai/` folder

Create the `ai/` directory if it doesn't exist.

### File 1: `CLAUDE.md` (project root, < 150 lines)

Must reflect the project AS IT IS, not as you'd ideally want it.
Must include `@ai/` references so Claude loads the spec files on demand.

```markdown
# Project Name
One-line description

## Stack
- technologies WITH VERSIONS (from package.json/lockfile)

## Commands
- `command`: description (ONLY commands that WORK NOW)

## Structure
- ACTUAL folder map of the project

## Code Style
- conventions OBSERVED in existing code
- new rules agreed with the developer

## Workflow
- how to work on the project
- how to verify changes
- how to deploy (if applicable)

## IMPORTANT
- files NOT to touch
- critical rules
- project-specific gotchas

## References
- Product requirements: @ai/PRD.md
- Architecture and data model: @ai/ARCHITECTURE.md
- Coding rules and anti-patterns: @ai/AI_RULES.md
- Implementation plan: @ai/PLAN.md
```

### File 2: `ai/PRD.md`

```markdown
# PRD — Project Name
## Vision
## Current state (what works today)
## Target user
## Problem
## Core features (existing)
## Planned features (next)
## What this is NOT (out of scope)
## Known tech debt
## Constraints and assumptions
```

### File 3: `ai/ARCHITECTURE.md`

```markdown
# Architecture — Project Name
## Overview
## Tech stack (with versions)
## Folder structure (ACTUAL tree)
## Data model (EXISTING schema)
## API / Routes (EXISTING)
## Main flows
## Services and external dependencies
## Architectural decisions (why it's built this way)
## Tech debt and planned refactoring
## Security
## Performance considerations
```

### File 4: `ai/AI_RULES.md`

```markdown
# AI Rules — Project Name
## Code style (observed + agreed)
## Testing (framework, coverage, conventions)
## Git & commits
## Naming conventions
## Error handling (pattern used in the project)
## Logging
## Protected files (NEVER modify without confirmation)
## Project-specific anti-patterns
## Verification commands
- `test command`
- `lint command`
- `build command`
- `type-check command` (if applicable)
```

### File 5: `ai/PLAN.md`

```markdown
# Plan — Project Name

## Current state
Brief snapshot of where we are.

## Phase 1: [name] — [time estimate]
### Objective
### Files to create/modify
### Steps
- [ ] step 1
- [ ] step 2
### Verification
- command to verify phase completion
### Backward compatibility
- what must NOT break

## Phase 2: [name] — [time estimate]
...
```

---

## WIZARD RULES

1. **Ask language first.** Then conduct everything in that language.
2. **Analyze FIRST, ask LATER.** Don't ask questions the code can answer.
3. **Cite real files.** Don't say "your framework", say "Next.js 14 from package.json".
4. **One question at a time.** Natural conversation, not interrogation.
5. **Recap after each phase.** Summarize and ask "confirm?" before proceeding.
6. **Respect the existing.** CLAUDE.md describes the project as it IS, not as it ideally should be. Improvements go in ai/PLAN.md.
7. **Don't touch the code.** This wizard generates ONLY the 5 spec files. No code modifications.
8. **Generate only at the end.** Files are created ONLY after Phase 5 is confirmed.
9. **After generation**, replace this file with the project's CLAUDE.md.
10. **If user asks to skip**, allow skipping phases, but flag what was assumed.
11. **If the project is a mess**, be honest but constructive. Propose a cleanup plan in ai/PLAN.md.
12. **File language:** user's language for prose, English for code/commands/names.
13. **File layout:** CLAUDE.md in root, everything else in `ai/`. CLAUDE.md references specs via `@ai/`.

---

## ANTI-PATTERNS TO AVOID

- ❌ Asking questions the code already answers (stack, structure, dependencies)
- ❌ Proposing a different stack than what's in use unless user asks
- ❌ Aspirational CLAUDE.md instead of descriptive of reality
- ❌ Ignoring tech debt — document it in ai/PRD.md and ai/PLAN.md instead
- ❌ Plan that breaks backward compatibility without warning
- ❌ CLAUDE.md > 150 lines (will be ignored by the agent)
- ❌ Commands in CLAUDE.md that don't actually work
- ❌ Idealized architecture that doesn't match the code
- ❌ Putting spec files in root instead of `ai/`

---

## NOTES

- Compatible with GSD, Superpowers, and BMAD frameworks
- Follows best practices from "Everything Claude Code" and "CLAUDE.md Masterclass"
- Generated CLAUDE.md must be < 150 lines with ONLY universally applicable rules
- For domain-specific rules, use CLAUDE.md in subdirectories or skills
- Claude loads `@ai/` files on demand — they don't bloat every conversation
- After onboarding, use `/compact` to free context and start working with ai/PLAN.md

**Author:** Giovanni Tommasini · giovannitommasini.it
**Framework:** Evoseed / aiFabbric
