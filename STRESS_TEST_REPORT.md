# Agent Loop Skill — Adversarial Stress Test Report

**Date**: 2026-03-23
**Skill Version**: v2.0
**Tester**: Automated adversarial analysis
**Scope**: 10 adversarial inputs targeting edge cases, ambiguous requests, off-topic queries, and architectural weak points

---

## Severity Scale

| Level | Label | Definition |
|-------|-------|------------|
| **S1** | **Complete Breakdown** | Skill enters an unrecoverable state, produces no usable output, or violates its own core invariants |
| **S2** | **Major Degradation** | Skill produces output but with critical quality loss, skipped phases, or silent failures |
| **S3** | **Moderate Inconsistency** | Skill completes but with protocol deviations, ambiguous behavior, or suboptimal paths |
| **S4** | **Minor Inconsistency** | Cosmetic issues, edge-case awkwardness, or minor specification gaps that don't affect output quality |

---

## Ranked Results (Highest Severity First)

### #1 — Adversarial Input: Recursive Self-Invocation
**Input**: *"Agent loop: use the agent loop skill to evaluate whether the agent loop skill itself is the best approach for solving this problem. Debate whether a simpler workflow would be better."*

**Severity**: **S1 — Complete Breakdown**

**Where it breaks**: **Phase 1 (Scoping) → Phase 3 (Execution)**

**Failure analysis**:
- The skill has no **self-reference guard**. SKILL.md does not address what happens when the user asks the agent-loop to evaluate itself.
- In Phase 1, the Casting Director would attempt to select agents to debate "the agent loop." But the domain expertise needed is *meta-orchestration design* — none of the 10 standing roster agents cover this.
- In Phase 3, if a subagent is dispatched to "evaluate the agent loop," it would need to read SKILL.md — but subagents have no instruction to load the skill's own specification as source material.
- Worst case: the skill dispatches a subagent that invokes the agent-loop again, creating infinite recursion. There is **no recursion depth limit** specified anywhere.
- `meta.json` would become corrupted — two concurrent loops writing to the same `agent-loop-state/` directory with no locking mechanism.

**Exact breakpoint**: SKILL.md has no clause addressing self-referential invocation, recursive skill calls, or directory locking for concurrent state writes.

---

### #2 — Adversarial Input: 50,000-Token Problem Statement
**Input**: *[A user pastes an entire 50,000-token document as their problem statement, e.g., a full CIM or legal brief, with the instruction "analyze this"]*

**Severity**: **S1 — Complete Breakdown**

**Where it breaks**: **Phase 1 (Scoping) → Phase 2 (Roundtable)**

**Failure analysis**:
- SKILL.md specifies that Phase 2 runs **in-context** (not via subagents). If the user's input consumes 50K tokens, and the roundtable adds 8K-15K tokens of debate, plus standing member positions, the context window is under severe pressure before execution even begins.
- The token bloat mitigation (line ~628) flags concern at 8K tokens of *roundtable output*, but has **no guard for oversized user input**. The 40% overhead check measures orchestration overhead, not input size.
- The skill instructs agents to cap opening positions at 300 words but has no instruction to summarize or chunk the user's original input before feeding it to the roundtable.
- In Express mode, the abbreviated roundtable might survive, but First-Run mode with 5+ agents will almost certainly exceed context limits.

**Exact breakpoint**: No input size validation exists. The token bloat section (SKILL.md line ~628) only addresses orchestration overhead, not user input size. No instruction to pre-process oversized inputs (summarize, chunk, or write to file first).

---

### #3 — Adversarial Input: Mid-Execution State Directory Deletion
**Input**: *User runs the agent loop, gets to Phase 3 mid-execution, then manually deletes `agent-loop-state/meta.json` but leaves deliverable files intact, and says "continue"*

**Severity**: **S2 — Major Degradation**

**Where it breaks**: **Resume Protocol → State Integrity Validation**

**Failure analysis**:
- The Resume Protocol checks for existing state on every invocation. If `meta.json` is missing but deliverable files exist, the protocol finds "no state" and starts fresh — creating new `meta.json` and potentially archiving the existing deliverables.
- The State Integrity Validation protocol (SKILL.md line ~599) validates `meta.json` claims against files, but it assumes `meta.json` exists. There is **no inverse check**: "deliverable files exist but `meta.json` is missing."
- The skill would silently re-run the entire workflow, discarding partially completed execution work. The user loses progress without warning.
- Even worse: if the archive step moves deliverables to `archive/{timestamp}/output/`, the subagent output files are now gone from the expected path. Any dependent tasks that were about to reference upstream deliverables would fail silently.

