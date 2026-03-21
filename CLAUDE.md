# CLAUDE.md — Agent Loop Skill

## Project Overview

Agent Loop is a Claude Code skill (v2.0) that solves complex problems through structured multi-agent debate and execution. It assembles expert agents, runs a pressure-tested roundtable discussion, then executes tasks with parallel dispatch and iterative grading.

**This is a documentation-only skill** — no source code, no dependencies, no build step. The skill is a set of Markdown files packaged into a `.skill` ZIP archive.

## Repository Structure

```
agent-loop/
├── agent-loop.skill                    # Packaged skill archive (ZIP)
├── skill-package/
│   └── agent-loop/
│       ├── SKILL.md                    # Main skill definition (815 lines)
│       └── references/
│           ├── agent-personas.md       # Agent persona library (12 agents)
│           ├── roundtable-protocol.md  # Debate protocol & disagreement gate
│           └── grading-framework.md    # Iterative quality grading methodology
└── CLAUDE.md                           # This file
```

## Key Files

- **`skill-package/agent-loop/SKILL.md`** — The core skill. Defines all 4 phases, execution modes, state management, output templates, and failure modes.
- **`references/agent-personas.md`** — Standing members (Casting Director, Anthropic Senior Engineer) and 10 roster agents with deep persona profiles.
- **`references/roundtable-protocol.md`** — 3-round debate protocol: opening positions, challenge & debate, convergence. Includes the Disagreement Gate.
- **`references/grading-framework.md`** — 8-15 criteria grading with up to 4 passes, stall detection, and escape hatch back to roundtable.

## Architecture: Four Phases

1. **Scoping** — Decompose problem, select agent team, recommend output template
2. **Roundtable** — In-context 3-round debate with Disagreement Gate enforcement
3. **Execution** — Real subagent dispatch via `Agent` tool, dependency-aware wave scheduling
4. **Grading Loop** — Iterative quality improvement (up to 4 passes), with escape hatch

The **Structured Execution Plan Table** (output of Phase 2) is the binding contract for Phase 3 — no additions, omissions, or reinterpretations without returning to roundtable.

## Execution Modes

| Mode | Checkpoints | When to Use |
|------|-------------|-------------|
| **First-Run** | 4 (full process) | Default, high-stakes, unfamiliar problems |
| **Repeat** | 2 (collapsed) | Pattern-recognized problems |
| **Express** | 1 (abbreviated) | Low-stakes with complexity flag warning |

## Output Templates

1. **Memo** — Analysis/strategy (default)
2. **Deck Outline** — Presentations/board materials
3. **Decision Matrix** — Vendor selection/option comparison
4. **Action Plan** — Operations/project planning

## State Management

The skill persists state across sessions via files:
- `meta.json` — Machine-readable phase/progress tracking
- `roundtable.md` — Full debate record
- `grading.md` — Append-only grading log
- `draft_v{N}.md` — Versioned drafts
- `final_{template}.md` — Final output

Resume protocol validates state integrity before continuing interrupted sessions.

## Conventions

- **Specificity over generality** — Every agent argues from domain expertise, every grading criterion tests concrete substance
- **Debate must be real** — Disagreement Gate fails if agents agree too easily; triggers agent swap or contrarian injection
- **User is decision-maker** — Checkpoint gates at team selection, execution plan, grading criteria, and final output
- **File-first, context-lean** — Deliverables >500 words go to disk, roundtable summaries stay under 1,000 tokens in context
- **Circuit breakers** — Disagreement Gate, dependency graph validation, state integrity checks, grading escape hatch, stall detection

## Development Notes

- No package.json, no npm dependencies, no tests, no CI/CD
- Changes go into the Markdown files under `skill-package/agent-loop/`
- The `agent-loop.skill` ZIP must be rebuilt after changes to ship the updated skill
- Skill triggers: "agent loop", "roundtable", "debate this", "assemble a team", "figure out the best approach"

## Complementary Skills

Agent Loop can invoke during execution:
- `corsair-ic` — IC deck formatting
- `deal-assessment` — PE diligence
- `board-review` — Board materials
- `deck-builder` — Presentations
- `ic-deck-pipeline` — Full IC workflows
- `deck-qa` — Presentation QA
