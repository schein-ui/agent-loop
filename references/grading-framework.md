# Grading Framework

> This file extends SKILL.md Phase 4 (Grading Loop) with deeper detail on criteria generation, letter-grade calibration, cross-agent consistency, and the shared assumptions protocol. It does not restate the phase flow, scorecard format, iteration rules, or escape hatch — those live in SKILL.md.

---

## Letter Grade Scale

Every criterion receives a letter grade. The composite deliverable grade is derived from the individual criterion grades.

| Grade | Meaning | Action |
|-------|---------|--------|
| **A+** | Exceptional — exceeds the quality bar. Could be presented to the most demanding audience without caveat. | Ready to deliver. |
| **A** | Strong — fully meets the quality bar with substance and specificity. | Ready to deliver. |
| **A-** | Solid — meets the bar with minor polish opportunities that don't affect substance. | Ready to deliver. Flag polish items but do not iterate. |
| **B+** | Good — meets most of the bar but one element is thin or could be sharper. | One more targeted pass on the weak element. |
| **B** | Adequate — the right structure and direction, but 2-3 elements lack the depth or specificity the bar demands. | One more pass with specific improvement notes. |
| **B-** | Below standard — the approach is correct but execution is substantively incomplete. Multiple gaps in evidence, specificity, or accuracy. | Structural revision needed. Re-examine the execution plan for this section. |
| **C+** | Weak — partially addresses the criterion but misses key dimensions or relies on generic assertions instead of evidence. | Major revision. May require re-dispatch of the responsible agent. |
| **C** | Poor — technically addresses the topic but adds little value. Reads like filler. | Re-dispatch with augmented prompt, or escalate to user. |
| **C-** | Very poor — present but substantively wrong, misleading, or internally contradictory. | Re-dispatch required. Do not iterate on this content — rebuild it. |
| **D** | Failing — token effort. The criterion is nominally addressed but the content would not survive any scrutiny. | Re-dispatch or flag as blocked. |
| **F** | Missing or completely off-target — the criterion is not addressed at all, or the content is on a different topic. | Re-dispatch or scope change required. |

### Computing the Composite Grade

1. Grade each criterion individually using the scale above.
2. Convert to numeric: A+=4.3, A=4.0, A-=3.7, B+=3.3, B=3.0, B-=2.7, C+=2.3, C=2.0, C-=1.7, D=1.0, F=0.
3. Compute the unweighted mean across all criteria.
4. Convert back to a letter grade using the same thresholds.
5. Apply a **floor rule**: if ANY criterion scores C- or below, the composite cannot exceed B regardless of the mean. A single severe failure caps the deliverable.

### Grade Thresholds for Action

| Composite Grade | What Happens |
|----------------|--------------|
| **A- or above** | Deliver. The output is ready. |
| **B+ or B** | One more pass targeting criteria below A-. |
| **B- or below** | Structural revision. The orchestrator examines whether the issue is execution quality or approach quality. If more than half of criteria are B- or below, trigger the escape hatch (defined in SKILL.md). |

### Pass 1 Early Warning

After Pass 1 grading, before proceeding to revisions, check for early structural failure:

- **Trigger:** More than 60% of criteria score B- or below on Pass 1.
- **Action:** Fire an early structural warning identical to the Pass 2 escape hatch — present the user with options: return to roundtable (once only), continue grading, or stop and discuss.
- **Rationale:** If the output is fundamentally broken, waiting for a full revision cycle and a second grading pass wastes effort. This catches structural failures one pass earlier.
- **Relationship to Pass 2 escape hatch:** The Pass 2 escape hatch (defined in SKILL.md) remains active and unchanged. It catches outputs that degrade during revision or that were marginal on Pass 1 but fail to improve. The Pass 1 Early Warning catches outputs that are clearly broken from the start.

If the user chooses to continue after the Pass 1 Early Warning, proceed with revisions as normal. The Pass 2 escape hatch still applies independently.

---

## Shared Assumptions Protocol

> The full Shared Assumptions Protocol (extraction, conflict resolution, deliverable updates) is defined in SKILL.md Phase 2. This section covers only the grading-side complement: verifying that the protocol was applied correctly via the Cross-Agent Consistency criterion category below.

---

## Criterion Categories

Generate criteria across these dimensions. The category list below extends the summary table in SKILL.md with deeper guidance on what good vs. bad criteria look like in each category.

### Accuracy (2-3 criteria)

Tests whether facts, figures, and claims are correct.

**What to test:**
- Internal consistency — do subtotals add up? Do percentages sum to 100% where they should?
- External verifiability — can claims be traced to source materials or citations?
- Logical soundness — are there contradictions between the analysis and the recommendations?

