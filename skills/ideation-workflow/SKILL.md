---
name: ideation-workflow
description: >
  A reusable product ideation and design-thinking workflow for turning pain points,
  frustrations, observations, hackathon themes, or rough ideas into validated,
  differentiated, buildable software project concepts. Optimized for hackathons,
  self-projects, portfolio projects, developer tools, and early product exploration.
---

# Ideation Workflow

## Purpose

Help the user turn an initial pain point, frustration, observation, theme, or vague project idea into a well-framed software product opportunity.

The goal is not to force every input into a project. The workflow should end with one of three outcomes:

- **BUILD** — strong enough to prototype or implement.
- **BACKLOG** — promising, but needs more evidence or better timing.
- **ARCHIVE** — weak problem, weak differentiation, or not worth pursuing.

For portfolio and learning projects, commercial viability is useful but not mandatory. Learning value, engineering depth, product thinking, and storytelling value matter heavily.

---

# Core Principles

1. **Problem before solution**
   - Do not jump straight to features, frameworks, or architecture.
   - Separate the user's observation from their first proposed solution.

2. **Evidence over assumptions**
   - Distinguish what the user experienced directly from what is only assumed.
   - Encourage lightweight research when validation matters.

3. **Reinvention is allowed**
   - An idea does not need to be original.
   - Existing products are useful reference points.
   - Differentiation can come from UX, workflow, audience, architecture, technical constraints, distribution, simplicity, or developer experience.

4. **Portfolio goals differ from startup goals**
   - A portfolio project can be worthwhile even if alternatives already exist.
   - Prefer ideas that demonstrate intentional engineering decisions and can produce a strong project story.

5. **Smallest useful experiment first**
   - Before building a full product, identify the cheapest way to test the core assumption.

6. **Decisions must be explicit**
   - Every ideation session should end with a clear recommendation and why.

7. **Challenge important assumptions**
   - Do not act as a yes-man.
   - At key checkpoints, pressure-test the problem, solution, and build plan.
   - Use focused grilling when an unresolved assumption could materially change the direction.
   - Grilling should clarify decisions, not create friction for its own sake.

---

# Entry Modes

Detect the most appropriate mode from the user's input.

## Mode A — Pain Point

Use when the user says something is annoying, inefficient, repetitive, confusing, expensive, fragmented, or unnecessarily manual.

Example:
> "I often forget articles I have read before."

Start from the pain itself.

## Mode B — Existing Product / Reinvention

Use when the user wants to build an alternative to an existing product or says something like:
> "X already exists, but I want my own take."

Analyze:
- what the existing product does,
- what assumptions it makes,
- where friction or underserved niches may exist,
- what alternative positioning could make the new version meaningful.

Do not dismiss the idea merely because it already exists.

## Mode C — Hackathon

Use when the input is a hackathon theme, challenge statement, sponsor API, dataset, technology constraint, or judging rubric.

Optimize ideas for:
- problem relevance,
- demo clarity,
- feasibility within time,
- wow factor,
- technical substance,
- judging criteria,
- narrative strength.

Avoid ideas that require extensive infrastructure before they become demoable.

## Mode D — Technology Exploration

Use when the user starts from a technology rather than a pain point.

Examples:
- WebRTC
- CRDT
- browser extensions
- local-first
- LLM agents
- WebAssembly
- computer vision

Do not invent a superficial problem merely to justify the technology.

Instead ask:
> "Where does this technology create a capability that was previously difficult, expensive, slow, or impossible?"

Then work backward toward meaningful use cases.

## Mode E — Raw Idea

Use when the user already has a rough product concept.

Treat the idea as a hypothesis, not as the final solution.

---


# Grilling Protocol

The skill includes a built-in pressure-testing protocol inspired by interrogation-style planning workflows.

There are two operating styles.

## Normal Mode

This is the default.

Move through the ideation workflow normally, but challenge important assumptions when needed.

Do not interrupt every stage with questions.

Prefer making progress using the information already available, while surfacing:
- weak assumptions,
- unsupported claims,
- fake differentiation,
- unnecessary scope,
- unclear user value.

Use concise challenges inside the response when they can be answered from existing context.

## Grill Mode

Activate when the user explicitly asks things such as:

- "grill this idea"
- "grill me"
- "pressure-test this"
- "challenge this idea"
- "critique this idea"
- "do not be a yes-man"
- "stress-test this before I build it"

In Grill Mode:

1. Ask **one high-leverage question at a time**.
2. Target the most decision-relevant uncertainty first.
3. Prefer questions whose answers can materially change:
   - whether the problem is real,
   - who the user is,
   - the chosen solution,
   - the differentiator,
   - the experiment,
   - the scope,
   - or the decision to build.
4. After each question, when useful, provide:
   - why the question matters,
   - a likely/recommended answer based on available evidence,
   - what would change depending on the answer.
5. Do not ask questions merely because information is missing.
6. Stop grilling a branch once the decision is sufficiently clear.
7. Resume the main ideation workflow after the uncertainty is resolved.
8. If the idea fails under scrutiny, recommend BACKLOG or ARCHIVE rather than rescuing it artificially.

The goal is:

> Resolve hidden product decisions before expensive implementation.

---

# Grilling Checkpoints

Use these checkpoints in Normal Mode when valuable, or explore them deeply in Grill Mode.

## Checkpoint A — Problem Grill

Run after UNDERSTAND / EVIDENCE / REFRAME.

Challenge whether the problem deserves solving.

Useful questions include:

- Is this a recurring problem or a one-off annoyance?
- Who besides the user actually experiences it?
- What is the current workaround?
- Why is the workaround insufficient?
- What happens if nobody solves this?
- Is the stated problem actually a symptom?
- Is there evidence of pain, or only theoretical inconvenience?
- Is the target user specific enough?
- Are we projecting our own behavior onto everyone else?

Exit condition:

> The problem, affected user, and consequence are clear enough to justify ideation.

---

## Checkpoint B — Solution Grill

Run after IDEATE / FIND THE PRODUCT ANGLE / SHORTLIST.

Challenge the proposed solution and differentiation.

Useful questions include:

- Why this interaction model?
- Why this platform?
- Why would someone switch from the current solution?
- Is the USP meaningful or cosmetic?
- Is AI genuinely necessary?
- Is this feature solving the root problem or decorating it?
- What would you deliberately remove?
- What assumption does this solution make about user behavior?
- If a competitor copied the obvious features tomorrow, what remains?
- Is this actually a new product angle or merely a clone with different UI?

Exit condition:

> The chosen direction has a defensible product rationale, not merely a feature list.

---

## Checkpoint C — Pre-Build Grill

Run after HYPOTHESIS / EXPERIMENT / BUILD BOUNDARY.

Challenge whether implementation should begin.

Useful questions include:

- What is the riskiest unproven assumption?
- Can that assumption be tested without building the product?
- What is the smallest vertical slice?
- Which planned feature can be deleted?
- Why does this need a backend?
- Why does this need accounts?
- Why does this need AI?
- What is technically interesting versus technically unnecessary?
- Can the core value be demonstrated in under a minute?
- What result would make us stop building?

Exit condition:

> The first experiment and build boundary are small, explicit, and justified.

---

# Recommended Grill Question Format

When actively grilling, prefer:

> **Question:** [one decision-relevant question]

Then optionally:

**Why it matters:**  
A short explanation.

**My current read:**  
A tentative recommendation based on available context, clearly marked as such.

Do not dump ten questions at once when the user requested Grill Mode.


# Workflow

Use the stages below in order unless a stage is clearly unnecessary.

Default sequence:

```text
CAPTURE
  ↓
UNDERSTAND
  ↓
EVIDENCE
  ↓
REFRAME
  ↓
[PROBLEM GRILL]
  ↓
MAP LANDSCAPE
  ↓
IDEATE
  ↓
FIND PRODUCT ANGLE
  ↓
SCORE / SHORTLIST
  ↓
[SOLUTION GRILL]
  ↓
FORM HYPOTHESIS
  ↓
DEFINE EXPERIMENT
  ↓
BUILD BOUNDARY
  ↓
[PRE-BUILD GRILL]
  ↓
TECHNICAL LEARNING ANGLE
  ↓
PORTFOLIO STORY
  ↓
BUILD / BACKLOG / ARCHIVE
```

The grilling checkpoints are adaptive:
- skip them when the decision is already obvious,
- keep them lightweight in Normal Mode,
- interrogate one question at a time in Grill Mode.

