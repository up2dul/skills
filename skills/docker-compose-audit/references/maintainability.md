# Maintainability Audit Rules

Use this reference for duplication, configuration boundaries, network clarity, unnecessary host mappings, and Compose readability.

## Inspect

- duplicated configuration that can safely use YAML anchors/extensions;
- overly broad environment injection;
- unclear separation between secrets, ordinary environment values, and file-based config;
- unnecessary host port mappings used only for container-to-container communication;
- network definitions and trust boundaries;
- confusing or redundant configuration;
- environment-specific settings mixed together;
- build-time values repeated as runtime values when runtime consumption is impossible;
- abstractions that reduce rather than improve readability.

## Duplication

Use YAML anchors/extensions when they remove meaningful repetition without obscuring service behavior.

Good candidates can include shared logging, labels, environment fragments, or repeated image/runtime settings.

Do not refactor trivial duplication merely to make YAML clever.

## Environment Boundaries

A service should generally receive only the configuration it needs.

If a broad `env_file` is used across unrelated services:

- identify the breadth;
- do not invent its contents;
- validate whether narrower injection would improve clarity or least privilege.

Keep security-sensitive conclusions in the security dimension, but note maintainability costs when configuration ownership becomes unclear.

## Host Port Mappings

If a port exists only so another Compose service can reach it, the mapping is usually unnecessary because shared networks provide direct service-to-service connectivity.

Validate host-level tooling needs before recommending removal.

## Networks

The implicit default network is valid and often sufficient.

Custom frontend/backend networks may improve trust-boundary clarity or isolation, but classify this as hardening/maintainability unless a concrete isolation requirement exists.

Do not create network complexity with no demonstrated benefit.

## Environment-Specific Files

If development and production concerns are mixed, consider whether Compose overrides or separate production configuration would make intent clearer.

Do not insist on multiple files when profiles, overrides, or a single explicit file already provide clear behavior.

## Redundant Configuration

Flag values that are clearly ineffective or duplicated semantically, but validate application/runtime behavior before removal when Compose alone cannot prove redundancy.