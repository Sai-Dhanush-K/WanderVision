# Project Glossary

## Observation

Evidence produced by a subsystem.

## Tracked entity

A persistent object representation across frames.

## Hazard

A risk-bearing interpretation of one or more observations.

## Threat

Use sparingly. In this project, a threat is an informal term for a hazard with meaningful risk. Prefer `Hazard` in code.

## Event

A meaningful lifecycle change.

## Alert

A user-facing spoken response.

## Priority

Urgency assigned to a hazard:
- P0
- P1
- P2
- P3

## P0

Immediate danger requiring highest urgency.

## P1

Urgent potential hazard.

## P2

Important but non-immediate context.

## P3

Informational content normally kept silent.

## Walking corridor

Image-space approximation of the forward walking region.

## Zone

Camera-centric spatial label.

## Apparent proximity

Image-space proxy based on apparent object size.

Not metric distance.

## Apparent expansion

Increase in apparent object size across tracked observations.

Possible evidence of approach.

Not guaranteed physical motion.

## Digest

Periodic cloud-generated environmental summary.

## StateStore

Application-owned source of truth for live state.

## TTL

Time-to-live after which stale state is removed or reassessed.

## Deduplication

Preventing repeated equivalent alerts.

## Escalation

Increasing hazard priority when risk becomes more urgent.

## Resolution

Determining that active evidence no longer supports a hazard.

## Baseline

Simpler system used for comparison in the experiment.

## Proposed system

Stateful adaptive architecture being evaluated.

## Redundant alert

An alert that repeats substantially the same hazard information without meaningful risk change.

## Cognitive load

A human cognitive construct.

Do not claim it is reduced unless directly measured with an appropriate human-subject study.
