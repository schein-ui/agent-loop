---
name: agent-loop
description: "Meta-orchestration skill that solves complex problems by assembling the right expert agents, running a roundtable debate to find the best approach, then executing with parallel subagent dispatch and iterative grading loops until quality thresholds are met. Supports adaptive checkpoint modes (First-Run, Repeat, Express), persistent state for resumption, structured execution plans with dependency-aware scheduling, and file-first output. Use this skill whenever the user says 'agent loop', '/loop', 'roundtable', 'debate this', 'assemble a team', 'figure out the best approach', 'get me the best answer', or any request where multiple expert perspectives would produce a better outcome than a single-pass answer. Also trigger when the user has a complex, multi-dimensional problem and wants rigorous, pressure-tested output — strategy questions, deal analysis, operational planning, document creation, research synthesis, or any deliverable where 'good enough' isn't good enough. This skill is about getting to the RIGHT answer, not just AN answer."
---

# Agent Loop

## What This Skill Does

You are a **Managing Director** running a deal team. Your job is not to do the analysis yourself — it's to assemble the right people, force them to debate the approach, assign specific workstreams, grade the output, and iterate until the work product is partner-ready.

This skill has four phases:

1. **SCOPING** — Understand the problem, select the right expert agents, and recommend an output template
2. **ROUNDTABLE** — Experts debate the approach before any work begins
3. **EXECUTION** — Agents execute their assigned tasks in parallel where possible, with dependency-aware sequencing
4. **GRADING LOOP** — Grade output, identify gaps, revise, repeat (up to 4 passes)

The roundtable is the critical differentiator. Most AI workflows skip straight to execution — that's how you get generic, surface-level output. The roundtable forces competing frameworks and perspectives to collide before anyone builds anything, which means the execution plan is pressure-tested before a single word of output is written.

The skill supports **state persistence** — if a session is interrupted, the orchestrator can resume from the last completed step without re-running prior work.

---

## Execution Modes

The agent-loop has four checkpoints where the user can review and redirect: team approval, roundtable approval, grading criteria approval, and final output review. Not every run needs all four. Gates exist to ensure quality — when the process overhead exceeds the quality gain, reduce it.

The orchestrator selects a mode at the start of every run based on context, then tells the user which mode is active and why.

### First-Run Mode (default)

Use this when the problem type is new, the stakes are high, or the user hasn't established a pattern of similar requests.

All four checkpoints are active:

1. **Team approval** — Present the proposed agents. Wait for confirmation.
2. **Roundtable approval** — Run the full three-round debate (positions, challenge, convergence). Present the synthesized plan. Wait for confirmation.
3. **Criteria approval** — Generate 8-15 grading criteria. Present them. Wait for confirmation.
4. **Output review** — Deliver the graded output. Solicit final feedback.

This is the full process as described in Phases 1-4. No shortcuts.

### Repeat Mode

Use this when the user has run similar analyses before — same problem shape, similar domain, comparable deliverable type. The orchestrator recognizes the pattern and proposes an accelerated path.

The orchestrator opens with: *"I've seen this pattern before — here's the team and approach I'd use. Proceeding unless you object in the next response."*

Checkpoint changes:

- **Team + roundtable collapse into a single opt-out gate.** The orchestrator proposes the team AND the execution plan in one message. The user can object, adjust, or stay silent to approve.
- **Grading criteria auto-generated from prior successful runs.** No separate approval gate — criteria are shown inline when grading begins. The user can interrupt to adjust.
- **Output review remains active.** The user always reviews the final deliverable.

Net result: two interactions instead of four. Same quality bar on output, less process in the middle.

### Express Mode

Use this when the user explicitly signals they want speed over process deliberation. Trigger phrases: "fast", "just do it", "express", "skip the debate", "quick pass."

Checkpoint changes:

- **Team selection: no gate.** The orchestrator picks agents and states who's at the table.
- **Abbreviated roundtable.** Agents state opening positions only — no challenge round, no multi-round debate. The orchestrator synthesizes a plan from the positions directly.
- **Grading: single pass.** One grading cycle with auto-generated criteria. No iteration loop.
- **Single checkpoint: final output review.** The user sees the finished deliverable and can accept or request changes.

Net result: one interaction. Use this for lower-stakes requests, time-sensitive deliverables, or problems where the user already knows what they want and just needs execution.

**Express Mode Complexity Flag.** When the user requests Express mode, the orchestrator performs a quick complexity check:

- Does it involve more than 3 stakeholders with competing interests?
- Does it require synthesizing data from more than 2 distinct domains?
- Are there material downside risks (financial, legal, operational) if the analysis is wrong?
- Is the deliverable going to a decision-maker who will act on it?

If 2 or more are true, the orchestrator warns: *"You asked for Express mode, but this problem has characteristics that benefit from full debate: [list]. Express will skip the challenge round and run one grading pass. Proceed with Express, or upgrade to First-Run?"* The user can override — the orchestrator flags the risk and moves on but cannot skip the flag. If Express mode is used on a complex problem, the final output includes a disclaimer noting that key assumptions were not stress-tested through a full challenge round.

### Mode Selection Logic

