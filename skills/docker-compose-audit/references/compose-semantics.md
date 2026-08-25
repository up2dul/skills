# Compose Semantics Reference

Use this reference to keep audit claims technically precise.

## Networking

- Services attached to the same Compose network can normally reach each other by service name.
- Compose creates an implicit default network when custom networks are not declared.
- Publishing a host port is not required for container-to-container communication on a shared network.
- A published port without an explicit host IP normally binds beyond loopback, but actual external reachability still depends on host/network controls.
- A loopback binding such as `127.0.0.1:5432:5432` limits host binding compared with `5432:5432`, but may still be unnecessary if only containers need access.

## Dependencies and Health

- Short-form `depends_on` establishes dependency/start ordering.
- Short-form `depends_on` does not mean the dependency is healthy or application-ready.
- Health-based dependency behavior requires a meaningful healthcheck plus the appropriate condition semantics.
- A container being `running` does not prove the application is healthy.

## Restart and Recreate

- Restart policies govern container restart behavior, not application-level correctness.
- Restarting an existing container does not inherently recreate it from a newly pulled image.
- Configuration/image changes generally require recreation to take effect.

## Volumes and Bind Mounts

- Named volumes persist data across ordinary container recreation.
- Named volumes are not backups.
- Bind mounts expose host filesystem paths directly to containers.
- Writable bind mounts can allow containers to modify host files within the mounted path.

## Secrets, Configs, and Environment

- Environment variables are appropriate for ordinary runtime configuration.
- Sensitive values may warrant narrower secret mechanisms.
- Compose secrets/configs can make file-based values available to selected services.
- Do not infer the contents of an `env_file` when they are not supplied.

## Images

- Image tags are mutable references.
- Image digests provide immutable identity.
- Digest pinning improves reproducibility but creates an explicit image-update responsibility.

## Hardening Controls

The following are context-dependent controls, not mandatory checklist items:

- `init: true`;
- `read_only: true`;
- `cap_drop`;
- `security_opt` such as `no-new-privileges`;
- custom networks;
- CPU/memory limits;
- `pids_limit`;
- `stop_grace_period`.

Recommend them only when a concrete runtime, threat, reliability, or resource concern supports the change.

## Shutdown

- Containers receive a stop signal before forced termination.
- `stop_grace_period` controls how long Compose allows graceful shutdown before force-killing the container.
- Workers and long-running tasks may require validation of termination semantics before changing this value.

## Image-Level Facts

Compose does not reveal every image property.

Do not infer solely from missing Compose fields:

- whether the image defines a non-root `USER`;
- what its entrypoint does internally;
- whether it contains health-probe tools;
- which filesystem paths it must write;
- which Linux capabilities its process genuinely needs.

Request or inspect image/Dockerfile context only when needed to validate a Compose finding.