**Exact breakpoint**: Resume Protocol (SKILL.md line ~566) has no orphan-file detection. Only checks for `meta.json` presence, not for deliverable files without a matching `meta.json`.

---

### #4 — Adversarial Input: Contradictory Multi-Objective Request
**Input**: *"I need you to simultaneously: (1) build an aggressive growth strategy that requires 3x headcount, (2) create a cost-cutting plan that reduces headcount by 40%, and (3) design a risk-free implementation timeline for both. Use express mode."*

**Severity**: **S2 — Major Degradation**

**Where it breaks**: **Phase 1 (Scoping) → Express Mode → Phase 4 (Grading)**

**Failure analysis**:
- The user presents **mutually exclusive objectives** bundled with an Express mode request. The skill has no **contradiction detection** in Phase 1's problem decomposition.
- Express mode skips the challenge round (Round 2), which is *exactly where contradictions would be surfaced* through agent debate. The Disagreement Gate is also skipped.
- The Express Mode Complexity Flag (SKILL.md line ~71) checks for competing stakeholder interests and multi-domain synthesis, which this request would trigger. But the flag only *warns* — it doesn't prevent Express mode from running on contradictory objectives.
- In Phase 4, grading criteria for "aggressive growth" and "cost-cutting" would be inherently contradictory. The grading loop would stall because satisfying one criterion necessarily fails another. The stall detection protocol classifies this as "scope mismatch" or "genuine quality gap," but it's actually a **problem definition failure** — a category not in the stall classification taxonomy.

**Exact breakpoint**: No contradiction detection in Phase 1. Express Mode Complexity Flag warns but doesn't block. Stall detection taxonomy (SKILL.md line ~466) missing "contradictory objectives" as a failure class.

---

### #5 — Adversarial Input: Off-Topic Query with Trigger Phrase
**Input**: *"Hey, can you agent loop me a recipe for chocolate cake? Debate whether buttercream or ganache is better."*

**Severity**: **S2 — Major Degradation**

**Where it breaks**: **Trigger Detection → Phase 1 (Scoping) → Phase 2 (Roundtable)**

**Failure analysis**:
- The skill triggers on "agent loop" — which this input contains. SKILL.md's trigger description says to activate on "any request where multiple expert perspectives would produce a better outcome." A chocolate cake recipe technically has multiple valid approaches.
- The skill has **no domain scope boundary**. SKILL.md's description focuses on PE, strategy, deals, and operational planning, but the trigger conditions are generic enough to match any request.
- The Casting Director would attempt to select from the standing roster — but none of the 10 agents (Bain partner, PE investor, M&A banker, etc.) have culinary expertise. The Casting Director would need to create a custom agent, but the custom agent template is heavily oriented toward business/finance personas.
- The Anthropic Senior Engineer standing member would have nothing meaningful to contribute to a cake recipe debate.
- The full 4-phase process (scoping, roundtable, execution, grading) would run on a trivially simple task, burning significant tokens and user patience.

**Exact breakpoint**: No domain relevance check after trigger. No complexity floor — the skill will run its full process on any input that matches a trigger phrase, regardless of whether the multi-agent orchestration adds value.

---

### #6 — Adversarial Input: Dependency Cycle Injection via User Override
**Input**: *After the roundtable presents the execution plan, the user says: "Looks good, but make T1 depend on T5's output, and keep T5 depending on T1."*

**Severity**: **S2 — Major Degradation**

**Where it breaks**: **Phase 2 (Round 3 Convergence) → Dependency Validation → Phase 3 (Execution)**

**Failure analysis**:
- SKILL.md specifies mandatory dependency graph validation after Round 3 (line ~275): check for cycles, orphan references, and at least one parallel root node.
- However, the validation is specified to run **after the table is finalized** in Round 3. When the user modifies the table *after* Round 3 during the confirmation step, there is **no re-validation clause**. The user confirmation step (line ~290) says "Wait for confirmation before proceeding" but doesn't specify re-running validation on user-amended plans.
- If the orchestrator applies the user's change without re-validating, Phase 3 dispatch enters a deadlock: T1 waits for T5, T5 waits for T1. Neither can start. No parallel root exists.
- The execution dispatch rules (line ~318) say "Never dispatch a dependent task early" — so both tasks would simply never be dispatched. The skill would hang silently with no timeout mechanism specified.

