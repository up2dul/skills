# Agent Skills

A collection of reusable skills for AI coding agents.

## Available skills

### Ideation Workflow

A structured product ideation and design-thinking workflow that turns pain points, observations, hackathon themes, and rough ideas into validated, differentiated, and buildable software project concepts.

It supports:

- product and portfolio project ideation;
- hackathon idea development;
- existing-product reinvention;
- technology-first exploration;
- assumption pressure-testing through Grill Mode;
- explicit **BUILD**, **BACKLOG**, or **ARCHIVE** recommendations.

### Docker Compose Audit

A context-aware Docker Compose auditor focused on production correctness and quality across security, reliability, resources and performance, operations, and maintainability.

It supports:

- evidence-first Compose reviews without inventing surrounding infrastructure;
- prioritized findings with independent severity and confidence;
- explicit **CONFIRMED ISSUE**, **NEEDS VALIDATION**, **HARDENING OPPORTUNITY**, and **INFORMATIONAL** verdicts;
- targeted validation questions when project context can change a recommendation;
- auditing ports, persistence, healthchecks, dependencies, privileges, mounts, secrets, resources, logging, networks, and lifecycle behavior;
- recognition of good practices already present without turning optional hardening into mandatory checklist items.

## Installation

List the available skills:

```bash
npx skills add up2dul/skills --list
```

Install the Ideation Workflow skill:

```bash
npx skills add up2dul/skills --skill ideation-workflow
```

Install the Docker Compose Audit skill:

```bash
npx skills add up2dul/skills --skill docker-compose-audit
```

Install a skill globally for Codex:

```bash
npx skills add up2dul/skills --skill docker-compose-audit --agent codex --global
```

## Repository structure

```text
skills/
├── docker-compose-audit/
│   └── SKILL.md
└── ideation-workflow/
    └── SKILL.md
```
