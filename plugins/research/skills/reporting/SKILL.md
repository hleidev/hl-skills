---
name: reporting
description: "Structured external-information research that produces a decision-grade report — technology comparisons, market research, academic topics, and life choices. Runs a two-phase workflow: classify and clarify the question, then run parallel multi-source searches with adversarial verification and source-quality tags, and write a typed report. Use when the user wants researched recommendations backed by sources. NOT for pre-implementation planning — use a planning workflow for that."
allowed-tools: WebSearch WebFetch Read Write TaskCreate TaskUpdate
argument-hint: "[topic, question, or vague idea]"
effort: high
---

## Topic

$ARGUMENTS

If no topic is provided, ask the user what to research before proceeding.

## Output language

Default output language is **Chinese**. If the user writes in — or explicitly asks for — another language, produce all output (discovery, questions, plan, report) in that language instead. Every "write in Chinese" instruction below follows this setting.

## Two-Phase Workflow

At the start, create a task list with **TaskCreate** to track progress — do **not** paste a markdown checklist into your reply. Mark each task `in_progress` when you begin it and `completed` when done with **TaskUpdate**. Skip the Phase 0 tasks when context is sufficient and you go straight to Phase 1 (see Phase 0, Step 3). Write task titles in the output language (Chinese by default; see Output language above).

Create these tasks:

1. Phase 0 · classify research type + blind exploration (parallel searches)
2. Phase 0 · separate hard constraints vs. challenged assumptions
3. Phase 0 · ask (≤3 questions incl. Outcome Alignment), then confirm plan
4. Phase 1 · parallel dimension searches (official + community each)
5. Phase 1 · per-dimension recommendation + source-quality tags
6. Phase 1 · adversarial check on every recommendation
7. Phase 1 · cross-dimension synthesis + self-review checkpoint
8. Phase 1 · write the typed report

---

### Phase 0: Assess Context & Clarify

**Goal**: Classify the research type, surface and challenge wrong assumptions, then ask only about what is genuinely missing.

**Step 1 — Read existing context (before any search):**

If the user mentions an existing file, tool, implementation, or prior attempt, Read it first. This reveals constraints not mentioned in the request. Skip this step if nothing concrete is referenced.

**Step 2 — Classify research type:**

| Type        | User state                      | User need            | Signals                                                                |
| ----------- | ------------------------------- | -------------------- | ---------------------------------------------------------------------- |
| Exploratory | Has a topic, no prior knowledge | Understand the space | "what is", "overview", "help me understand", "tell me about"           |
| Directional | Has a goal, no path             | Find viable routes   | "how to", "how do I", "best way to", "I want to achieve X"             |
| Comparative | Has ≥2 options                 | Choose between them  | "A vs B", "should I use X or Y", "comparing"                           |
| Validatory  | Has a specific plan or approach | Confirm or refute it | "is this right", "will this work", "I'm planning to", "my approach is" |

For mixed signals, classify by the primary need.

**Step 3 — Context assessment:**

- **Skip Phase 0** if: type is Comparative AND ≥2 specific options named AND ≥1 hard constraint stated AND clear goal evident
- **Condensed Phase 0** if: type is Comparative or Validatory with partial context — run blind exploration, ask only about genuine gaps
- **Full Phase 0** if: type is Exploratory or Directional, OR insufficient context for other types

Never ask about information the user already provided.

**Step 4 — Blind exploration (if Phase 0 not skipped):**

Run 3–5 broad searches **in a single batch (all in one response)**:

1. Topic overview — key concepts, current state of the space
2. Community and practitioner experience — what real teams or people actually do
3. Known failure modes and pitfalls
4. **Assumption validation** — for each concrete approach or claim the user has stated, actively search for evidence that challenges it

Collect signals only. Treat each search result as an independent signal — do not interpret result B through the lens of result A. Cross-comparison only after all results are in.

For **Directional** research: include at least one search for routes the user has not mentioned.

**Step 5 — Separate constraints from assumptions:**

From user input and search results, distinguish:

- **Hard constraints**: cannot be changed (existing systems, team size, budget, regulatory requirements, time)
- **Soft assumptions**: user believes these to be true but research may contradict them (proposed solution, chosen tool, stated implementation path)

