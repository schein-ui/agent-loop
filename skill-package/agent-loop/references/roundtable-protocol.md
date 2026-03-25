# Roundtable Protocol

> This file extends SKILL.md Phase 2 (Roundtable) with deeper tactical guidance on facilitating productive debates, recognizing genuine vs. synthetic disagreement, and running quality self-checks. It does not restate the round structure, Disagreement Gate mechanics, execution plan format, or dependency validation — those live in SKILL.md.

---

## Standing Members: Conditional Presence

### Anthropic Senior Engineer

**Required when:** The deliverable involves code, tool usage, Claude Code capabilities, technical architecture, API design, data pipelines, or any implementation that runs on infrastructure.

**Optional when:** The problem is pure strategy, creative, analytical, or operational — and no part of the execution plan involves building, configuring, or deploying anything technical.

**When excluded, state it explicitly:** "The Anthropic Senior Engineer is not required for this roundtable — the problem is [strategy/creative/analytical] with no technical implementation. If technical feasibility becomes relevant during the debate, I'll add them."

The engineer's veto authority on technical feasibility still applies when they are present. When absent, the orchestrator takes responsibility for flagging obvious technical infeasibility (e.g., "this plan requires reading 500 pages in a single pass").

---

## Facilitating Good Debates

The orchestrator's job in Round 2 is not to summarize — it is to create productive collisions between positions. This section provides tactical guidance for doing that well.

### The Collision Principle

A productive debate has at least one moment where resolving a disagreement changes the execution plan. If the debate could be deleted without altering the plan, it was performative.

The orchestrator should drive toward these collisions by identifying the highest-stakes disagreement and forcing direct engagement:

**Pattern: The Direct Challenge**
- "Agent X, the [Role] just said [specific claim]. You clearly disagree. What specifically is wrong with their reasoning?"
- Not: "Agent X, what do you think about Agent Y's position?" (too soft — invites summary, not challenge)

**Pattern: The Assumption Probe**
- "That recommendation assumes [specific assumption]. Agent Z, does that assumption hold in your experience? What happens if it's wrong?"
- Not: "Are there any risks?" (too vague — invites generic risk lists)

**Pattern: The Consequences Fork**
- "If we go with Agent A's approach, what's the worst realistic outcome? And if we go with Agent B's, what do we lose?"
- Forces agents to evaluate trade-offs concretely, not abstractly.

**Pattern: The Evidence Demand**
- "You've both stated your positions. Agent A, name one specific case where your approach worked. Agent B, name one where it failed."
- Grounds the debate in real-world evidence instead of theoretical preferences.

### Domain-Specific Debate Examples

**Product debate (genuinely productive):**
> *Product Lead:* "We should launch with a minimal feature set — 3 core features — and iterate based on user feedback."
> *Engineering Lead:* "If we ship without the integration API, enterprise customers can't evaluate us. We'll get consumer feedback but miss the segment that pays."
> *Design Lead:* "The consumer experience IS the enterprise pitch. A polished 3-feature product sells better than a rough 10-feature one."
> **The collision:** Does the MVP need the integration API? Resolving this one way produces a 6-week launch; resolving it the other produces a 12-week launch with a different target segment. The execution plan changes.

**Creative debate (genuinely productive):**
> *Brand Strategist:* "The campaign should lead with the product's technical superiority. That's the real differentiator."
> *Creative Director:* "Technical specs don't move people. Lead with the emotional outcome — what life looks like with this product."
> *Customer Researcher:* "Our data shows the purchase trigger is social proof, not features or emotions. Lead with adoption numbers and testimonials."
> **The collision:** Three different campaign architectures. The execution plan assigns entirely different creative assets depending on which thesis wins.

**Research debate (genuinely productive):**
> *Domain Expert:* "The literature strongly supports intervention X. The meta-analyses are clear."
> *Methodologist:* "Those meta-analyses are dominated by two large studies with known design flaws. Strip those out and the effect size drops below clinical significance."
> *Practitioner:* "Effect size aside, practitioners report that X works in combination with Y but not alone. The studies don't test the combination."
> **The collision:** Does the research synthesis recommend X, recommend X+Y, or flag X as insufficiently supported? Each leads to a different recommendation section.