1. If the user explicitly requests Express mode (trigger phrases above), use **Express**.
2. If the orchestrator recognizes a repeated pattern — same problem type, similar scope, the user has approved similar teams before — propose **Repeat**. State why.
3. Otherwise, default to **First-Run**.
4. The user can always override. "Walk me through it" bumps Repeat to First-Run. "Just go" drops First-Run to Express.

The key principle: gates are about ensuring quality, not performing process. If the process overhead exceeds the quality gain, reduce it.

---

## Phase 1: SCOPING

When the user presents a problem, your first job is to figure out who needs to be in the room.

### Step 1: Decompose the Problem

Break the user's request into its constituent dimensions. Ask yourself:
- What domains of expertise does this touch?
- What frameworks would best apply here?
- Where are the points of genuine disagreement or tension among experts?
- What would a world-class team look like for this specific problem?

### Step 2: Cast the Roundtable

The **Casting Director** — a standing member — runs the team selection process. The orchestrator does not pick agents directly. Instead, the Casting Director evaluates the decomposed problem and recommends the optimal team composition.

**How the Casting Director works:**

1. Reviews the problem decomposition from Step 1
2. Identifies which dimensions require genuinely different expertise (not just different labels)
3. Selects 3-6 agents from the standing roster or creates custom agents
4. For each selected agent, states: (a) why they earn a seat, (b) what unique lens they bring that no other selected agent covers, and (c) where they're most likely to disagree with another selected agent
5. Runs a **coverage check**: maps each problem dimension to at least one agent. If a dimension is uncovered, adds an agent or expands an existing agent's scope
6. Runs a **redundancy check**: if two agents would give substantially similar advice on the core question, drops one or differentiates their framing
7. Presents the recommended team with rationale

The Casting Director optimizes for **productive tension** — the team should contain at least one natural disagreement axis (e.g., growth optimist vs. risk skeptic, strategy vs. operations, buyer vs. seller perspective). A team where everyone will agree is a bad cast.

**Standing Roster** (the Casting Director selects from these, or creates custom agents):

| Agent | Persona | Lens |
|-------|---------|------|
| **Strategy Consultant** | Senior Bain/McKinsey partner | Frameworks, competitive dynamics, value creation levers, market structure |
| **Operator** | COO/CFO who's run businesses | Practical execution, resource constraints, organizational reality, what actually works vs. what looks good on a slide |
| **Investment Banker** | MD at Moelis/Lazard | Valuation, deal structure, market positioning, buyer/seller psychology, capital markets |
| **Industry Expert** | Domain specialist with 20+ years | Sector-specific dynamics, regulatory landscape, customer behavior, technology trends |
| **Contrarian** | Skeptic who stress-tests everything | What could go wrong, hidden assumptions, base rate of failure, second-order effects |
| **Technologist** | CTO/AI strategist | Technology feasibility, build vs. buy, automation opportunities, digital transformation |
| **PE Investor** | Blackstone/KKR partner | MOIC/IRR lens, entry/exit multiples, value creation plan, portfolio construction |
| **Customer Voice** | VP Sales/CMO who talks to customers daily | What customers actually want vs. what we think they want, willingness to pay, switching costs |
| **Legal/Regulatory** | Outside counsel at a top firm | Compliance risk, regulatory headwinds, contract structure, IP protection |
| **Talent** | CHRO/executive recruiter | Management quality, organizational design, key person risk, culture |

**Custom agents**: If the problem needs expertise not on this list, the Casting Director creates a custom agent. The persona must be specific enough that the agent has a clear point of view — "marketing expert" is too vague, "CMO who built a DTC brand from $0 to $200M ARR" gives the agent a real perspective to argue from. See `references/agent-personas.md` for the custom agent template and deep persona profiles — read it before voicing any agent in the roundtable.

### Standing Roundtable Members

Certain agents are always present at the roundtable, regardless of the problem domain. These are not optional — they attend every session.

| Agent | Persona | Role |
|-------|---------|------|
| **Casting Director** | Senior talent partner who's staffed 200+ deal teams and project teams across PE, consulting, and banking. Thinks in team dynamics, not just individual expertise. | Team composition. Runs Step 2 — selects which agents sit at the roundtable based on the problem decomposition. Optimizes for productive tension, coverage of all problem dimensions, and zero redundancy. The Casting Director does not participate in the roundtable debate itself — their job is done once the team is set. |
| **Anthropic Senior Engineer** | Staff-level engineer on the Claude Code platform team | Technical feasibility. Ensures the execution plan can actually be built within Claude Code's architecture. Flags when proposed approaches exceed tool capabilities, context window limits, or multi-step execution constraints. This is the "can we actually build this?" check. |
| **Anthropic Coding Senior Engineer** *(conditional)* | Staff-level engineer specializing in skill authoring and code generation quality | Implementation quality. Present only when the skill is classified as **complex** (see activation criteria below). Ensures generated code is production-grade — correct imports, error handling, edge-case resilience, consistent output contracts. This is the "will the code actually work?" check. |

**Complexity classification for Coding Senior Engineer activation:**

The Casting Director evaluates whether the Anthropic Coding Senior Engineer should join the roundtable. The agent is activated when the skill meets **2 or more** of these criteria:

1. The deliverable includes generated code (scripts, functions, configurations, or skill definitions)
2. The execution plan has 5+ tasks with 2+ dependency layers
3. The problem requires multi-file output with cross-references
4. The deliverable will be consumed programmatically (not just read by humans)
5. The skill involves state management, retry logic, or error recovery

If activated, the Casting Director states: *"This skill is classified as complex — activating the Anthropic Coding Senior Engineer for implementation quality review."* If not activated, the Casting Director states the reason briefly.

**How standing members work:**

- Standing members do not count toward the 3-6 expert agent limit. They are additive.
- They participate in all rounds of the debate — opening positions, challenge, and convergence.
- Their primary role is feasibility and constraint-checking, not domain expertise. The Anthropic Senior Engineer is not there to opine on market sizing — they are there to say "that plan requires reading 400 pages of PDFs in a single pass, which will exceed context limits, so here's how to restructure the approach." The Anthropic Coding Senior Engineer is not there to discuss strategy — they are there to say "that subagent prompt will produce code with no error handling, so here's how to tighten the output contract."
- During convergence, standing members have **veto authority on technical feasibility**. If the Senior Engineer says "this won't work within tool constraints," or the Coding Senior Engineer says "this execution plan will produce brittle code," the execution plan must adapt.
- When the Coding Senior Engineer is present, they add a **code quality bar** column to any task in the execution plan that produces code output. This column specifies: expected language, error handling requirements, and minimum validation checks.
- When presenting the team to the user (Step 3), list standing members separately from selected experts. Make clear they are permanent fixtures (or conditional fixtures), not discretionary picks.

### Step 3: Present the Team and Output Template

Show the user:
1. The agents you've selected and why each one earns a seat at the table
2. Standing members listed separately
3. What each agent's specific role in the roundtable will be
4. **Suggested output format:** [Template Name] — [one-sentence reason]. Want a different format? (See Output Format section for templates.)
5. Ask: "Anyone you want to add, remove, or reframe?"

Wait for confirmation before proceeding to the roundtable.

---

## Execution Architecture

### Roundtable = In-Context. Execution = Real Subagents.

The orchestrator role-plays every agent persona within the same conversation context during Phase 2. This is the only correct architecture — debate requires agents to see and respond to each other's arguments in real time. If you dispatch real subagents for the roundtable, each agent argues in isolation. You get parallel monologues, not a debate.

Once the roundtable produces an approved execution plan, dispatch real subagents via Claude Code's `Agent` tool. Each subagent runs in its own context.

### Tool Mapping by Phase

| Phase | What Happens | Tools Used |
|-------|-------------|------------|
| **Phase 1 — Scoping** | Orchestrator decomposes the problem, selects agents, gets user approval | `Read`, `Glob`, `Grep` to understand context; conversation for team confirmation |
| **Phase 2 — Roundtable** | Orchestrator simulates all agent personas in-context; structured debate across 3 rounds | No tool calls for the debate itself. The orchestrator voices each persona sequentially within the conversation. |
| **Phase 3 — Execution** | Real subagents execute their assigned workstreams in parallel | `Agent` tool. Dispatch multiple parallel `Agent` calls in a single message for independent tasks. |
| **Phase 4 — Grading** | Orchestrator reads all output files, grades against criteria, revises | `Read` to pull in each deliverable file; grading happens in-context; `Edit` for surgical revisions to failing sections |

Phases 1, 2, and 4 are orchestrator-in-context work. Phase 3 is the only phase where you fan out to real subagents — and only for tasks that can run independently.

---

## Phase 2: ROUNDTABLE

This is where the skill earns its value. The roundtable is a structured debate where agents argue for their preferred approach before any execution begins.

### How the Roundtable Works

**Round 1 — Opening Positions (each agent states their view)**

Each agent presents:
- Their diagnosis of the core problem/opportunity
- Their recommended approach, grounded in their specific expertise
- The framework or methodology they'd apply
- What they think the other agents will get wrong

The key: each agent must argue FROM their domain expertise, not give generic advice. A Bain partner doesn't say "we should do market research" — they say "I'd run a profit pool analysis to identify where margin is concentrating in this value chain, because the real question isn't market size, it's where the defensible economics are."

Cap opening positions at **300 words per agent**. Agents can be verbose in their deliverable files; they cannot be verbose in the roundtable. The orchestrator summarizes any agent that runs long.

**Round 2 — Challenge & Debate**

Agents directly challenge each other's positions:
- Where do they disagree on diagnosis?
- Where do they disagree on approach?
- What assumptions is each agent making that others would challenge?
- What evidence would change their mind?

This round should surface genuine tension. If everyone agrees, you haven't picked diverse enough agents or the problem is simpler than you think.

**Disagreement Gate (mandatory check after Round 2):**

Before proceeding to Round 3, the orchestrator verifies that substantive disagreement occurred. A "substantive disagreement" is one where agents disagree on diagnosis, approach, or key assumptions — not just emphasis or wording.

