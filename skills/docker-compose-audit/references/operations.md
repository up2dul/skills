# Operations Audit Rules

Use this reference for logging, reproducibility, backups, environment-specific behavior, deployment verification, and runtime/build-time configuration.

## Inspect

- logging configuration and possible unbounded local log growth;
- whether logs may already be shipped externally;
- image tag mutability and reproducibility requirements;
- development-oriented mounts/settings in production-targeted Compose files;
- backup implications for stateful volumes;
- maintenance/recreation behavior;
- health information useful for deployment verification;
- runtime versus build-time configuration distinctions;
- environment-specific Compose overrides/profiles when supplied.

## Logging

Docker-local logs can consume host disk if left unbounded, but do not assume local JSON logs are the only log sink.

If no rotation is configured:

- identify possible local disk-growth risk;
- validate whether logging is shipped/managed elsewhere if material;
- recommend rotation only when relevant to the actual runtime.

## Image Tags and Reproducibility

Image tags are mutable; digests are immutable.

Treat mutable tags as a reproducibility/update-policy tradeoff, not an automatic security defect.

Validate whether the project prioritizes:

- exact reproducibility;
- automatic patch uptake;
- explicit release management.

Digest pinning improves immutability but requires an intentional update process for new image/security releases.

## Persistence vs Backup

Named volumes survive ordinary container recreation, but they are not backups.

When backup/recovery matters:

- identify which services hold durable data;
- validate whether backups exist externally;
- do not claim Compose persistence alone is sufficient disaster recovery.

## Environment-Specific Configuration

Development and production may intentionally differ in:

- source bind mounts;
- published ports;
- restart policies;
- logging;
- debug flags;
- resource settings.

If target environment is unknown, avoid labeling dev-oriented configuration a production anti-pattern as a confirmed issue.

## Build-Time vs Runtime Configuration

When Compose includes build arguments and runtime environment values with the same names, determine whether the application can actually consume runtime changes.

For static frontend builds, some values may be baked into artifacts at build time. Do not assume runtime environment variables rewrite already-built assets unless an entrypoint/runtime substitution mechanism is visible or known.

## Deployment Verification

Healthchecks can make deploy verification stronger, but do not equate `docker compose up -d` with application health.

If deployment behavior is discussed, keep recommendations scoped to what Compose can expose rather than expanding into a full CI/CD review.