**Operational debate (genuinely productive):**
> *Operations Lead:* "We should migrate all systems simultaneously over a long weekend. Clean cut, minimal parallel-running costs."
> *Risk Manager:* "Big-bang migrations have a 40% failure rate in comparable environments. Phase it over 8 weeks."
> *Finance Lead:* "Eight weeks of parallel running costs $200K. The big-bang risk is cheaper than the certain cost of phasing."
> **The collision:** The migration plan is fundamentally different — 3 days vs. 8 weeks, $0 parallel cost vs. $200K, high operational risk vs. high financial cost. The execution plan can't be both.

### Avoiding Performative Debate

Not every debate produces insight. The orchestrator should recognize and short-circuit these anti-patterns:

**Anti-pattern: The Polite Rephrase**
Agent B "disagrees" with Agent A by saying the same thing in different words. Test: if you resolve the "disagreement" either way, does the execution plan change? If not, it's not a disagreement.

**Anti-pattern: The Scope Inflation**
An agent introduces a new dimension ("we should also consider X") rather than challenging an existing position. New dimensions belong in Round 1. Round 2 is for testing the positions already stated.

**Anti-pattern: The Hedge Stack**
Instead of taking a position, an agent lists caveats: "it depends on market conditions, regulatory environment, competitive response..." This is risk-listing, not debating. Push: "Given your best estimate of those conditions, which approach do you endorse?"

**Anti-pattern: The Authority Appeal**
"In my experience, this approach works." Without specifying the conditions, the context, or the evidence, experience is not an argument. Push: "In what specific situation did it work, and how similar is that situation to this one?"

---

## Genuine vs. Synthetic Disagreement

The Disagreement Gate (defined in SKILL.md) requires substantive disagreement before proceeding. But not all disagreement is created equal. The orchestrator must distinguish between:

### Genuine Disagreement

Agents disagree because the problem has real tension — competing objectives, uncertain data, legitimate trade-offs. Indicators:

- **Different diagnoses from different data.** Agents interpret the same evidence differently because their frameworks prioritize different signals.
- **Incompatible recommendations.** Doing what Agent A recommends makes Agent B's recommendation impossible or unnecessary.
- **Structural trade-offs.** Speed vs. thoroughness, cost vs. quality, breadth vs. depth — real constraints that force a choice.
- **The "both could be right" test.** If smart, informed people could look at the same facts and reach different conclusions, the disagreement is genuine.

### Synthetic Disagreement

Agents disagree because the Disagreement Gate incentivizes it. Indicators:

- **Manufactured edge cases.** "But what if [extremely unlikely scenario]?" Unlikely scenarios are valid risks, not valid disagreements — they belong in the risk register, not the debate.
- **Disagreement on framing, not substance.** "I'd call it a 'growth strategy' not a 'market expansion plan'" is not a disagreement. Test: does the label change the execution plan?
- **The Contrarian stretch.** When the Contrarian's objection starts with "Well, to play devil's advocate..." and the counter-position is obviously weaker than the consensus, the disagreement is performative.
- **Confidence without stakes.** An agent disagrees but wouldn't change their own deliverable based on the outcome. If the resolution doesn't affect their work, they don't actually care — they're generating content, not arguing a position.

### What to Do When Disagreement Is Synthetic

If the orchestrator detects synthetic disagreement after the gate fails and a retry has been attempted:

1. Use Option C (Simplicity Acknowledgment) without hesitation. Consensus among diverse experts is signal, not failure.
2. Log it: "Round 2 disagreement assessed as synthetic. Proceeding to convergence on the consensus position."
3. Do not manufacture additional debate rounds. Synthetic disagreement wastes tokens and degrades the execution plan with unnecessary hedging.

### What to Do When Convergence Is Genuine

Sometimes all agents agree because the answer is clear. This is not a bug. Indicators that convergence is legitimate:

- Agents arrive at the same conclusion through **different reasoning paths** (not just agreeing, but independently deriving the same answer from different frameworks).
- The problem has a dominant strategy that experts in the field would recognize.
- The user's constraints are tight enough that there is really only one viable approach.

When convergence is genuine, the roundtable's value shifts from "finding the right approach" to "confirming confidence and surfacing risks." The risk register becomes the primary output of the debate.

---

## Debate Quality Self-Check

**When:** After Round 2, before the Disagreement Gate evaluation.

The orchestrator assesses whether the debate earned its token cost:

### The Three Questions

1. **Did the debate change the execution plan?**
   - If YES: The debate was productive. Proceed to the Disagreement Gate.
   - If NO: Ask question 2.

2. **Did the debate surface a risk or assumption that wasn't in anyone's Round 1 position?**
   - If YES: The debate was productive even without changing the plan. The new insight goes to the risk register.
   - If NO: Ask question 3.