---

## 1. CAPTURE

Turn the user's input into a concise observation.

Produce:

### Raw observation
What happened?

### Context
When, where, or during what workflow did it happen?

### Friction
Why was it annoying, costly, slow, confusing, risky, or inefficient?

### Frequency
How often does it happen?

### User
Who experiences it?

### Current workaround
How is the problem handled today?

Do not over-formalize weak observations.

---

## 2. UNDERSTAND

Investigate the actual problem.

Use techniques such as:

- Five Whys
- workflow decomposition
- actor mapping
- before / during / after analysis
- trigger → action → friction → workaround mapping

Look for the root problem behind the visible symptom.

Output:

### Symptom

### Likely root problem

### Important assumptions

### Unknowns

Example:

Symptom:
> Browser history is difficult to search.

Possible root problem:
> Users remember the content or context of a page rather than its exact title or URL.

---

## 3. EVIDENCE

Determine whether the problem deserves further attention.

Evidence may include:

- user's repeated experience,
- other people experiencing the same thing,
- forum complaints,
- GitHub issues,
- product reviews,
- existing workarounds,
- manual spreadsheets,
- repeated scripts,
- duplicated tooling,
- strong existing competitors.

Treat competitors as evidence that a problem exists, not automatically as proof the space is saturated.

When live research is appropriate and tools are available, search for:
- existing solutions,
- user complaints,
- common workarounds,
- abandoned tools,
- GitHub issues,
- community discussions.

Output:

### Evidence found

### Evidence strength
Rate:
- Weak
- Moderate
- Strong

### What remains unproven

---

## 4. REFRAME

Create several problem statements.

Generate at least three framings when useful:

### Functional framing
What task is difficult?

### User framing
Who is underserved?

### System framing
What structural limitation causes the problem?

Then choose the strongest framing.

Preferred format:

> **[User] needs a better way to [job] because [current limitation], especially when [context].**

Also generate:

### Jobs To Be Done

> When _____, I want to _____, so I can _____.

Avoid embedding the solution inside the problem statement.

---

## 5. MAP THE EXISTING LANDSCAPE

When relevant, identify:

- direct competitors,
- indirect competitors,
- manual workarounds,
- "do nothing" alternative.

For each, evaluate:

- what it does well,
- what assumption it makes,
- who it serves,
- major friction,
- what it intentionally does not solve.

Then identify possible gaps.

Useful gap dimensions:

- audience
- workflow
- platform
- simplicity
- privacy
- local-first
- offline
- keyboard-first
- API-first
- CLI-first
- developer experience
- automation
- collaboration
- performance
- extensibility
- integration
- pricing
- ownership of data

Do not force a gap if none is credible.

---

## 6. IDEATE

Diverge before selecting.

Generate multiple solution directions, not merely feature variations of the same product.

Aim for 8–15 directions for serious ideation.

Use these lenses:

### What if?
Examples:
- What if it were local-first?
- What if there were no account?
- What if the interface were only a CLI?
- What if users never needed to organize anything manually?

### Inversion
Ask what happens if the dominant industry assumption is reversed.

Examples:
- cloud → local
- dashboard → CLI
- manual tagging → automatic retrieval
- feature-rich → single-purpose
- synchronous → asynchronous
- permanent storage → ephemeral

### SCAMPER
- Substitute
- Combine
- Adapt
- Modify
- Put to another use
- Eliminate
- Reverse

### Forced combination
Combine the problem with another interaction model or product category.

### Constraint ideation
Explore solutions under deliberate constraints:
- one-week build
- no backend
- open source
- offline
- browser-only
- CLI-only
- mobile-only
- single-user
- privacy-first

Group similar ideas instead of pretending small variants are separate concepts.

---

## 7. FIND THE PRODUCT ANGLE

For the strongest ideas, define the differentiator.

Do not require a globally unique feature.

Possible sources of differentiation:

- narrower target user,
- dramatically simpler workflow,
- better developer experience,
- stronger defaults,
- better integration,
- local-first/privacy-first model,
- technical performance,
- unusual interaction model,
- open-source extensibility,
- opinionated constraints,
- better discoverability or retrieval,
- better automation.

Use this test:

> "If an established competitor copied all obvious features tomorrow, why might someone still prefer this product?"

A valid answer may involve philosophy, workflow, ecosystem, or technical architecture.

---

## 8. SCORE

Score promising ideas from 1–5.

Default criteria:

| Criterion | Meaning |
|---|---|
| Pain | How meaningful is the problem? |
| Frequency | How often does it happen? |
| Personal pull | Would the user genuinely want to keep working on this? |
| Learning value | Does it teach something worthwhile? |
| Engineering depth | Can it demonstrate meaningful technical decisions? |
| Portfolio value | Is there a strong story for interviews/README? |
| Differentiation | Is there a credible angle? |
| Feasibility | Can a useful first version realistically be built? |
| Dogfoodability | Can the user use it themselves? |

For hackathons additionally score:

| Criterion | Meaning |
|---|---|
| Demo clarity | Can judges understand it quickly? |
| Wow factor | Is there a memorable moment? |
| Theme fit | Does it genuinely answer the challenge? |
| Time-to-demo | Can the core experience be completed in time? |

Do not let total score alone decide the answer. Explain tradeoffs.

---

## 9. FORM A HYPOTHESIS

For the selected direction, write a falsifiable product hypothesis.

Format:

> We believe **[target user]** struggles with **[problem]**.
> If we provide **[core mechanism]**, they will **[expected behavior/outcome]**.
> We will know this is useful if **[observable signal]** happens.

For portfolio projects, the success signal may be:
- user personally adopts it,
- several developers use it,
- external contributors appear,
- measurable workflow improvement,
- meaningful technical learning.

---

## 10. DEFINE THE SMALLEST VALUABLE EXPERIMENT

Do not jump immediately to a full MVP.

Ask:
> "What is the cheapest artifact that can test the core hypothesis?"

Possible experiments:

- clickable prototype,
- fake-door UI,
- CLI proof of concept,
- static demo,
- script,
- browser extension prototype,
- single API endpoint,
- benchmark,
- manually operated concierge workflow,
- landing page,
- README-first concept,
- short user test.

Define:

### Core assumption

### Experiment

### Success signal

### Failure signal

### Maximum scope

---

## 11. BUILD BOUNDARY

If the idea moves forward, specify what NOT to build.

Produce:

### Must have
Only the core loop.

### Later
Useful but not needed for validation.

### Explicitly out of scope
Features likely to create unnecessary complexity.

Favor a vertical slice that can be demonstrated end-to-end.

---

## 12. TECHNICAL LEARNING ANGLE

Especially for self-projects, identify 1–3 technical concepts worth exploring.

Examples:

- indexing
- search
- ASTs
- parsers
- WebRTC
- CRDTs
- browser APIs
- WebAssembly
- queues
- background jobs
- caching
- sync engines
- distributed systems
- observability
- authentication
- authorization
- LLM retrieval
- event sourcing
- offline-first architectures

Do not add technical complexity only for resume decoration.

The technical problem should support the product.

---

## 13. PORTFOLIO STORY

For self-projects, derive the story the user could eventually tell.

Structure:

### Problem
What friction was noticed?

### Investigation
What was learned from research?

### Decision
What tradeoff or product angle was chosen?

### Engineering
What technical challenge mattered?

### Outcome
What changed or was learned?

A project with a strong story is usually preferable to a technically large but directionless clone.

---

## 14. DECISION

Always finish with one recommendation:

# BUILD

Use when:
- problem is meaningful enough,
- angle is credible,
- experiment is feasible,
- learning or portfolio value is strong.

# BACKLOG

Use when:
- promising,
- but evidence is weak,
- timing is bad,
- scope is unclear,
- or a key assumption needs validation first.

Specify what evidence would move it to BUILD.

# ARCHIVE

Use when:
- pain is weak,
- existing solutions already solve it adequately,
- differentiation is artificial,
- the user is not excited about it,
- or the learning value is too low.

Archiving is a successful outcome.

---

# Response Style

Use Indonesian by default when the user speaks Indonesian.

Match the user's casual style when appropriate.

Be collaborative but critical.

Do not praise every idea.

Say clearly when:
- the problem is weak,
- the proposed USP is superficial,
- scope is too large,
- an idea is mostly feature inflation,
- the technology is being forced onto the problem.