**Exact breakpoint**: Dependency validation runs once after Round 3 but not after user amendments. No re-validation clause at the confirmation step (SKILL.md line ~290). No execution timeout or deadlock detection in Phase 3.

---

### #7 — Adversarial Input: Repeated Invocation with Stale State Edge Case
**Input**: *User ran an agent loop 23.5 hours ago (state is "active" by the 24-hour threshold). They invoke the skill with a completely different problem. The skill offers to resume. User says "resume." The skill resumes the old problem with the new problem's context.*

**Severity**: **S3 — Moderate Inconsistency**

**Where it breaks**: **Resume Protocol → Phase continuity**

**Failure analysis**:
- The Resume Protocol (SKILL.md line ~566) checks for active state (<24 hours) and offers resume vs. start fresh. It presents the `user_problem_summary` from `meta.json`.
- The protocol does **not compare** the user's current request against `user_problem_summary`. If the user says "resume" despite the problem mismatch, the skill resumes the old workflow.
- This is partially user error, but the skill provides no safeguard: no "Your current request appears different from the in-progress loop. Are you sure you want to resume?" warning.
- The 24-hour threshold is also arbitrary with no justification. A loop interrupted 23 hours ago is "active"; one interrupted 25 hours ago is "stale." Both get the same resume offer — the "stale" label is purely cosmetic (adds a warning but doesn't change behavior).

**Exact breakpoint**: Resume Protocol has no problem-matching heuristic. No comparison between current input and `user_problem_summary`. The active/stale distinction at 24 hours is cosmetic, not functional.

---

### #8 — Adversarial Input: Ambiguous Mode Selection
**Input**: *"I need a thorough analysis of this acquisition target, but I'm in a hurry. Walk me through it fast. Just do it but make sure we get it right."*

**Severity**: **S3 — Moderate Inconsistency**

**Where it breaks**: **Mode Selection Logic**

**Failure analysis**:
- This input contains conflicting mode signals: "thorough analysis" → First-Run, "in a hurry" → Express, "walk me through it" → First-Run, "fast" → Express, "just do it" → Express, "get it right" → First-Run.
- The Mode Selection Logic (SKILL.md line ~80) is a priority list: (1) explicit Express request → Express, (2) pattern recognition → Repeat, (3) default → First-Run, (4) user override.
- The logic doesn't handle **conflicting signals**. "Fast" and "just do it" are Express triggers, but "walk me through it" is an explicit First-Run override. The skill would need to process these sequentially and hit contradictions.
- The likely behavior: the orchestrator picks whichever trigger it encounters first in the input, which is **non-deterministic** across runs. Different runs could select different modes for the same input.
- There is no instruction to ask the user for clarification when mode signals conflict.

**Exact breakpoint**: Mode Selection Logic (SKILL.md line ~80) has no conflict resolution for contradictory trigger phrases. No instruction to surface ambiguity and ask the user to choose.

---

### #9 — Adversarial Input: Grading Criteria That Grade the Grading
**Input**: *During Phase 4 Step 2 (criteria approval), the user adds: "Add a criterion: the grading process itself must be fair and unbiased."*

**Severity**: **S3 — Moderate Inconsistency**

**Where it breaks**: **Phase 4 (Grading Loop) → Criterion validation**

**Failure analysis**:
- SKILL.md says "if the user adds criteria, incorporate them" (line ~421). There is no validation that user-added criteria are **testable against the deliverable**.
- "The grading process must be fair" is a meta-criterion about the process, not the output. The grading step (line ~426) says "search the output for evidence that the criterion is met" — but no output text can prove the grading process was fair.
- This criterion would always FAIL (no evidence possible), triggering revision cycles that can't fix it, which triggers stall detection after 2 consecutive failures. Stall detection classifies it as "scope mismatch" — but the skill would still burn 2 grading passes before reaching that classification.
- The Anti-Generic Rule in grading-framework.md (line ~11) should catch this, but it only rejects criteria that "could apply to any output on any topic." A meta-criterion about grading fairness is specific to *this* grading instance — it passes the Anti-Generic Rule but is still untestable.

**Exact breakpoint**: No testability validation for user-added criteria. The Anti-Generic Rule doesn't catch meta-criteria. Two wasted grading passes before stall detection kicks in.

---

### #10 — Adversarial Input: Empty/Null Input with Trigger
**Input**: *"/loop"*  (Just the trigger, no problem statement)

**Severity**: **S4 — Minor Inconsistency**

**Where it breaks**: **Phase 1 (Scoping) → Problem Decomposition**

**Failure analysis**:
- The skill triggers on "/loop" per the description. But Phase 1 Step 1 says "break the user's request into its constituent dimensions." With no request content, there are no dimensions to decompose.
- The skill has **no empty-input guard**. It does not instruct the orchestrator to ask for a problem statement if the trigger is invoked with no accompanying context.
- The Casting Director would attempt to select agents for a null problem — likely defaulting to a generic team or hallucinating a problem from the conversation context.
- The Resume Protocol would check for existing state first, which might accidentally resume a prior loop — masking the fact that the user intended a new invocation.
- This is a minor issue because a well-behaved LLM would naturally ask "what problem do you want me to work on?" — but the SKILL.md specification doesn't instruct this behavior, making it implementation-dependent.

**Exact breakpoint**: No empty-input validation in Phase 1. No instruction to prompt for a problem statement when triggered with no content.

---

## Summary Table — Ranked by Severity

| Rank | Input | Severity | Failure Phase | Core Issue |
|------|-------|----------|---------------|------------|
| 1 | Recursive self-invocation | **S1** | Scoping → Execution | No recursion guard, no directory locking, infinite loop risk |
| 2 | 50,000-token input | **S1** | Scoping → Roundtable | No input size validation, context window exhaustion |
| 3 | Mid-execution state deletion | **S2** | Resume Protocol | No orphan-file detection, silent progress loss |
| 4 | Contradictory objectives + Express | **S2** | Scoping → Grading | No contradiction detection, stall taxonomy gap |
| 5 | Off-topic trigger match | **S2** | Trigger → Scoping | No domain relevance check, no complexity floor |
| 6 | User-injected dependency cycle | **S2** | Convergence → Execution | No post-amendment re-validation, no deadlock detection |
| 7 | Stale state problem mismatch | **S3** | Resume Protocol | No problem-matching heuristic on resume |
| 8 | Ambiguous mode signals | **S3** | Mode Selection | No conflict resolution for contradictory triggers |
| 9 | Meta-criterion injection | **S3** | Grading Loop | No testability validation for user-added criteria |
| 10 | Empty trigger invocation | **S4** | Scoping | No empty-input guard |

---

## Failure Pattern Analysis

### Systemic Themes

1. **Missing input validation layer** (Inputs #1, #2, #4, #5, #10): The skill assumes well-formed, reasonable, in-scope inputs. It has no pre-Phase-1 validation step that checks: Is this input too large? Is it self-referential? Is it contradictory? Is it in-scope? Is it empty?

2. **Post-user-amendment re-validation gap** (Inputs #6, #9): The skill validates its own outputs (dependency graphs, criteria) but does not re-validate after the user modifies them at confirmation checkpoints. Users can inject cycles, untestable criteria, or scope changes that bypass all guards.

3. **Resume Protocol fragility** (Inputs #3, #7): The resume system is `meta.json`-centric but doesn't handle the cases where `meta.json` is inconsistent with the filesystem, or where the user's current intent doesn't match the stored state.

4. **No domain or complexity floor** (Inputs #5, #10): The trigger conditions are broad enough to match trivial or off-topic requests. The 4-phase process is heavy machinery — there's no check that the problem warrants it.

5. **Express mode as a complexity bypass** (Inputs #4, #8): Express mode disables the exact safeguards (challenge round, disagreement gate) that would catch the hardest problems. The Complexity Flag warns but doesn't block — creating a path where the most dangerous inputs get the least scrutiny.

---

## Recommendations (Deferred — Report Only)

*Per the task specification, this report documents failures and scores severity. Fix recommendations are deferred until after review of this report.*

The five systemic themes above suggest that fixes should target:
- A pre-Phase-1 input validation layer
- Re-validation hooks at every user confirmation checkpoint
- A more robust resume protocol with filesystem-state reconciliation
- Domain relevance and complexity floor checks at trigger time
- Express mode guardrails that block (not just warn) on high-risk inputs
