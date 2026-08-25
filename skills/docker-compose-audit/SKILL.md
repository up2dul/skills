---
name: docker-compose-audit
description: >
  Audit Docker Compose configurations for production correctness and quality across
  security, reliability, resources and performance, operations, and maintainability.
  Prioritizes evidence over assumptions, explicitly identifies missing context, and
  separates confirmed issues from validation needs and optional hardening.
---

# Docker Compose Audit

## Purpose

Audit one or more Docker Compose files and produce a prioritized, context-aware review of the configuration.

The goal is not to maximize the number of "best practice" findings. The goal is to identify configuration that is actually risky, fragile, misleading, unnecessarily privileged, or difficult to operate, while clearly separating those findings from optional hardening.

This skill audits Docker Compose configuration. Do not expand into a general Dockerfile, CI/CD, registry, Kubernetes, cloud infrastructure, or application architecture review unless that context is necessary to explain a Compose finding. If another file is required to validate a finding, request or inspect it only for that validation and keep the resulting finding scoped to Compose.

---

# Core Principles

1. **Evidence over assumptions**
   - Report what the Compose configuration proves.
   - Never invent surrounding infrastructure such as reverse proxies, firewalls, load balancers, tunnels, orchestrators, or host-level services.
   - If external context can change the verdict, mark the finding as needing validation.

2. **Production correctness over checklist compliance**
   - Do not recommend a Compose feature merely because it exists.
   - Missing `healthcheck`, custom networks, resource limits, `init`, `read_only`, secrets, or capability drops are not automatically defects.
   - Explain the concrete failure mode or benefit before recommending a change.

3. **Severity and certainty are separate**
   - A potentially severe issue can still require validation.
   - Do not lower potential impact simply because context is missing.
   - Do not present uncertain findings as confirmed facts.

4. **Context changes recommendations**
   - Development, CI, staging, and production Compose files have different requirements.
   - Cache-only Redis has different durability requirements from Redis used as a durable queue.
   - A host-bound database port may be intentional for host-local administration.

5. **Least privilege without cargo culting**
   - Prefer the minimum ports, mounts, capabilities, secrets, networks, and writable paths needed by a service.
   - Do not recommend hardening that would break legitimate runtime behavior without first validating that behavior.

6. **Prioritize actionable findings**
   - Surface high-impact issues before cosmetic or optional improvements.
   - Avoid overwhelming the user with low-value recommendations.

7. **Audit first, ask second**
   - Produce all findings that can be established from available evidence.
   - Do not block the audit because some context is missing.
   - Ask only the highest-value validation questions after presenting the initial audit.

---

# Audit Dimensions

Treat these as peer dimensions. Security is important but is not the identity of the audit.

## Security

Inspect for:
- public or unnecessarily broad host port bindings;
- `privileged: true`;
- dangerous or unexplained `cap_add` entries, especially powerful capabilities such as `SYS_ADMIN`;
- host namespace sharing such as `network_mode: host`, `pid: host`, or similar settings;
- host device access;
- sensitive bind mounts, including `/`, Docker/Podman sockets, host credentials, SSH material, and system directories;
- writable bind mounts that could be read-only;
- unnecessary secret/config exposure between services;
- sensitive values passed as ordinary environment variables when a narrower secret mechanism is appropriate;
- services receiving a broad `env_file` despite needing only a small subset of values;
- unnecessary service-to-service reachability;
- security options and capabilities when the Compose file explicitly weakens defaults.

Do not assume a container runs as root solely because Compose does not specify `user`. Image-level `USER` is defined in the image/Dockerfile. Mark this as needing validation if relevant.

## Reliability

Inspect for:
- stateful services without appropriate persistent storage;
- persistence that does not match the service's actual durability requirements;
- health/readiness assumptions;
- misuse or misunderstanding of `depends_on`;
- startup ordering where dependency health is actually required;
- restart policies that conflict with the service's intended lifecycle;
- graceful shutdown needs, including `stop_grace_period` and `stop_signal` for workers or long-running jobs;
- PID 1 / child-process concerns where `init: true` may materially help;
- single points of failure that are specifically created or exposed by Compose configuration;
- unsafe coupling of one-off lifecycle operations to long-running service startup when visible in `command` or `entrypoint`;
- missing or incorrect volume declarations that could cause data loss during recreation.

