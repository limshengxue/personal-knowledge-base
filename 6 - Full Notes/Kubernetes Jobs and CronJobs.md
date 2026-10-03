2026-10-03 18:49

Tags: [[3 - Tags/kubernetes|kubernetes]]

# Kubernetes Jobs and CronJobs
- A Job runs pods to accomplish a finite task; a CronJob creates Jobs on a schedule.
- Use [[Kubernetes Deployment]] for continuously maintained replicas instead of finite completion.

## Job Controls
- `parallelism`: how many pods may run concurrently.
- `completions`: required successful completions for applicable Job modes.
- `backoffLimit`: retry/failure boundary; capitalization matters.
- `activeDeadlineSeconds`: maximum active duration.
- `ttlSecondsAfterFinished`: optional cleanup after completion/failure.

Containers finish when the work ends. Completed pod objects can remain for inspection until deletion or configured cleanup; they are not always immediately removed.

## CronJob Controls
- `schedule` defines recurring creation times.
- `startingDeadlineSeconds` limits acceptable lateness for starting a scheduled Job.
- `concurrencyPolicy` controls overlapping Jobs created by that CronJob.
- Scheduling can produce delayed, missed, or duplicate execution, so make work idempotent where possible.

## Inspection
Use `kubectl get jobs,cronjobs`, pod logs, and events to distinguish scheduling, retries, and cleanup failures.

# References
[[17 - Advanced Concepts]]
[Jobs](https://kubernetes.io/docs/concepts/workloads/controllers/job/)
[CronJobs](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/)
