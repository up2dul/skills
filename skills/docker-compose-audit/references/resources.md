# Resources and Performance Audit Rules

Use this reference for host exhaustion risk, process counts, worker concurrency, and explicit resource constraints.

## Inspect

- CPU constraints;
- memory constraints;
- PID/process constraints;
- worker/process counts visible in commands;
- services that may spawn many children;
- obvious resource duplication or contention visible from Compose.

## Missing Resource Limits

Absence of resource limits is not automatically an issue.

If host capacity and workload are unknown:

- identify the potential contention condition;
- use `NEEDS VALIDATION` when risk depends on VM CPU/RAM or workload;
- do not invent arbitrary limits.

A small single-host deployment may benefit from limits to prevent one service from exhausting the VM, but inappropriate limits can also cause avoidable failures.

## Worker and Process Counts

If commands expose process counts such as web workers, Celery concurrency, or similar settings:

- report their combined visible concurrency;
- validate against host CPU/RAM and workload type;
- consider both CPU-bound and memory-bound implications;
- avoid generic formulas unless runtime-specific evidence supports them.

Do not claim a number is excessive solely because it is larger than CPU count; workload behavior matters.

## PID Limits

`pids_limit` can reduce process-exhaustion blast radius for services that spawn children.

Treat it as hardening unless process behavior or threat model makes it materially important. Do not recommend an arbitrary numeric limit without evidence.

## Database and Cache Resources

If resource constraints are visible for databases/caches, consider whether they conflict with persistence, startup, or expected workloads.

If no tuning context is available, do not expand the Compose audit into database-performance tuning.