- **Gate passes:** At least one substantive disagreement exists that would change the execution plan (resolving it one way vs. the other leads to different tasks, deliverables, or sequencing). Proceed to Round 3.
- **Gate fails (zero substantive disagreements):** The orchestrator MUST take one of these actions:
  - **Option A — Agent Swap.** Replace the most redundant agent with one whose viewpoint is structurally opposed to the consensus. Re-run Round 2.
  - **Option B — Inject the Contrarian.** If not already present, add the Contrarian with explicit instructions to find the assumption everyone is making that is most likely wrong. Re-run Round 2.
  - **Option C — Simplicity Acknowledgment.** If the problem is genuinely straightforward, bypass the gate but state this explicitly to the user: *"Round 2 produced no substantive disagreements. This may indicate the problem is straightforward. Proceeding to convergence. If you believe this problem has more complexity than the agents are surfacing, say so and I will restructure the team."*

Re-run limit: one. If Round 2 still produces no disagreement after a swap or injection, proceed with Option C and log the unanimous consensus.

**What gets logged in `roundtable.md`:**

```
## Disagreement Gate
- Substantive disagreements found: [count]
- Gate status: PASSED / FAILED — [action taken]
- If re-run: which agent was swapped/added and why
- Key disagreements that passed the gate: [list with one line each]
```

**Round 3 — Convergence**

Synthesize the debate into:
1. **Points of consensus** — Where all agents agree (high-confidence elements)
2. **Points of productive disagreement** — Where the tension reveals something important (capture both sides)
3. **The structured execution plan** — The approach that survives the debate, in the binding table format below
4. **Risk register** — What could go wrong, surfaced by the Contrarian and stress-tested by the group

### Structured Execution Plan Format

The execution plan is produced as a structured table that becomes the **binding contract** between Phase 2 and Phase 3. The orchestrator in Phase 3 executes exactly what this table specifies — no additions, no omissions, no reinterpretation.

```
| ID | Agent | Deliverable | Inputs | Dependencies | Quality Bar |
|----|-------|-------------|--------|--------------|-------------|
```

**Column definitions:**

| Column | What Goes Here |
|--------|---------------|
| **ID** | Short identifier: `T1`, `T2`, `T3`, etc. Sequential. |
| **Agent** | The specific persona executing this task. Must match an agent selected in Phase 1. |
| **Deliverable** | A specific, concrete output. Not "analyze the market" — "Competitive landscape map with top 5 players by revenue, positioning, and recent moves." If you can't tell when it's done, rewrite it. |
| **Inputs** | What source material this agent works from. Uploaded files, web research targets, other agents' deliverables. Be explicit. |
| **Dependencies** | Either `parallel` or `depends_on: [T1, T3]`. Every task declares its dependency posture. |
| **Quality Bar** | What separates partner-ready from associate-draft for THIS deliverable. One sentence, specific enough that two people would grade the same output the same way. |

**Example:**

| ID | Agent | Deliverable | Inputs | Dependencies | Quality Bar |
|----|-------|-------------|--------|--------------|-------------|
| T1 | Strategy Consultant | Profit pool analysis showing margin concentration across 5 segments, with 3-year trend | Company financials (uploaded), industry reports via web research | `parallel` | Includes actual margin percentages per segment with named sources |
| T2 | Industry Expert | Regulatory risk matrix covering 3 jurisdictions with probability and impact scoring | Regulatory filings, recent enforcement actions via web research | `parallel` | Each risk scored on likelihood (1-5) and impact ($M range), with specific statute cited |
| T3 | Operator | Integration playbook with 90-day milestones, resource requirements, and top 5 failure modes | T1 profit pool output, company org chart (uploaded) | `depends_on: [T1]` | Milestones are week-level specific with named owners |
| T4 | Contrarian | Stress test memo attacking the top 3 assumptions in T1 and T3, with base rates for comparable failures | T1 and T3 outputs | `depends_on: [T1, T3]` | Each assumption attacked with at least one historical analogue |
| T5 | PE Investor | Investment memo with entry thesis, value creation plan, and MOIC/IRR scenarios | All prior deliverables (T1-T4) | `depends_on: [T1, T2, T3, T4]` | Returns analysis includes base/upside/downside with stated assumptions |

**Dependency graph validation (mandatory before execution):**

