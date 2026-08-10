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

## Installation

List the available skills:

```bash
npx skills add up2dul/skills --list
```

Install the Ideation Workflow skill:

```bash
npx skills add up2dul/skills --skill ideation-workflow
```

Install it globally for Codex:

```bash
npx skills add up2dul/skills --skill ideation-workflow --agent codex --global
```

## Repository structure

```text
skills/
└── ideation-workflow/
    └── SKILL.md
```