Do not claim that `depends_on` short syntax waits for dependency health. Distinguish service start ordering from health-based readiness.

## Resources and Performance

Inspect for:
- CPU, memory, PID, or process concurrency settings that can create host exhaustion risk;
- resource constraints when host/workload context makes them meaningful;
- excessive worker/process counts visible in commands;
- services with potentially unbounded process creation where `pids_limit` may be useful;
- duplicated heavyweight services or configurations that have an obvious resource implication.

Absence of resource limits is not automatically an issue. If host capacity and workload are unknown, mark the concern as needing validation.

## Operations

Inspect for:
- logging configuration and possible unbounded local log growth;
- whether log rotation advice is relevant when logs may already be shipped externally;
- restart/recreate behavior implied by the configuration;
- image tag mutability and reproducibility requirements;
- production-specific versus development-specific mounts and settings;
- backup implications for named volumes and stateful services;
- configuration that makes routine maintenance, recreation, or recovery unexpectedly destructive;
- health information that can support deployment verification;
- Compose configuration that obscures which settings are runtime versus build-time.

Do not claim that a named volume is a backup. Persistence and backup are separate concerns.

## Maintainability

Inspect for:
- duplicated configuration that can safely use YAML anchors/extensions;
- overly broad environment injection;
- unclear separation between sensitive secrets, ordinary environment configuration, and file-based configs;
- unnecessary host port mappings used for container-to-container communication;
- implicit default-network behavior when isolation would materially improve the design;
- confusing or redundant configuration;
- environment-specific settings mixed together in ways that make production behavior unclear;
- build-time values repeated as runtime environment values when the application cannot consume them at runtime.

Do not optimize for clever YAML. Prefer explicit configuration when abstraction would reduce readability.

---

# High-Sensitivity Configuration

Always surface the following when present, because their blast radius can be large even when legitimate:

- `privileged: true`;
- Docker/Podman socket mounts;
- host root or sensitive system-directory mounts;
- `network_mode: host`;
- `pid: host` or other host namespace sharing;
- powerful added capabilities;
- host device passthrough;
- broad writable host bind mounts;
- secrets embedded directly in the Compose file;
- externally reachable database, cache, queue, or administrative ports.

Do not automatically label every occurrence a vulnerability. Explain what capability it grants and validate why it is needed.

---

# Finding Classification

Every meaningful finding should use one of these verdicts.

## CONFIRMED ISSUE

Use when the available configuration is sufficient to establish a real problem or dangerous behavior without relying on missing external context.

## NEEDS VALIDATION

Use when the Compose file establishes a potentially important condition but external context determines whether it is actually problematic.

State exactly what fact changes the verdict.

Example:

```text
Redis has no persistent volume.

Verdict: NEEDS VALIDATION
Reason: This is a durability issue if Redis stores durable queue/state. If Redis is intentionally cache-only and data loss is acceptable, persistence may be unnecessary.
Validate: What role does Redis serve, and must its data survive container recreation?
```

## HARDENING OPPORTUNITY

Use when the current configuration is not wrong, but an optional change can reduce attack surface, blast radius, or operational risk.

## INFORMATIONAL

Use when behavior is worth making explicit but no change is necessarily warranted.

After validation, a `NEEDS VALIDATION` finding should become a confirmed issue, hardening opportunity, informational note, or be removed as not an issue.

---

# Severity

Assign severity independently from the verdict:

- **CRITICAL** — plausible immediate host compromise, catastrophic data exposure/loss, or equivalent blast radius.
- **HIGH** — significant security, data durability, or availability risk that deserves prompt attention.
- **MEDIUM** — meaningful reliability, operational, resource, or maintainability risk that should be addressed deliberately.
- **LOW** — limited-impact issue or improvement.

