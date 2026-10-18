Tags: [[3 - Tags/database|database]] [[3 - Tags/data streaming|data streaming]]

# Event Sourcing

## Core Idea
Event sourcing records accepted state changes as an append-only sequence of events. The event log is the authoritative history; current state can be derived by processing those events in order. A command is a request to change state, while an event records a change that has already been accepted.

## How It Works
The command handler checks whether a request is valid against the current rules and state. If accepted, it appends the resulting event. Consumers build read models or trigger other work from the event stream. A projection should not need to repeat command validation, because the logged event is already a fact.

An event history can explain how state arose and permit new projections or rebuilding an incorrect projection. This requires enough retained events and stable interpretation of old event formats.

## Limits
- Event order matters when state changes depend on earlier events.
- Replaying a consumer must not accidentally repeat external effects such as payments or messages.
- A projection depending on changing external information needs the historical value or a reproducible lookup.
- Personal information in an immutable history complicates retention and deletion.
- Events describe what happened; they should not be treated as commands that might still fail validation.

## Related Concepts
- [[Event Streaming and Log-Based Storage]]
- [[CQRS]]
- [[System of Record and Derived Data]]

# References
- [[2 - Source Materials/Books/Design Data-Intensive Application/3.1 Data Models and Query Languages]]