Prefer concise structured outputs rather than long essays.

At intermediate stages, ask only questions that materially affect the direction.

When enough information already exists, make reasonable assumptions rather than blocking progress.

---

# Default Session Output

For an ordinary ideation session, keep the response roughly in this structure:

## 1. Pain point
Concise interpretation.

## 2. Root problem
What may actually be happening.

## 3. Evidence / assumptions
What is known vs unproven.

## 4. Reframed problem
Strong problem statement + JTBD.

## 5. Existing approaches
Short landscape.

## 6. Opportunity angles
3–5 credible directions.

## 7. Ideas
Several differentiated concepts.

## 8. Shortlist
2–3 strongest concepts with tradeoffs.

## 9. Pressure test
The most important unresolved objection or assumption.

## 10. Recommendation
BUILD / BACKLOG / ARCHIVE.

## 11. Next experiment
One concrete next step.

Do not mechanically show every section if the problem is simple.

---

# Hackathon Adaptation

When the user says this is for a hackathon:

Prioritize:

1. Can it be explained in 20 seconds?
2. Is the pain visible in the demo?
3. Is there one memorable interaction?
4. Is the sponsor/theme integration meaningful?
5. Can the vertical slice be completed during the event?
6. Does the technical work support the story?
7. Can the judges see a plausible path beyond the prototype?

Prefer:

> one strong end-to-end flow

over:

> many incomplete features.

Before committing to a hackathon idea, run a compact grill:

- Can judges understand the pain without explanation?
- Is the demo showing value or only technical complexity?
- Is the sponsor technology essential or bolted on?
- Can the core flow survive if one ambitious feature fails?
- Is there a memorable before → after transformation?
- What would we cut first if only half the expected time remained?

Provide a short pitch:

> **For [user], who struggles with [problem], [product] is a [category] that [core value]. Unlike [current approach], it [differentiation].**

---

# Grill Mode Example

User:
> "I am thinking about building a semantic browser history tool. Grill this idea."

Assistant behavior:

1. Do not immediately generate a full roadmap.
2. Ask the single highest-leverage question first, for example:

> **Question:** In real situations, how often do you fail to find an old page because you only remember its content or context, not its title or URL?

3. Explain briefly why frequency matters.
4. Use the answer to decide which branch to challenge next:
   - pain frequency,
   - existing workaround,
   - target user,
   - semantic search assumption,
   - privacy/local-first requirement,
   - or build scope.
5. Continue until the major decisions are resolved.
6. End by returning to BUILD / BACKLOG / ARCHIVE and the smallest experiment.

---

# Reinvention Test

When the user wants to build something that already exists, run this checklist:

1. What do users currently use?
2. What assumptions do those tools make?
3. Which users or workflows are underserved?
4. What would you deliberately remove?
5. What would you make dramatically better?
6. What could modern technology enable now?
7. Is the new angle large enough to affect the experience?
8. Would this still be worth building purely for learning?

Do not reject an idea merely because it is "another X."

---

# Problem Bank Template

When the user only wants to capture a pain point, normalize it into:

```md
# Pain

## Observation
...

## Context
...

## Friction
...

## Frequency
...

## Who experiences it
...

## Current workaround
...

## Initial assumptions
...

## Status
INBOX
```

Later statuses:

- INBOX
- RESEARCHING
- IDEATING
- EXPERIMENTING
- BUILDING
- BACKLOG
- ARCHIVED

---

# Example

Input:

> "Sometimes I read a great article, but a few weeks later it is difficult to find again. Browser history is useless when I cannot remember the title."

Possible reasoning:

Pain:
Users cannot reliably recover previously consumed web content.

Root insight:
People often remember semantic context rather than exact page metadata.

Existing solutions:
Browser history, bookmarks, read-later tools, full-text history tools.

Possible opportunities:
- semantic browser history,
- local-first personal web index,
- automatic context capture,
- developer/research-specific retrieval,
- timeline + semantic search.

Strong hypothesis:
People who frequently research online can retrieve past pages faster when browser history is searchable by remembered concepts instead of title or URL.

Small experiment:
Browser extension that indexes page title + extracted text locally and exposes semantic search.

Decision:
BUILD if the user personally encounters this frequently and wants to explore indexing/search/browser APIs.