Do not inflate severity to make an audit look useful. Optional hardening is usually LOW or MEDIUM unless the surrounding threat model clearly justifies more.

A finding may be `HIGH + NEEDS VALIDATION` when the potential impact is high but the auditor lacks the context needed to determine whether the configuration is intentional and protected elsewhere.

---

# Validation Rules

When context is missing:

1. State the observable Compose fact.
2. Explain the potential consequence conditionally.
3. State what external fact changes the verdict.
4. Ask a focused validation question only if the answer would materially change prioritization or recommendation.

Bad:

```text
Port 3000 is insecure. Bind it to 127.0.0.1.
```

Better:

```text
Port 3000 is published without a host IP, so Docker normally binds it on host interfaces.

Verdict: NEEDS VALIDATION
Potential impact: The service may be reachable directly depending on host firewall/network policy.
Validate: Is this service intentionally public, or should it only be reachable through another host-level ingress/reverse proxy?
```

Never invent:
- reverse proxies;
- firewall rules;
- cloud security groups;
- external secret managers;
- backup systems;
- log shipping;
- deployment topology;
- workload characteristics;
- host CPU/RAM;
- application retry behavior;
- whether Redis/cache/queue data is disposable.

---

# Validation Question Budget

Do not turn an audit into an interview.

- Ask at most **5 validation questions** in the initial audit.
- Prioritize questions that can change a HIGH/CRITICAL verdict first, then MEDIUM findings.
- Combine closely related questions when one answer resolves several findings.
- Do not ask about low-value hardening when higher-impact unknowns exist.
- If more than five unknowns remain, report the lower-priority findings as `NEEDS VALIDATION` without asking immediately.

---

# Audit Procedure

## Step 1 — Establish scope

Determine:
- which Compose file(s) are being audited;
- whether the intended environment is known;
- whether overrides/profiles are part of the supplied configuration.

If environment is unknown, continue the audit and mark environment-dependent conclusions accordingly.

## Step 2 — Build a service map

For each service, identify visible:
- image/build source;
- published ports;
- networks;
- dependencies;
- volumes/bind mounts;
- environment/env files/secrets/configs;
- healthchecks;
- restart policy;
- command/entrypoint;
- privileges/capabilities/devices/namespaces;
- resource settings;
- logging;
- lifecycle/shutdown settings.

Also identify named volumes, networks, configs, and secrets at the project level.

## Step 3 — Audit high-blast-radius configuration first

Check host access, privilege escalation, sensitive mounts, public infrastructure ports, embedded secrets, and destructive persistence behavior before lower-priority improvements.

## Step 4 — Audit all five dimensions

Review security, reliability, resources/performance, operations, and maintainability. Do not force findings into every dimension.

## Step 5 — Separate evidence from context

For every potential finding ask:

```text
Can the supplied Compose configuration prove the verdict?
  yes -> classify the finding
  no  -> could external context change the verdict?
           yes -> NEEDS VALIDATION
           no  -> classify from available evidence
```

## Step 6 — Prioritize

Sort primarily by severity, then by confidence/actionability. Do not bury confirmed high-impact issues below optional hardening.

## Step 7 — Ask targeted validation questions

Ask no more than five questions, following the validation question budget.

## Step 8 — Re-evaluate after answers

When the user provides context, update affected verdicts instead of repeating the entire audit. Explicitly say which findings were confirmed, dismissed, or reclassified.

---

# Important Compose Semantics

Keep these distinctions correct during audits:

- Services on the implicit default Compose network can normally reach each other by service name. Custom networks are optional isolation, not a universal requirement.
- Publishing a port is not required for container-to-container communication on a shared Compose network.
- A port mapping without an explicit host IP normally publishes beyond loopback; actual external reachability still depends on host/network controls.
- Short-form `depends_on` establishes dependency/start ordering but does not mean the dependency is healthy. Health-based dependency behavior requires appropriate healthcheck/condition configuration.
- `restart` policy does not make an unhealthy application healthy; it governs container restart behavior.
- Named volumes provide persistence across container recreation but are not backups.
- Bind mounts expose host filesystem paths and should be evaluated according to required access and write permissions.
- Compose `secrets` and `configs` can narrow file-based configuration access per service; environment variables remain appropriate for ordinary non-sensitive runtime configuration.
- Image tags are mutable. Digest pinning improves reproducibility but also requires an intentional update process for new image/security releases.
- `init: true`, `read_only`, `cap_drop`, `security_opt`, custom networks, resource limits, and PID limits are context-dependent controls, not mandatory checklist items.
- `stop_grace_period` matters when a process needs more time than the default shutdown window to finish or safely abandon work.

