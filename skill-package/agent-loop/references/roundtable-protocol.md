# Roundtable Protocol

## Purpose

The roundtable forces expert agents to debate the best approach to a problem BEFORE any execution begins. This prevents the most common failure mode in AI workflows: jumping straight to a mediocre first-pass answer without questioning the approach.

## Why Debate Matters

Real experts disagree. A Bain partner and a McKinsey partner would frame the same problem differently — one might lead with profit pools, the other with capability-driven strategy. A COO would challenge both for ignoring execution complexity. An investment banker would ask about exit implications neither consultant considered.

When agents debate, three things happen:
1. Weak assumptions get exposed before they infect the analysis
2. The execution plan incorporates the strongest elements from multiple frameworks
3. Edge cases and risks surface that a single-perspective approach would miss

## Participants

### Selected Experts (3-6 agents)

Chosen by the orchestrator in Phase 1 based on the problem's domains. Each must bring a genuinely different lens. If removing an agent wouldn't cost the roundtable a unique perspective, don't include them.

### Standing Members (always present)

| Agent | Role | Presence |
|-------|------|----------|
| **Anthropic Senior Engineer** | Technical feasibility. Ensures the execution plan can be built within Claude Code's architecture. Has veto authority on technical feasibility during convergence. | Always |
| **Anthropic Coding Senior Engineer** | Implementation quality. Ensures generated code is production-grade with correct imports, error handling, and edge-case resilience. Has veto authority on code quality during convergence. | Conditional — activated when the Casting Director classifies the skill as complex (2+ of: code generation, 5+ tasks with 2+ dependency layers, multi-file cross-referenced output, programmatic consumption, state management/retry/error recovery). |

Standing members do not count toward the 3-6 expert limit. They participate in all three rounds. Their primary role is constraint-checking, not domain expertise. When the Coding Senior Engineer is present, they add a code quality bar to any execution plan task that produces code. When presenting the team to the user, list standing members separately from selected experts.

## Running the Roundtable

### Setting the Stage

Before agents speak, the orchestrator frames the problem clearly:
- What is the user trying to accomplish?
- What decisions need to be made?
- What constraints exist (time, budget, information availability)?
- What does "good" look like for this deliverable?

### Round 1: Opening Positions

Each agent gets a structured opening. They must address:

**Diagnosis**: "Here's what I think the real problem/opportunity is..."
- Not just restating the user's request, but reframing it through their expert lens
- What does this look like from their domain? What patterns do they recognize?

**Recommended Approach**: "Here's how I'd tackle this..."
- Specific methodology or framework they'd apply
- Why this approach over alternatives
- What data or inputs they'd need

**Predicted Blindspots**: "Here's what I think the other agents will miss..."
- What their domain reveals that others might overlook
- Common mistakes they've seen when their expertise is absent

**Each opening should be 3-5 sentences, capped at 300 words.** This isn't a monologue — it's a position statement designed to provoke productive disagreement. The orchestrator summarizes any agent that runs long.

**Post-Round 1 Redundancy Check**: The orchestrator compares opening positions. If two agents' recommendations converge on the same diagnosis AND the same approach (would produce the same execution plan), either merge their scope and drop one agent, or push the more generic of the two to differentiate: *"Your position overlaps with [Agent X]'s. What do you see that they are missing?"*

### Round 2: Challenge & Debate

The orchestrator drives the debate by:

1. **Identifying the biggest disagreement** — Where do two agents fundamentally disagree on diagnosis or approach?
2. **Forcing direct response** — "Agent X, the Strategy Consultant just said Y. You clearly disagree. What's wrong with their framing?"
3. **Probing assumptions** — "That recommendation assumes Z. Agent W, does that assumption hold in your experience?"
4. **Escalating** — If agents are being too polite, push harder. "The Contrarian thinks this whole approach has a 40% chance of failure. Strategy Consultant, defend your position."

Good debate questions:
- "What would have to be true for your approach to work?"
- "When have you seen this approach fail?"
- "What's the base rate of success for strategies like this?"
- "If you're wrong about [key assumption], what happens?"
- "The Operator says this can't be executed in 6 months. What's your response?"

### Disagreement Gate (mandatory after Round 2)

Before proceeding to Round 3, the orchestrator verifies that substantive disagreement occurred.

**What counts as a substantive disagreement:** Agents disagree on at least one of:
- **Diagnosis** — They define the core problem differently
- **Approach** — They recommend materially different execution strategies
- **Key assumptions** — One agent's plan depends on an assumption another agent explicitly rejects

**What does NOT count:** Differences in emphasis, wording, ordering, or level of detail. If Agent A says "focus on cost synergies first" and Agent B says "revenue synergies should come first," that is a disagreement on sequencing. If Agent A gives a specific number and Agent B says "synergies are real" without specifics, that is not a disagreement — it is one agent being lazy.

**Gate evaluation:**