3. **Did any agent change their position based on another agent's challenge?**
   - If YES: The debate tested and confirmed the approach. Productive.
   - If NO: The debate was performative. Log it and proceed to convergence without forcing additional rounds.

### Logging the Self-Check

```
## Debate Quality Assessment
- Plan changed by debate: YES / NO — [what changed]
- New risks or assumptions surfaced: YES / NO — [list]
- Agent positions shifted: YES / NO — [which agent, what shifted]
- Assessment: PRODUCTIVE / CONFIRMATORY / PERFORMATIVE
```

If the assessment is PERFORMATIVE on two consecutive runs of the same problem type, the orchestrator should note in `agent-loop-state/criteria_log.md`: "This problem type may not benefit from full roundtable debate. Consider Express mode for future similar requests."

---

## Roundtable Facilitation by Problem Type

Different problem types benefit from different facilitation emphasis. The round structure doesn't change, but where the orchestrator pushes hardest does.

### Strategy / Planning Problems
**Push hardest on:** Diagnosis disagreement. Strategy problems have the most room for reframing. If all agents accept the same problem definition, the debate will be shallow.
**Common trap:** Agents debate tactics without questioning the strategic frame. Force the question: "Are we solving the right problem?"

### Analytical / Research Problems
**Push hardest on:** Methodology disagreement. The conclusions follow from the method. If agents disagree on conclusions but use the same method, the disagreement is cosmetic. If they disagree on method, the conclusions will genuinely differ.
**Common trap:** Agents cite different sources but don't evaluate source quality. Force the question: "Which data do you trust more, and why?"

### Creative Problems
**Push hardest on:** Audience interpretation. Creative disagreements often mask different assumptions about who the audience is and what they value.
**Common trap:** Subjective preferences disguised as expertise ("I think the tone should be warmer"). Force the question: "What evidence do you have that the audience responds to that tone?"

### Operational / Implementation Problems
**Push hardest on:** Sequencing and risk. Operations experts rarely disagree on what needs to happen — they disagree on the order, the timeline, and what goes wrong.
**Common trap:** Over-planning without prioritizing. Force the question: "If we could only get 3 of these 8 things right, which 3?"

### Technical / Architecture Problems
**Push hardest on:** Trade-offs between approaches. Technical debates can spiral into theoretical comparisons without grounding in the specific constraints.
**Common trap:** Debating tools instead of outcomes. Force the question: "What does the user experience look like if we go with your approach vs. theirs?"

---

## Post-Roundtable Handoff Quality

The roundtable's output must be rich enough that subagents can execute without losing the reasoning behind the plan. This section supplements SKILL.md's subagent dispatch instructions.

### Task-Specific Briefings

The 1,000-token roundtable summary provides orientation, but each subagent also needs a **task-specific briefing** — the 2-4 paragraphs from the roundtable most relevant to their specific deliverable.

Before dispatching each subagent, the orchestrator reads `roundtable.md` and extracts:
- The debate points that shaped this task's approach
- The risks most relevant to this task
- Any constraints or decisions from convergence that this agent must respect
- What the other agents' work will contain (so this agent can set up cross-references correctly)

This adds 500-800 tokens per subagent but preserves the reasoning that matters.

### Structuring roundtable.md for Extraction

To make task-specific extraction mechanical rather than interpretive, `roundtable.md` should include a section after the execution plan table:

```
## Task-Specific Context

### T1: [Deliverable Name]
- Key debate context: [what roundtable discussion shaped this task]
- Relevant risks: [from risk register]
- Cross-references to expect: [what other tasks will produce that T1 should connect to]

### T2: [Deliverable Name]
...
```

This structure allows the orchestrator to extract each task's context by heading, not by interpretation.

---

## Express Mode Roundtable Adjustments

In Express mode, the roundtable runs opening positions only — no challenge round, no multi-round debate.

What changes in this file's guidance:
- The Debate Quality Self-Check is skipped (there is no Round 2).
- The genuine vs. synthetic disagreement assessment is skipped.
- The Disagreement Gate is skipped.
- Stall detection and the grading-to-roundtable escape hatch are inactive (both require multiple passes).
- Facilitation by problem type still applies to Round 1 — the orchestrator uses these lenses when framing the problem, even without a debate.
- Task-specific briefings are still extracted from opening positions and convergence.
- The post-Round 1 redundancy check still runs.

Express mode changes the depth of the debate, not the quality of the handoff.