If any assumption is contradicted by search results, mark it as **challenged**. Surface it explicitly in the Phase 0 output — do not smooth it over.

**Step 6 — Identify dimensions:**

- **Comparative**: extract independent decision axes. Prune any axis where blind exploration already has a clear dominant answer — state it as a premise constraint instead.
- **Exploratory / Directional / Validatory**: identify what is genuinely unknown vs. what blind exploration already resolved.

Translate unknowns into need-based questions: ask about outcomes, constraints, and intent — never about technologies.

- Wrong: "Should we use Mermaid or HTML?"
- Right: "Do you want the output to open in a browser, or appear inline in the terminal?"

**Step 7 — Outcome Alignment Check (mandatory, cannot skip):**

Before asking the user, formulate and include this question in the Phase 0 output:

> "In [user's core scenario], what does the user see or experience when this is 'done'?"

This question must appear regardless of how clear the rest of the context seems. The user may answer "I don't care, follow mainstream" — that is a valid and complete answer. But the question must be asked.

User fast path: if the user explicitly says "skip phase 0", this step may be skipped.

**Step 8 — Ask, then confirm plan:**

Present Phase 0 output and wait for answers. Once the user responds, synthesize answers into a research plan, present it, and wait for confirmation before proceeding to Phase 1.

**Phase 0 output — write in Chinese:**

```
## Discovery — <Topic>

**Research type**: <Exploratory / Directional / Comparative / Validatory>
**Reason**: <one sentence explaining the classification>

<key signals from blind exploration: what the space looks like, main points of interest>

**Challenged assumptions** (if any):
- "<user's stated assumption or direction>" — research shows: <one-sentence contradicting evidence>

A few questions to narrow the research direction:
1. <Outcome Alignment question — what does "done" look like for the user?>
2. <need-based question — answerable without domain knowledge>
```

Omit the "Challenged assumptions" section if no contradictions were found.

**Research plan output (after user answers) — write in Chinese:**

```
## Research Plan — <Topic>

**Research type**: <type>

Premise constraints (confirmed in Phase 0, not treated as research dimensions):
- <dimension>: use <X> — <one-line reason>

Dimensions / questions requiring deep research:
- Dimension 1: ...
- Dimension 2: ...

Out of scope: <anything excluded based on user's constraints>

Confirm to start, or adjust scope.
```

Omit the premise constraints section if no dimensions were pruned.

---

### Phase 1: Targeted Deep Research

**Starts only after the user confirms the research plan (or skips Phase 0 when context is sufficient).**

Hard constraints are fixed. Assumptions challenged in Phase 0 must be researched further — do not revert to treating them as constraints.

**Step 1 — Read `references/source-catalog.md`** — select source types most relevant to this topic's domain.

**Step 2 — Search all dimensions in a single batch** — all searches fire in one response; never search one dimension, process, then the next. For each dimension: two searches simultaneously — one official/authoritative, one community/practitioner. Use source types from the catalog. Do not use context7 for general research topics (SEO, architecture, community experience) — context7 is for library/framework API documentation only.

**Step 3 — Per-dimension recommendation** — pick one default recommendation per axis, tied to user's constraints. List alternatives only for specific edge cases.

After each recommendation, append a source quality tag based on the evidence gathered:

- `[High]` = ≥2 independent authoritative sources agree (official docs, peer-reviewed papers)
- `[Medium]` = 1 authoritative source OR community consensus across ≥2 platforms
- `[Low]` = practitioner experience only, or adversarial check raised concerns

Taste, aesthetic, or preference-type recommendations (e.g. visual style) cap at `[Medium]` — no authoritative source can settle a matter of taste, so they are never `[High]`. If nearly every dimension comes out `[High]`, re-check the tags: the tag is calibration, not decoration.

In the Chinese report, label this "Source Quality" — not "Confidence Level" — to avoid false precision.

**Step 4 — Adversarial check** — for each recommended option from Step 3, run one targeted search per recommendation in a single batch using negation-intent queries: `"<X> problems"`, `"why not use <X>"`, `"<X> pitfalls"`. If results surface a failure mode that undermines the recommendation, revise it before proceeding to Step 5.

**Step 5 — Cross-dimension synthesis** — verify the recommended combination works across all axes. Check for conflicts.