**Calibration anchors:**
- **A:** Every figure is internally consistent and traceable to a named source. No logical contradictions.
- **B:** Figures are internally consistent but 1-2 claims lack citations. No contradictions.
- **C:** Multiple uncited claims. Minor internal inconsistencies (e.g., a percentage that doesn't match the underlying numbers).
- **F:** Contradictory claims within the same section, or fabricated figures presented as factual.

**Domain-neutral examples of good criteria:**
- *Product:* "Unit economics section shows CAC, LTV, and payback period that are mathematically consistent with the assumptions table. Changing an assumption updates the downstream figures."
- *Creative:* "Brand positioning claims ('fastest-growing,' 'most trusted') are each backed by a specific data point with date and source."
- *Research:* "Every statistical claim includes sample size, methodology, and confidence interval (or flags their absence)."
- *Operations:* "The resource plan's FTE counts, fully-loaded costs, and total budget are arithmetically consistent."

### Completeness (2-3 criteria)

Tests whether the output addresses every element from the execution plan.

**What to test:**
- Coverage — does each task from the execution plan table appear in the output?
- Gap detection — are there dimensions the roundtable identified as important that the output skips?
- Agent contribution — did any agent's deliverable get lost or under-represented in compilation?

**Calibration anchors:**
- **A:** Every task's deliverable is fully represented. All roundtable-identified dimensions are covered.
- **B:** All tasks represented, but one dimension is thinner than the execution plan specified.
- **C:** A task's deliverable is present but substantially reduced from its quality bar. Or a full dimension is covered only superficially.
- **F:** An entire task's output is missing from the compiled draft.

### Specificity (2-3 criteria)

Tests whether the output contains actual substance vs. abstract assertions.

**What to test:**
- Named entities — does the output use real names, companies, products, dates?
- Quantification — are claims backed by numbers, not just directional language ("significant," "substantial")?
- Actionable precision — could someone execute the recommendations without needing to ask clarifying questions?

**Calibration anchors:**
- **A:** Recommendations name specific actions, owners, timelines, and expected outcomes. Analysis cites real entities and data.
- **B:** Most recommendations are specific, but 1-2 fall back to directional language ("consider expanding into adjacent markets").
- **C:** The structure is right but the content reads like a template. Replace any company name with another and the analysis still reads the same.
- **F:** The output is entirely generic platitudes. "Leverage synergies to drive value creation."

**Domain-neutral examples of good criteria:**
- *Product:* "Each feature recommendation includes a user story, estimated development effort (S/M/L), and the metric it's expected to move."
- *Creative:* "The creative brief specifies target audience by demographic, psychographic, and behavioral attributes — not just 'millennials' or 'professionals.'"
- *Research:* "Each finding names the studies, their sample sizes, and the specific conditions under which the finding holds."
- *Operations:* "The migration plan specifies each system by name, the migration sequence, rollback procedure, and success criteria."

### Evidence (1-2 criteria)

Tests whether claims are supported.

**Calibration anchors:**
- **A:** Every major claim cites a specific source (document, page, URL, data table). The reader can verify independently.
- **B:** Most claims are sourced. 1-2 rely on the analyst's judgment, flagged as such.
- **C:** Claims are asserted without sources. The analysis reads as opinion, not evidence-based.
- **F:** Claims contradict the provided source materials.

### Coherence (1-2 criteria)

Tests whether the output holds together as a unified work product.

**Calibration anchors:**
- **A:** The executive summary accurately reflects the body. Sections build on each other logically. Recommendations follow from the analysis.
- **B:** Minor disconnects — a finding mentioned in one section isn't picked up in the recommendations.
- **C:** The executive summary and the body tell different stories. Or two sections contradict each other.
- **F:** The output reads like fragments from different analyses stitched together.

### Cross-Agent Consistency (1-2 criteria) — MANDATORY

Tests whether deliverables from different agents are internally consistent.

**This category is required on every grading run.** It is the grading-side complement to the Shared Assumptions Protocol above.

**What to test:**
- Do sections authored by different agents cite the same key figures?
- Do recommendations from different sections align or contradict?
- If Agent A identifies a risk, does Agent B's plan account for it?

**Calibration anchors:**
- **A:** All cross-agent figures match. Recommendations are complementary. Risks identified by one agent are addressed in another's plan.
- **B:** Figures match (or the Shared Assumptions Protocol resolved them), but one recommendation pair is in mild tension without acknowledgment.
- **C:** Unresolved numerical contradictions remain in the final draft. Or one agent's recommendations directly undermine another's without discussion.
- **F:** The draft contains two contradictory conclusions on the same question, each from a different agent, with no reconciliation.

### Actionability (1-2 criteria)

Tests whether the output is usable.

**Calibration anchors:**
- **A:** A decision-maker could act on this immediately. Next steps are clear, owners are named, timelines are set.
- **B:** The decision is clear but 1-2 implementation details require follow-up.
- **C:** The analysis is sound but doesn't resolve into a clear action. The reader knows more but doesn't know what to do.
- **F:** No clear recommendation or next step. The output is informational, not decisional.

---

## The Anti-Generic Rule

Every criterion must be specific to THIS deliverable. A criterion that would pass on a mediocre output is useless. A criterion that could apply to any output on any topic needs rewriting.

**Test:** Read the criterion. Could you grade it without knowing what the deliverable is about? If yes, rewrite it.

**Bad criteria** (would apply to anything):
- "The analysis is thorough"
- "Recommendations are actionable"
- "The output is well-organized"

**Good criteria** (specific to the task):
- *Deal analysis:* "The EBITDA bridge shows the walk from reported to adjusted EBITDA with each add-back individually identified and sourced to a specific page in the CIM"
- *Product strategy:* "The competitive feature matrix includes all 4 competitors named in the roundtable, with pricing tiers and the specific feature gaps that create differentiation opportunities"
- *Research synthesis:* "Each of the 3 research questions from the execution plan has a dedicated section with at least 5 primary sources published within the last 2 years"
- *Creative brief:* "The brand voice section includes 3 'do' examples and 3 'don't' examples, each drawn from the brand's actual content or competitors' content"
- *Operational plan:* "The critical path identifies the 3 longest-lead-time workstreams, with specific vendor names, lead times in weeks, and the date by which each must be initiated to hit the launch target"

---

## Criteria Count Guidance

Generate **1-2 criteria per task in the execution plan**, plus **1-2 cross-cutting criteria** (coherence, cross-agent consistency, actionability). Typical range: 8-12 criteria. Never exceed 15.

This makes the criteria count a function of the plan's complexity, not an arbitrary choice. A 4-task plan produces 8-10 criteria. A 7-task plan produces 12-15.

---

## Calibration Guidance

LLMs tend toward grade inflation. The following anchors help maintain consistent grading across runs.

### What an A Looks Like

The output would survive scrutiny from the most demanding expert in the room. A senior decision-maker could act on it without requesting revisions. Every claim has substance behind it. The reader learns something they didn't know. The recommendations are specific enough that two different teams would execute them the same way.

An A is not "no errors." It is "substantively excellent, with evidence and specificity at every turn."

### What a B Looks Like

The output is correct and useful, but has visible gaps. A decision-maker could act on it but would have follow-up questions. Some sections are strong; others feel like first-draft quality that wasn't fully developed. The structure is right but the substance is uneven.

A B is not bad work. It is work that would benefit from one more focused pass.

### What a C Looks Like

The output addresses the right topic but adds limited value beyond what the reader could have produced themselves. Recommendations are directional, not specific. Evidence is asserted, not cited. The output reads like a competent summary, not an expert analysis.

A C means the approach may be right but the execution is not there yet. This is where the orchestrator considers whether the issue is the execution or the plan.

### What a D/F Looks Like

The output either misses the target entirely (F) or makes a token effort that would not survive any scrutiny (D). At this level, iteration is unlikely to help — the issue is structural, not editorial. The orchestrator should consider re-dispatch or scope change.

### Grading Honesty Protocol

Before assigning any grade above B+, the grader must pass this self-check:

1. **Evidence test:** Can I quote specific text from the output that earns this grade? If the justification is "it covers the topic well" without pointing to specific passages, downgrade.
2. **Substitution test:** If I replaced the company/product/topic names with different ones, would the content still make sense and be accurate? If yes, the output is generic — cap at B.
3. **Action test (for A/A+ only):** Could someone who reads only this section take a specific, concrete action? If they'd need to ask "but what exactly should I do?", cap at B+.

---

## Regression Prevention

After revising to address failing criteria, re-evaluate every previously passing criterion that shares a section with a revised section. If the revision touched the competitive analysis, re-check all passing criteria whose evidence was drawn from that section. Do not re-check criteria from untouched sections.

If a previously passing criterion now fails due to a revision, that is a regression. Fix the regression before re-grading the originally failing criteria.

## Context Overflow During Grading

If context pressure exceeds 80% during the grading loop:
1. Write the current scorecard and all pending revision notes to `agent-loop-state/grading.md`
2. Summarize remaining criteria as "not yet graded — deferred due to context pressure"
3. Switch any remaining grading passes to Express (single pass, no re-grade)
4. Inform the user: *"Context pressure hit [X]% during grading. Wrote partial scores to grading.md. Remaining criteria graded in Express mode."*

This prevents silent quality degradation when the grading loop runs long on complex deliverables.

---

## Criteria Evolution

After each completed loop, append criteria evolution notes to `agent-loop-state/criteria_log.md`:

```
## Run: [date] — [problem summary]
- Criteria that were too easy (passed on mediocre output): [list]
- Criteria that were impossible to satisfy: [list + reason]
- Missing quality dimensions that should be added next time: [list]
- Cross-agent consistency issues encountered: [list]
```

On future runs, the orchestrator reads this file during criteria generation to incorporate lessons from prior runs.
