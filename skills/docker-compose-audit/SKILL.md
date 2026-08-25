---
name: docker-compose-audit
description: >
  Audit Docker Compose configurations for production correctness and quality across
  security, reliability, resources and performance, operations, and maintainability.
  Prioritizes evidence over assumptions, identifies missing context explicitly, and
  separates confirmed issues from validation needs and optional hardening.
---

# Docker Compose Audit

## Purpose

Audit one or more Docker Compose files and produce a prioritized, context-aware review.

The goal is not to maximize the number of best-practice findings. Identify configuration that is actually risky, fragile, misleading, unnecessarily privileged, difficult to operate, or unnecessarily hard to maintain, while clearly separating those findings from optional hardening.

This skill is Compose-focused. Do not expand into a general Dockerfile, CI/CD, registry, Kubernetes, cloud infrastructure, or application architecture review unless external context is required to validate a Compose finding.

## Core Principles

1. **Evidence over assumptions**
   - Report what the supplied Compose configuration proves.
   - Never invent reverse proxies, firewalls, load balancers, tunnels, secret managers, backups, log shipping, host resources, or workload behavior.
   - If external context can change the verdict, use `NEEDS VALIDATION`.

2. **Production correctness over checklist compliance**
   - Missing `healthcheck`, custom networks, resource limits, `init`, `read_only`, secrets, or capability drops are not automatically defects.
   - Explain a concrete failure mode or benefit before recommending a change.

3. **Severity and certainty are separate**
   - A potentially severe issue can still require validation.
   - Do not present uncertain findings as facts.

4. **Context changes recommendations**
   - Development, CI, staging, and production Compose files have different requirements.
   - Cache-only state has different durability needs from queues or durable state.
   - A host-bound port may be intentional for local administration.

5. **Least privilege without cargo culting**
   - Prefer the minimum ports, mounts, capabilities, secrets, networks, and writable paths needed.
   - Do not recommend hardening that may break legitimate runtime behavior without validation.

6. **Audit first, ask second**
   - Produce all findings supported by current evidence.
   - Ask only the highest-value validation questions afterward.
   - Do not block the audit because some context is missing.

## Audit Dimensions

Treat these as peer dimensions. Security is important but is not the identity of the audit.

- Security
- Reliability
- Resources and performance
- Operations
- Maintainability

Load the corresponding reference for each full audit:

- `references/security.md`
- `references/reliability.md`
- `references/resources.md`
- `references/operations.md`
- `references/maintainability.md`

Always consult `references/compose-semantics.md` for Compose behavior and distinctions that findings depend on.

For a narrowly scoped audit, load only the relevant dimension references plus `compose-semantics.md`.

## Finding Verdicts

Use one of these verdicts for every meaningful finding:

### CONFIRMED ISSUE

Available evidence is sufficient to establish a real problem or dangerous behavior.

### NEEDS VALIDATION

The Compose file establishes a potentially important condition, but external context determines whether it is actually problematic.

State exactly what fact changes the verdict.

### HARDENING OPPORTUNITY

The current configuration is not wrong, but an optional change can reduce attack surface, blast radius, or operational risk.

### INFORMATIONAL

Behavior is worth making explicit, but no change is necessarily required.

After validation, reclassify a `NEEDS VALIDATION` finding instead of leaving it unresolved.

## Severity

Assign severity independently from verdict:

- **CRITICAL** — plausible immediate host compromise, catastrophic data exposure/loss, or equivalent blast radius.
- **HIGH** — significant security, data durability, or availability risk requiring prompt attention.
- **MEDIUM** — meaningful reliability, operational, resource, or maintainability risk.
- **LOW** — limited-impact issue or improvement.

Do not inflate severity to make an audit look useful. A finding may be `HIGH + NEEDS VALIDATION` when potential impact is high but context is incomplete.

## Validation Rules

When context is missing:

1. State the observable Compose fact.
2. Explain the potential consequence conditionally.
3. State what external fact changes the verdict.
4. Ask a focused validation question only if the answer materially changes prioritization or recommendation.

Never invent application behavior, infrastructure, durability requirements, host capacity, backup systems, or security controls.

### Validation Question Budget

- Ask at most **5 validation questions** in the initial audit.
- Prioritize questions that can change CRITICAL/HIGH verdicts, then MEDIUM findings.
- Combine related questions when one answer resolves multiple findings.
- Report lower-priority unknowns without interrogating the user immediately.

## Audit Procedure

### Step 1 — Establish scope

Determine which Compose file(s) are being audited, whether the target environment is known, and whether overrides/profiles are part of the supplied configuration.

If environment is unknown, continue and mark environment-dependent conclusions accordingly.

### Step 2 — Build a service map

For every service, identify visible:

- image/build source;
- published ports;
- networks;
- dependencies;
- volumes and bind mounts;
- environment, env files, secrets, and configs;
- healthchecks;
- restart policy;
- command/entrypoint;
- privileges, capabilities, devices, and namespace sharing;
- resource settings;
- logging;
- shutdown/lifecycle settings.

Also identify named volumes, networks, configs, and secrets at project level.

### Step 3 — Read relevant references

For a full audit, read all five dimension references and `compose-semantics.md`. For a scoped audit, read only the requested dimensions plus semantics.

### Step 4 — Check high-blast-radius configuration first

Prioritize host access, privilege escalation, sensitive mounts, public infrastructure ports, embedded secrets, and destructive persistence behavior before lower-priority hardening.

### Step 5 — Audit all applicable dimensions

Do not force findings into every category.

### Step 6 — Separate evidence from context

For every candidate finding:

```text
Can supplied Compose evidence prove the verdict?
  yes -> classify it
  no  -> could external context change the verdict?
           yes -> NEEDS VALIDATION
           no  -> classify from available evidence
```

### Step 7 — Prioritize and ask targeted questions

Sort primarily by severity, then by confidence and actionability. Ask no more than five validation questions.

### Step 8 — Re-evaluate after answers

When the user provides context, update affected findings only. State which were confirmed, dismissed, or reclassified.

## Output Contract

Start with:

```text
Docker Compose Audit
Overall risk: Low | Medium | High | Critical
Confirmed issues: N
Needs validation: N
Hardening opportunities: N
```

Group findings by severity. Each finding should include, when applicable:

```text
[HIGH] Finding title
Verdict: NEEDS VALIDATION
Dimension: Security / Operations
Evidence: What the Compose file proves.
Why it matters: Conditional or confirmed consequence.
Validate: The fact that changes the verdict.
Recommendation: Concrete action, conditional when needed.
```

Then include:

### Good Practices Already Present

Mention only meaningful good decisions visible in the configuration.

### Validation Questions

Ask up to five highest-value unresolved questions.

### Priority Actions

Give a short ordered set based on confirmed findings plus clearly conditional recommendations.

Do not rewrite the Compose file unless explicitly asked.

## Behavior to Avoid

Never:

- invent surrounding infrastructure or application behavior;
- call every missing hardening feature an issue;
- equate startup with readiness;
- equate persistence with backup;
- assume all Redis/cache/queue data must persist;
- assume every published port is internet-reachable;
- assume an image runs as root solely because Compose lacks `user`;
- recommend arbitrary CPU/memory/PID values without workload context;
- recommend secrets for ordinary non-sensitive configuration solely for checklist compliance;
- recommend Kubernetes or another orchestrator as part of this audit;
- expand into CI/CD or Dockerfile refactoring unless explicitly requested;
- ask the user to resolve every uncertainty before providing findings;
- rewrite configuration when only an audit was requested.

## Documentation Policy

When fresh documentation access is available and a finding depends on version-sensitive Compose behavior, prefer current official Docker documentation and the Compose specification over blog posts or remembered syntax.

Treat documentation as evidence for behavior, not as a checklist every project must implement.