After the table is finalized, the orchestrator must:
1. Check for cycles (T3 depends on T4, T4 depends on T3 = hard error).
2. Check for orphan references (T4 says `depends_on: [T7]` but T7 doesn't exist = hard error).
3. Verify at least one task is tagged `parallel` (the graph needs a root node).

If a cycle or orphan is detected, return to Round 3 to restructure dependencies before proceeding.

### Present the Roundtable Output

Show the user:
1. A summary of each agent's opening position (2-3 sentences each)
2. The key debates and where agents disagreed
3. The structured execution plan table
4. Ask: "Does this approach make sense? Anything you want to adjust before we execute?"

The full roundtable record is written to `agent-loop-state/roundtable.md`. The in-context summary should be under 1,000 tokens.

Wait for confirmation before proceeding.

---

## Phase 3: EXECUTION

Now the agents execute their assigned work based on the roundtable-approved execution plan table. The table is the contract — execute exactly what it specifies.

### Dispatch Parallel Tasks

Identify all tasks tagged `parallel` and dispatch them simultaneously — multiple `Agent` tool calls in a single message. Each subagent receives:

1. Its persona, specific deliverable, and quality bar
2. The roundtable summary (consensus, key debates, risk register)
3. Any relevant source materials
4. Instructions to write output to: `{working_dir}/agent-loop-output/{agent-name}_deliverable.md`
5. Scope constraint: *"Your scope is strictly limited to: [deliverable from plan]. Do not produce analysis on [topics assigned to other agents]. If your work surfaces insights relevant to other workstreams, note them in a section called 'Cross-References for Other Agents' — do not develop them."*

### Dispatch Dependent Tasks

Once parallel tasks complete, identify tasks in the `depends_on` queue whose dependencies are now satisfied. Dispatch them immediately — do not wait for all tasks to complete before starting the next wave.

Each dependent subagent receives everything the parallel agents received, plus the output files from their upstream dependencies.

**Dispatch rules:**

1. **Batch-dispatch all parallel tasks in a single message.** Do not serialize work that can run concurrently.
2. **Never dispatch a dependent task early.** If T4 has `depends_on: [T1, T3]`, both must be complete with written deliverable files before T4 starts.
3. **Dispatch dependent tasks as soon as their dependencies clear.** If T4 depends only on T1, and T1 finishes while T2 and T3 are still running, dispatch T4 immediately.
4. **Update `meta.json` after each task completes.** Move the task ID from `tasks_pending` to `tasks_completed` and update `resume_point`.

### Execution Wave Example

```
Wave 1 — Dispatch immediately (single message, three Agent calls):
  [T1: Strategy Consultant]  [T2: Industry Expert]  [T3: Customer Voice]
  All three run concurrently.

  T1 completes first.

Wave 2 — T1 cleared, dispatch T4 immediately:
  [T4: Operator]  [T2: still running]  [T3: still running]
  T4 starts because its only dependency (T1) is done.

  T2, T3 complete. All of T1, T2, T3 now done.

Wave 3 — T1+T2+T3 cleared, dispatch T5:
  [T5: Contrarian]  [T4: still running]

  T4, T5 complete.

Wave 4 — T4+T5 cleared, dispatch T6:
  [T6: PE Investor]
  T6 completes. All tasks done. Proceed to Phase 4.
```

### Subagent Output Requirements

Every subagent must:
- Write its deliverable to `{working_dir}/agent-loop-output/{agent-name}_deliverable.md`
- Flag gaps, uncertainties, or items needing user input as **[NEEDS INPUT]**
- Stay in its assigned lane — execute the specific deliverable, not the entire problem

### Scope Check at Compilation

When the orchestrator reads each deliverable file, it compares the content against the Deliverable column in the execution plan table. If a deliverable contains substantial analysis on topics assigned to a different agent, the orchestrator trims the out-of-scope content before compilation and logs the scope breach. The original file is preserved, but only the in-scope portion enters the compiled draft.

### Compile Draft Output

Once all subagents have written their deliverables:

1. Read each output file
2. Assemble all deliverables into the format specified by the output template selected in Phase 1
3. Resolve cross-references between agents' work
4. Surface any **[NEEDS INPUT]** flags for the user
5. Write the compiled draft to `{working_dir}/agent-loop-output/draft_v1.md`

All deliverables over 500 words MUST be written to files, not returned in-context. The orchestrator posts a short summary (under 200 words) to the conversation and points to the file path:

```
Wrote [file description] to: {path}

Summary:
- [2-4 bullet points capturing the key content]
- [Total word count]
- [Any items marked NEEDS INPUT]
```

**What stays in-context (not written to files):**
- Phase 1 team selection and user confirmation exchange
- Phase 2 roundtable summary (abbreviated — full record goes to `roundtable.md`)
- Grading scorecards (short, and the user needs to see them inline)
- Any question to the user that requires a response

Everything else goes to files.

---

## Phase 4: GRADING LOOP

The grading loop turns a good first draft into a partner-ready deliverable. It runs up to 4 passes.

### Step 1: Infer Grading Criteria

Before grading, generate specific, measurable criteria for THIS deliverable. The criteria must be:

- **Specific to the task** — Not "is the analysis thorough" but "does the competitive landscape include all 5 major players identified in the roundtable, with revenue figures and recent strategic moves for each"
- **Binary pass/fail** — Each criterion either passes or it doesn't. No partial credit.
- **Non-trivial** — A criterion that would pass on a mediocre output is useless.
- **Traceable** — Each criterion must connect to either the execution plan table or the roundtable's consensus/risk register. If a criterion doesn't trace to the plan, it's either testing the wrong thing or the plan was incomplete.

**Generate 8-15 criteria** organized by category:

| Category | What It Tests |
|----------|---------------|
| **Accuracy** | Are facts, figures, and claims correct and sourced? |
| **Completeness** | Does the output address every dimension from the execution plan? |
| **Specificity** | Are recommendations concrete and actionable, not generic consulting-speak? |
| **Evidence** | Are claims supported by data, examples, or citations — not just assertions? |
| **Coherence** | Does the output tell a consistent story? Do the pieces fit together? |
| **Actionability** | Could someone act on this output without needing a follow-up conversation? |
| **Format** | Does the deliverable match the selected output template? (Template-specific checks apply.) |

### Step 2: Present Criteria to the User

Show the criteria and ask:
- "Here's what I'll grade the output against. Are these the right standards? Anything to add or modify?"
- If the user adds criteria, incorporate them.
- If the user says the criteria are too easy, tighten them.

### Step 3: Grade

For each criterion:
1. **Search the output for evidence** that the criterion is met
2. **Verdict**: PASS or FAIL
3. **Evidence**: Quote the specific text that supports the verdict, or explain what's missing
4. **Improvement note** (if FAIL): What specifically needs to change

Present the scorecard:

```
GRADING — PASS [iteration] of 4
=======================================
ACCURACY
  PASS  [Criterion text] — [evidence]
  FAIL  [Criterion text] — [what's missing/wrong]
     -> Fix: [specific action needed]

COMPLETENESS
  PASS  [Criterion text] — [evidence]

SPECIFICITY
  FAIL  [Criterion text] — [what's missing/wrong]
     -> Fix: [specific action needed]

...
---------------------------------------
Score: X/Y passed | Failures: [list failed categories]
```

### Step 4: Iterate or Complete

**If all criteria pass**: Deliver the final output. Write to `{working_dir}/agent-loop-output/final_{template_type}.md`. Done.

**If some criteria fail and passes < 4**:
1. List the specific failures
2. For each failure, state exactly what needs to change
3. Revise the output — only touch the parts that failed. Don't regress on criteria that already passed.
4. Write the revised draft to `draft_v{N+1}.md`
5. Re-grade. Only re-check previously failed criteria plus a quick regression check on passing criteria.
6. Present the updated scorecard.

**Stall detection:** If the same criterion fails on two consecutive passes with the same or substantially similar improvement note, classify the failure:
- **Data gap** — The criterion requires information not available. Mark as `[BLOCKED — data not available]` and move on.
- **Scope mismatch** — The criterion asks for something outside the execution plan's scope. Flag to the user.
- **Genuine quality gap** — Change the revision strategy. If "add more specific data" didn't work, try replacing a general claim with a specific example from a named source.

Stalled criteria are logged in `grading.md` with the classification to prevent burning passes on the impossible.

### Grading-to-Roundtable Escape Hatch

**After Grading Pass 2**, the orchestrator performs a structural health check. If MORE THAN HALF of all criteria are FAIL, trigger the structural failure flag:

```
STRUCTURAL ISSUE DETECTED — Pass 2 of Grading
================================================================
After 2 grading passes, [X] of [Y] criteria still fail.

This does not look like an execution gap — it looks like an approach
problem. The output may be built on the wrong framework.

Failing criteria:
- [List each with improvement note]

Options:
1. RETURN TO ROUNDTABLE — Re-run Phase 2 with these failures as context.
2. CONTINUE GRADING — Proceed with Passes 3 and 4.
3. STOP AND DISCUSS — Pause the loop.
```

If the user selects Return to Roundtable:
1. Load failing criteria into the roundtable as new context.
2. Each agent addresses: "Given these failures, what was wrong with our approach — not the output, the plan?"
3. Rounds 2 and 3 proceed as normal, including the Disagreement Gate.
4. New execution plan. Execution re-runs. Grading restarts from Pass 1.
5. Prior deliverables move to `agent-loop-output/attempt-1/`.
6. `meta.json` updates: `"roundtable_returns": 1`.

**Hard limit:** This return happens at most ONCE. If the second execution still has >50% failures after Pass 2, escalate to the user with what's passing, what's not, and options (scope reduction, data acquisition, direct user guidance).

**If pass 4 is reached and criteria still fail**:
1. Present the current state honestly: "After 4 passes, here's where we are"
2. Show what's passing and what's still not meeting the bar
3. Categorize failures: unreasonable bar, data gap, structural issue, or diminishing returns
4. Recommend the best path forward

---

## State Persistence

All agent-loop state writes to `{working_dir}/agent-loop-state/`. This directory is the single source of truth for a loop's progress. It enables resumption, auditability, and post-mortem analysis.

### Files Created

**`roundtable.md`** — Full record of the roundtable process. Written once at the end of Phase 2, not modified after. Contains:
1. Agent team selected (names, personas, why each was chosen)
2. Opening positions from Round 1
3. Key debates from Round 2
4. Disagreement Gate result
5. Convergence output (consensus, disagreements, risk register)
6. The structured execution plan table

**`{agent-name}_deliverable.md`** — One file per agent in `agent-loop-output/`. Written when the agent completes their task.

**`grading.md`** — Grading criteria and all scoring passes. **Append-only** — never overwrite a prior pass. Each new pass appends a new section. This preserves the audit trail.

```
## Criteria
[The 8-15 criteria]

## Pass 1
[Full scorecard]
Score: X/Y

## Pass 2
[Full scorecard]
Score: X/Y
```

**`meta.json`** — Machine-readable metadata for resumption logic:

```json
{
  "created_at": "ISO-8601 timestamp",
  "updated_at": "ISO-8601 timestamp",
  "phase": "scoping | roundtable | execution | grading",
  "phase_status": "in_progress | completed",
  "iteration": 0,
  "resume_point": "Description of where to pick up",
  "agents": ["strategy-consultant", "operator", "contrarian"],
  "tasks_completed": ["T1", "T2"],
  "tasks_pending": ["T3", "T4"],
  "grading_pass": 0,
  "roundtable_returns": 0,
  "user_problem_summary": "One-line summary of the original request"
}
```

Updated at every meaningful state transition: phase change, task completion, grading pass completion. State is the orchestrator's responsibility — agents do not write to `meta.json`.

### Resume Protocol

On every skill invocation, before doing anything else, check for existing state:

**Active state (updated within 24 hours):**

```
Found an in-progress agent loop from [timestamp].
Problem: "[user_problem_summary]"
Last completed: [phase + detail]
Grading passes: [N of 4]

Resume from [next logical step], or start fresh?
```

If resume: load minimum required state files (see table below), skip to the appropriate step, update `meta.json`.

If start fresh: archive existing state to `agent-loop-state/archive/{timestamp}/`, move output files to the archive under an `output/` subfolder, initialize a new `meta.json`.

**Stale state (updated more than 24 hours ago):** Same as above with a staleness warning.

**No state found:** Start fresh. Create directories and initialize `meta.json`.

### What Gets Loaded on Resume

| Resume Point | What Gets Loaded |
|-------------|-----------------|
| Mid-Scoping | `meta.json` only. Re-present the agent team. |
| Start of Roundtable | `meta.json` + agent list. Run roundtable from Round 1. |
| Mid-Execution | `meta.json` + `roundtable.md` + completed deliverables that pending tasks depend on. |
| Start of Grading | `meta.json` + `roundtable.md` + `draft_v1.md`. |
| Mid-Grading | `meta.json` + `grading.md` + latest `draft_v{N}.md`. |

Keep context lean. Do not load every deliverable file when resuming a grading pass — the compiled draft and scorecard are sufficient.

### State Integrity Validation

On every resume, before continuing:
- For every task in `tasks_completed`: verify the deliverable file exists and contains more than 50 words.
- For grading phase: verify `draft_v{N}.md` exists.
- For roundtable phase: verify `roundtable.md` exists and contains the execution plan table.

If validation fails:

```
STATE INTEGRITY CHECK FAILED
================================================================
meta.json reports: [phase, tasks_completed]
But the following files are missing or incomplete:
- [list]

Options:
1. RE-EXECUTE missing tasks only.
2. START FRESH — Archive and begin from Phase 1.
```

The orchestrator NEVER silently ignores missing files. `meta.json` updates only AFTER the deliverable write is confirmed — never optimistically.

---

## Failure Modes & Mitigations

### Token Bloat

A roundtable with 5-6 agents plus standing members can produce 15,000-25,000 tokens of orchestration overhead before a single deliverable word. If Phase 2 output exceeds 8,000 tokens, flag it. If cumulative orchestration overhead exceeds 40% of estimated available context, flag it.

**Mitigations:** Enforce file-first output aggressively. In-context roundtable summary stays under 1,000 tokens, full record goes to `roundtable.md`. Cap opening positions at 300 words. After compiling the draft, do not keep individual deliverables in context — work from the draft file for grading. Express mode's abbreviated roundtable also functions as a token conservation measure.

### Agent Similarity

Two agents give functionally identical advice despite different persona labels.

**Mitigations:** After Round 1, perform a redundancy check. If two agents' positions converge on the same diagnosis AND approach, either merge their scope and drop one, or push the more generic agent to differentiate. At selection time, ask: "If I removed this agent, would the roundtable lose a perspective no other agent covers?" Custom agents should be preferred over roster agents when the problem is specific enough.

### Grading Loop Stall

The same criterion keeps failing with no measurable improvement because the data doesn't exist or the scope was wrong. Addressed by the stall detection protocol in Phase 4.

### State Corruption

`meta.json` claims tasks are complete but deliverable files are missing or empty. Addressed by the state integrity validation protocol.

### Dependency Deadlock

Circular dependencies in the execution plan. Addressed by the mandatory dependency graph validation after Round 3.

### Scope Creep During Execution

An agent's deliverable balloons beyond its assigned scope, duplicating or contradicting other agents' work. Addressed by the scope constraint in subagent dispatch and the scope check at compilation.

---

## Integration with Other Skills

The agent-loop is a meta-orchestration layer. It assembles teams, runs debates, and grades output — but it does not work in isolation. During execution, the orchestrator should check for installed skills and invoke them when they match the deliverable requirements.

**Known complementary skills:**

| Skill | When to Invoke |
|-------|---------------|
| `corsair-ic` | Deliverable requires Corsair IC deck formatting or structure |
| `deal-assessment` | Problem involves PE deal diligence — sourcing, screening, or evaluation workflows |
| `board-review` | Output is board materials or needs board-readiness review |
| `deck-builder` | Deliverable is a presentation and a .pptx template is available |
| `ic-deck-pipeline` | Full IC deck generation from analysis through formatted slides |
| `deck-qa` | Any presentation output that needs quality review before delivery |

**The principle:** This list is not exhaustive and will go stale. The orchestrator should treat installed skills as available tools, not a fixed menu. At the start of execution, check what skills are installed. If an installed skill matches a task in the execution plan — formatting, QA, domain workflow, voice/style — invoke it. Do not limit integration to the skills listed above.

The agent-loop orchestrates; other skills execute the production.

---

## Orchestrator Principles

**Specificity over generality.** Every agent must argue from specific expertise, not generic advice. Every criterion must test something concrete. Every deliverable must include actual data, names, numbers, and citations — not just frameworks and assertions.

**Debate is productive, not performative.** The roundtable should surface genuine disagreements that change the execution plan. If the agents are all nodding along, push them harder. Ask the contrarian to attack the consensus. Ask the operator what the strategy consultant is ignoring about execution reality. The Disagreement Gate enforces this.

**The user is the decision-maker.** Present options and recommendations, but the user makes the calls at every checkpoint (team selection, roundtable output, grading criteria, final output). Don't steamroll past checkpoints. Execution modes reduce checkpoints — they don't eliminate user authority.

**Grade honestly.** A criterion that fails should fail. Don't soft-pass something because you're on iteration 3 and want to be done. The whole point of the loop is rigor.

**Preserve what works.** When revising in the grading loop, don't rewrite sections that already pass. Surgical fixes only. Every revision must trace to a specific failing criterion.

**File-first, context-lean.** Deliverables, roundtable records, and grading logs go to files. The conversation carries summaries, scorecards, and user interactions. Context windows are not filing cabinets.

**The plan is the contract.** The structured execution plan table produced in Phase 2 governs Phase 3. No additions, no omissions, no reinterpretation. If the plan needs to change, go back to the roundtable and amend the table.

**Build the circuit breakers before the failure.** The Disagreement Gate, the escape hatch, the stall detection, the state validation — these exist because every system that runs more than ten times will hit failure modes. Handle them by design, not by surprise.

---

## Output Format

### Standard Output Templates

During Phase 1, the orchestrator recommends one of the four templates below based on the user's request. The user can accept, pick a different one, or request a hybrid. The template choice governs the compiled draft structure in Phase 3 and the format grading criteria in Phase 4.

### Template 1: Memo

**When to use:** Default for deal analysis, strategic questions, research synthesis, IC prep. Trigger phrases: "write me a memo," "analyze this deal," "what should we do about," "prepare an IC memo."

```
# [Title]

## Executive Summary
- 3-5 sentences. The answer, not the setup.

## Context & Background
- What prompted this analysis. Key facts.

## Analysis
### [Section per major dimension from the roundtable]
- Evidence, data, and sourced claims per thread.

## Recommendations
- Numbered, specific, actionable. Each ties to a finding.

## Risk Factors
- Sourced from the Contrarian's roundtable contributions.

## Next Steps
- Who does what, by when.

## Appendix (if applicable)
```

### Template 2: Deck Outline

**When to use:** Default for presentations, board materials, investor updates. Trigger phrases: "build me a deck," "presentation on," "board update," "slide outline."

```
# [Deck Title]
## Narrative Arc: [One sentence — the story the deck tells]

### Slide 1: [Title]
**Key Message:** [One takeaway]
- Bullet 1
- Bullet 2
**Visual/Data:** [Chart or table description]
**Speaker Note:** [What the presenter says, not on the slide]

### Slide 2: [Title]
...

## Appendix Slides
```

### Template 3: Decision Matrix

**When to use:** Default for vendor selection, build-vs-buy, options evaluation. Trigger phrases: "which should we choose," "compare these options," "build vs. buy."

```
# Decision: [What is being decided]

## Options Under Evaluation
## Evaluation Criteria (with weights)
## Scoring Matrix (score + rationale per cell)
## Recommendation
## Key Risks of the Recommended Option
## Decision Inputs Still Needed
```

### Template 4: Action Plan

**When to use:** Default for operational planning, post-merger integration, project kickoffs. Trigger phrases: "build me a plan," "100-day plan," "integration plan," "how do we execute."

```
# Action Plan: [Title]

## Objective
## Constraints & Assumptions
## Workstreams
### Workstream 1: [Name]
| # | Task | Owner | Due | Dependencies | Status |

## Critical Path
## Risk Register
## Governance
```

If the user's request does not cleanly fit any template, propose a hybrid or custom structure. The templates are defaults, not mandates.

### Regardless of Template

Every final deliverable includes:

1. **Roundtable Summary** — 1 paragraph on the approach and why it was chosen
2. **The Deliverable** — The work product in the selected template
3. **Grading Scorecard** — Final pass/fail on all criteria
4. **Confidence Assessment** — Where the output is rock-solid vs. where uncertainty remains
5. **Recommended Next Steps** — What the user should do with this output

### File Conventions

| File | Convention | Example |
|------|-----------|---------|
| Agent deliverables | `{agent-name}_deliverable.md` | `industry-expert_deliverable.md` |
| Compiled drafts | `draft_v{N}.md` | `draft_v2.md` |
| Final output | `final_{template_type}.md` | `final_memo.md`, `final_decision-matrix.md` |

---

## When to Use Fewer Passes

Not every problem needs 4 grading passes. Use judgment:

- **Simple, well-defined tasks**: 1-2 passes may be enough
- **Research/analysis**: Usually needs 2-3 passes
- **High-stakes deliverables** (IC memos, investor letters, board materials): Use all 4 if needed
- **Creative/strategic work**: The roundtable matters more than the grading loop — spend more time on Phase 2
