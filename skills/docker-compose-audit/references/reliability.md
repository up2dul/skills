# Reliability Audit Rules

Use this reference for persistence, health/readiness, dependencies, restart behavior, startup coupling, and graceful shutdown.

## Inspect

- stateful services without appropriate persistent storage;
- persistence that does not match actual durability requirements;
- health/readiness assumptions;
- `depends_on` usage;
- restart policies;
- graceful shutdown needs;
- `init: true` where child-process handling materially matters;
- lifecycle operations coupled to long-running process startup;
- volume declarations whose absence could cause data loss during recreation.

## Persistence

Do not equate persistence with backup.

For every stateful-looking service, determine whether its state must survive container recreation.

Examples:

- database data usually requires persistence;
- cache data may be disposable;
- Redis may be cache-only, session/state storage, a queue/broker, or multiple roles;
- queue durability requirements depend on application semantics.

If role/durability is unknown, use `NEEDS VALIDATION` rather than prescribing a volume.

## Health and Readiness

A running process is not the same as a ready application.

Audit healthchecks when they materially support:

- dependency readiness;
- deployment verification;
- operational visibility.

Do not demand healthchecks for every service and do not recommend superficial probes that only prove a process exists.

## `depends_on`

Short-form `depends_on` establishes dependency/start ordering, not health readiness.

If a dependent service truly requires a healthy dependency before startup, validate whether:

- the dependency has a meaningful healthcheck;
- health-based dependency conditions are appropriate;
- the application already handles retries itself.

Do not assume startup ordering is required when the application can tolerate/retry dependency unavailability.

## Restart Policies

Assess restart policy against service lifecycle.

Consider whether:

- long-running services should restart after failure;
- one-shot jobs should not restart indefinitely;
- manual stops should remain stopped;
- restart loops can hide persistent application failure.

Do not treat `unless-stopped` or `always` as universally correct.

## Graceful Shutdown

For workers, consumers, long-running requests, or task processors, consider:

- `stop_grace_period`;
- `stop_signal`;
- how the process handles `SIGTERM`;
- whether active work can exceed the default shutdown grace window.

If task duration/termination semantics are unknown, mark as `NEEDS VALIDATION`.

## `init: true`

`init: true` can help with signal forwarding and zombie child-process reaping.

Treat it as context-dependent. It may be useful for runtimes that spawn children, but is not a universal requirement if the main process already handles PID 1 responsibilities correctly.

## Startup Commands and One-Off Lifecycle Work

If `command` or `entrypoint` couples schema migrations, setup jobs, or other one-off operations directly to every service start:

- identify the coupling;
- explain the operational consequence;
- consider multi-instance behavior;
- avoid assuming separation is mandatory unless the coupling creates a concrete failure mode.

Migration behavior is especially relevant when multiple replicas could execute the same migration concurrently.