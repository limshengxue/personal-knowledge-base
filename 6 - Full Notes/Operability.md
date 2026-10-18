Tags: [[3 - Tags/software architecture|software architecture]]

# Operability

## Core Idea
Operability is how easily people can keep a system running and diagnose it when something goes wrong. A service that works only when one person remembers a hidden procedure is hard to operate, even if its normal path is reliable.

## What Good Operability Provides
- Visibility into runtime behavior through useful metrics, logs, and clear failure signals.
- Predictable defaults and behavior that make routine operations safe.
- Documentation and automation for deployment, recovery, and maintenance.
- Recovery procedures that do not depend on one specific machine.

Automation handles repeated work but can hide failures if its actions are not observable. Design the system so operators can understand what it is doing and why. That shortens diagnosis and makes fault recovery more dependable.

## Related Concepts
- [[Reliability and Fault Tolerance]]
- [[Overload and Retry Storms]]
- [[Evolvability]]

# References
- [[2 - Source Materials/Books/Design Data-Intensive Application/1 - Reliable, Scalable, and Maintainable Applications]]
- [[2 - Source Materials/Books/Design Data-Intensive Application/2.1 Non Functional Requirements]]
