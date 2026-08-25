# Security Audit Rules

Use this reference for exposure, privileges, host access, secrets, capabilities, and service isolation.

## Inspect

- public or unnecessarily broad host port bindings;
- `privileged: true`;
- dangerous or unexplained `cap_add`, especially powerful capabilities such as `SYS_ADMIN`;
- host namespace sharing such as `network_mode: host`, `pid: host`, or similar settings;
- host device access;
- sensitive bind mounts, including `/`, Docker/Podman sockets, credentials, SSH material, and system directories;
- writable bind mounts that could safely be read-only;
- unnecessary secret/config exposure between services;
- sensitive values embedded directly in Compose;
- broad `env_file` usage when a service likely needs only a narrow subset;
- unnecessary service-to-service reachability;
- configuration that explicitly weakens container security defaults.

## Always Surface High-Blast-Radius Settings

When present, explicitly surface:

- `privileged: true`;
- Docker or Podman socket mounts;
- host root or sensitive system-directory mounts;
- host networking;
- host PID or other host namespace sharing;
- powerful added capabilities;
- host device passthrough;
- broad writable host bind mounts;
- secrets directly embedded in Compose;
- published database, cache, queue, or administrative ports.

Do not automatically label every occurrence a vulnerability. Explain what capability it grants and validate why it is needed.

## Ports

A published port is not required for container-to-container communication on a shared Compose network.

If a stateful/internal service publishes a host port:

- identify the binding precisely;
- distinguish loopback-only from broad binding;
- validate whether host-level or external clients intentionally need it;
- do not claim internet reachability without network/firewall evidence.

If no host IP is specified, treat broad host binding as an observable condition but external reachability as context-dependent.

## Bind Mounts

Distinguish named volumes from bind mounts.

For bind mounts:

- identify the host path;
- assess sensitivity of that path;
- determine whether write access is necessary;
- prefer read-only access when the use case only requires reads;
- treat Docker socket and host root mounts as especially sensitive.

## Environment and Secrets

Do not say "environment variables are insecure" categorically.

Instead distinguish:

- ordinary runtime configuration;
- sensitive credentials/secrets;
- file-based configuration.

When `env_file` contents are unavailable, do not invent specific secrets. Report only that all values in the file may be injected into the service and validate whether that breadth is intentional.

Compose secrets/configs may improve least privilege when services need narrower file-based access, but do not require them for all configuration.

## Networks

The default Compose network is not inherently insecure.

If all services share one network:

- report reachability accurately;
- classify isolation as hardening unless there is a concrete trust-boundary requirement;
- do not recommend custom networks merely to satisfy a checklist.

## User and Capabilities

Do not assume the container runs as root solely because Compose lacks `user`. The image may define a non-root `USER`.

If execution identity matters to a finding, mark it `NEEDS VALIDATION` unless image metadata or Dockerfile context is available.

Similarly, do not blindly recommend `cap_drop: [ALL]`, `read_only: true`, or `no-new-privileges`. Validate required capabilities and writable paths first unless evidence proves they are unnecessary.