---

# Common Context-Dependent Findings

## Published database/cache ports

Observable fact: a stateful service publishes a host port.

Validate whether host-level tools or external clients intentionally need it. If only other Compose services consume it, recommend removing the host mapping. If host-local access is required, a loopback binding may be appropriate.

## Redis without persistence

Validate whether Redis is disposable cache, session/state storage, Celery/task broker, or another queue. Do not recommend persistence blindly.

## Missing healthcheck

Validate whether health information is useful for dependency readiness or deployment verification and whether the image/application exposes a meaningful probe. Do not add a superficial healthcheck that only proves a process exists.

## No resource limits

Validate host capacity, workload, worker counts, and contention risk. Do not invent arbitrary CPU/memory limits.

## Broad `env_file`

If file contents are unavailable, report that every listed variable may be injected into the service but do not claim specific secrets are exposed. Ask what classes of values the file contains when material.

## Default network only

Report reachability as informational or hardening unless there is a concrete isolation requirement. Do not call the default network inherently insecure.

## Mutable image tags

Treat this as a reproducibility/supply-chain tradeoff. Validate release/update policy before insisting on digest pinning.

## No `read_only` or capability drops

Treat as hardening unless there is evidence of unnecessary privileges. Validate required writable paths/capabilities before recommending restrictive settings.

---

# Output Format

Keep the report concise enough to act on.

Start with:

```text
Docker Compose Audit
Overall risk: Low | Medium | High | Critical
Confirmed issues: N
Needs validation: N
Hardening opportunities: N
```

Then group findings by severity. Each finding should contain, when applicable:

```text
[HIGH] Database port exposure
Verdict: NEEDS VALIDATION
Dimension: Security / Operations
Evidence: `db` publishes `5432:5432` without a host IP.
Why it matters: This can expose PostgreSQL beyond container-only networking depending on host/network controls.
Validate: Is direct host/external database access required, and what network controls exist?
Recommendation if not required: Remove the published port and use Compose networking internally.
```

After findings, include:

## Good Practices Already Present

Mention meaningful good decisions visible in the file. Do not add filler praise.

## Validation Questions

Ask up to five highest-value unresolved questions.

## Priority Actions

Give a short ordered set of actions based only on confirmed findings and clearly conditional recommendations.

Do not rewrite the Compose file unless the user explicitly asks for implementation or a corrected version.

---

# Audit Behavior to Avoid

Never:
- invent infrastructure or application behavior;
- call every missing hardening feature an issue;
- equate container startup with application readiness;
- equate persistence with backup;
- assume all Redis data must persist;
- assume every published port is externally reachable from the internet;
- assume an image runs as root solely from missing Compose `user`;
- recommend arbitrary CPU/memory/PID values without workload context;
- recommend secrets for ordinary non-sensitive configuration merely for checklist compliance;
- recommend Kubernetes or another orchestrator as part of a Compose audit;
- expand into CI/CD or Dockerfile refactoring unless explicitly requested;
- ask the user to answer every uncertainty before providing useful findings;
- rewrite configuration when the user requested only an audit.

---

# Reference Basis

When fresh documentation access is available and a finding depends on version-sensitive Compose behavior, prefer the current official Docker documentation and Compose specification over blog posts or remembered syntax.

Useful official topics include:
- Compose file reference;
- production use of Compose;
- services and `depends_on` semantics;
- networking;
- volumes and bind mounts;
- secrets and configs;
- resource constraints;
- Compose trust/security model.

Treat documentation as evidence for behavior, not as a checklist that every project must implement.