- **PASSES** if at least one substantive disagreement exists that would change the execution plan (resolving it one way vs. the other leads to different tasks, deliverables, or sequencing). Proceed to Round 3.
- **FAILS** if zero substantive disagreements exist. The orchestrator MUST take one of these actions:

  **Option A — Agent Swap.** Replace the agent whose perspective is most redundant with another agent whose viewpoint is structurally opposed to the emerging consensus. State: *"Round 2 produced no substantive disagreements. Swapping [Agent X] for [Agent Y]. Re-running Round 2."*

  **Option B — Inject the Contrarian.** If not already present, add the Contrarian with explicit instruction: *"The prior round produced unanimous agreement. Your job is to find the assumption everyone is making that is most likely wrong, and build a case for an alternative approach."* Re-run Round 2.

  **Option C — Simplicity Acknowledgment.** If the orchestrator genuinely believes the problem is straightforward: *"Round 2 produced no substantive disagreements. This may indicate the problem is straightforward. Proceeding to convergence. If you believe this problem has more complexity than the agents are surfacing, say so and I will restructure the team."*

**Re-run limit:** The disagreement gate re-run happens at most once. If Round 2 still produces no disagreement after an agent swap or Contrarian injection, proceed with Option C and log the unanimous consensus.

**What gets logged in `roundtable.md`:**

```
## Disagreement Gate
- Substantive disagreements found: [count]
- Gate status: PASSED / FAILED — [action taken]
- If re-run: which agent was swapped/added and why
- Key disagreements that passed the gate: [list with one line each]
```

### Round 3: Convergence

After the debate, synthesize:

**Consensus Points** — List what all agents agree on. These are high-confidence elements of the execution plan.

**Productive Disagreements** — Disagreements that reveal genuine uncertainty or trade-offs. Capture both sides and let the user decide, or design the execution plan to test both hypotheses.

**Structured Execution Plan** — The approach that survives the debate, produced as a binding table:

```
| ID | Agent | Deliverable | Inputs | Dependencies | Quality Bar |
|----|-------|-------------|--------|--------------|-------------|
```

Column definitions:

| Column | What Goes Here |
|--------|---------------|
| **ID** | Short identifier: `T1`, `T2`, `T3`. Sequential. |
| **Agent** | The specific persona executing this task. Must match a Phase 1 agent. |
| **Deliverable** | A specific, concrete output. If you can't tell when it's done, rewrite it. |
| **Inputs** | Source material — uploaded files, web research targets, other agents' deliverables. Be explicit. |
| **Dependencies** | Either `parallel` or `depends_on: [T1, T3]`. Every task declares its dependency posture. |
| **Quality Bar** | What separates partner-ready from associate-draft. One sentence, specific enough that two people would grade identically. |

This table is the contract. Phase 3 executes exactly what it specifies. If the plan needs to change, return to the roundtable and amend the table.

**Dependency Graph Validation (mandatory):** After finalizing the table, the orchestrator must:
1. Check for cycles (hard error).
2. Check for orphan references (hard error).
3. Verify at least one task is tagged `parallel` (the graph needs a root node).

If any check fails, restructure dependencies within Round 3 before presenting to the user.

**Risk Register** — Top 3-5 risks surfaced by the roundtable:
- What could go wrong
- How likely it is
- What would mitigate it
- Who's responsible for monitoring it

## Roundtable Output Presentation

Show the user:
1. A summary of each agent's opening position (2-3 sentences each)
2. The key debates and where agents disagreed
3. The structured execution plan table
4. The risk register
5. Ask: "Does this approach make sense? Anything you want to adjust before we execute?"

The full roundtable record is written to `agent-loop-state/roundtable.md`. The in-context summary should be under 1,000 tokens. Wait for user confirmation before proceeding to execution.

## Roundtable Quality Checks

A good roundtable produces:
- At least one genuine disagreement that changes the execution plan
- Each agent contributes a perspective the others couldn't have provided
- The execution plan is more nuanced than any single agent would have produced
- Risks are concrete and specific, not generic
- The Anthropic Senior Engineer has confirmed the plan is technically feasible

A bad roundtable looks like:
- All agents basically agree (you picked agents that are too similar)
- The debate is superficial (agents are summarizing instead of arguing)
- The execution plan is just the first agent's suggestion with minor edits
- Risks are generic ("execution risk", "market risk") instead of specific
- The plan contains tasks that exceed context limits or require real-time subagent coordination

If the roundtable fails these checks, either swap agents for more diverse perspectives or push the debate harder before moving to execution.

## Express Mode Roundtable

When the orchestrator is running in Express mode:
- Agents state opening positions only — no challenge round, no multi-round debate.
- The Disagreement Gate is skipped (there is no Round 2 to evaluate).
- The orchestrator synthesizes a plan directly from opening positions.
- The dependency graph validation still runs — Express mode skips debate, not structural checks.
- The structured execution plan table is still produced — Express mode changes the process, not the output format.