If synthesis reveals a Phase 0 hard constraint is actually inapplicable, or a critical new dimension emerges that would change the entire research direction: stop, surface the finding to the user, and wait for direction before writing the report.

**Step 6 — Resolve remaining gaps** — if synthesis revealed knowledge gaps, run targeted searches now to fill them before writing the report. Only escalate to Open Questions if a gap genuinely cannot be resolved by any search (i.e., the answer depends on the user's specific preferences or constraints).

**Step 7 — Self-review checkpoint** — before loading the template, answer the following six questions. All six must pass; if any fails, return to Step 3 or Step 6 and revise before proceeding.

1. **Answer alignment**: Does the report directly answer the core question confirmed in Phase 0?
2. **Constraint compliance**: Does every recommendation comply with all hard constraints from Phase 0?
3. **Cross-dimension conflict**: Do any recommendations across dimensions contradict each other (e.g., one recommends lightweight while another pulls in a large dependency)?
4. **Challenged assumptions handled**: Were all assumptions flagged as "challenged" in Phase 0 explicitly addressed in the recommendations?
5. **Source honesty**: Are all "mainstream" or "industry standard" claims backed by a named source?
6. **Source quality tagged**: Does each per-dimension recommendation carry a `[High / Medium / Low]` source quality tag?

**Step 8 — Write report** — Read the template matching the confirmed research type, then generate the full report in Chinese following its structure:

- Exploratory → `references/template-exploratory.md`
- Directional → `references/template-directional.md`
- Comparative → `references/template-comparative.md`
- Validatory → `references/template-validatory.md`

Add a **Research Sources** line in the report header listing the source types actually consulted (e.g., `Google Search Central official docs, Stack Overflow, GitHub Issues, Hacker News`). Output in conversation AND write to `/tmp/research-<date>-<slug>.md` using the Write tool (`<date>` = today's date in YYYY-MM-DD format; `<slug>` = 3–5 word English description of the topic in lowercase with hyphens — translate from the original language if needed, e.g. a Chinese topic becomes its English equivalent).

## Rules

### Both Phases

- **All searches in parallel** — Phase 0 blind exploration and Phase 1 dimension searches must be batched in a single response; never run one search, wait, then the next
- **All searches in English** — all WebSearch queries must be in English regardless of the topic's language; search quality degrades significantly otherwise
- **Output language** — default Chinese; if the user uses or requests another language, switch all output (discovery, questions, report, commentary) to it. This governs every "write in Chinese" instruction above
- **Research type first** — classify type before deciding output structure; never default to Comparative
- **Assumptions vs. constraints** — user-stated approaches, tools, and paths are assumptions, not hard constraints; actively challenge them when research contradicts them
- **Official sources first** — do not recommend what you have not verified at the source
- **Default + escape hatch, not menus** — one recommendation per dimension; alternatives only for specific conditions the user might hit
- **Give a concrete recommendation** — "it depends" with no follow-up is not acceptable
- **Research only** — surface the report; do not implement findings

### Phase 0

- **Need-based questions only** — questions must be answerable without domain knowledge; reframe if not
- **Prune obvious dimensions** — if blind exploration yields a clear winner, state it as premise constraint, not a research dimension
- **Phase 0 question cap** — maximum 3 questions total in Phase 0 output (including Outcome Alignment); additional unknowns must be resolved through blind exploration, not from the user

### Phase 1

- **Adversarial check is not optional** — Step 4 fires for every recommendation; a recommendation that has not been challenged is incomplete
- **Tag calibration** — source-quality tags must discriminate; taste/preference items cap at `[Medium]`, and an all-`[High]` table is a signal to re-examine the evidence, not a sign of success
- **Dynamic source selection** — read source-catalog, select by domain; never default to "docs + community" for everything
- **context7 scope** — context7 is for library/framework API documentation only; use WebSearch for SEO, architecture decisions, and community experience
- **Open Questions: research gate** — before surfacing any candidate question, ask: "could research resolve this?" If yes, research it now. Only ask the user for decisions that depend on their preferences, constraints, or situation that no search can resolve. Max 2. Omit section entirely if nothing genuinely requires user input.
- **Open Questions handoff** — if user confirms to proceed without answering Open Questions, surface them again before implementing; do not silently pick defaults
