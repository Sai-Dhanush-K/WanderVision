# Instructions for Antigravity / Coding Agent

## 1. Role

You are implementing a B.Tech research prototype.

Do not redesign the architecture casually.

If a requested feature conflicts with these context files, identify the conflict before changing the architecture.

## 2. First priority

Build the MVP before extensions.

MVP:
1. camera
2. YOLOv8n
3. BoT-SORT
4. observations
5. StateStore
6. spatial reasoning
7. hazard engine
8. priority engine
9. event lifecycle
10. alert manager
11. TTS
12. cloud adapter
13. structured cloud response
14. digest
15. push-to-talk query
16. SQLite metadata
17. logging
18. tests

## 3. Do not add

Unless explicitly requested:
- LangChain
- RAG
- agents
- microservices
- Redis
- Celery
- Kubernetes
- message brokers
- custom ML training
- depth estimation
- navigation
- currency
- face recognition

## 4. Coding style

Prefer:
- small functions
- typed Python
- dataclasses/Pydantic where appropriate
- explicit interfaces
- dependency injection at boundaries
- deterministic rules
- testable pure functions

Avoid:
- giant `main.py`
- global mutable dictionaries
- provider SDK calls scattered across modules
- model-specific objects leaking into domain code
- magic constants

## 5. Safety architecture

Never implement:

```text
cloud response → direct TTS
```

Always:

```text
cloud response
→ validation
→ observation
→ StateStore
→ hazard assessment
→ priority
→ event
→ alert manager
→ TTS
```

## 6. Local-first requirement

Cloud calls must never block:
- camera capture
- local detection
- local hazard assessment
- P0/P1 alert processing

## 7. Secrets

Never write API keys into:
- source code
- Markdown
- Git
- logs

Use environment variables.

Provide `.env.example` only with variable names.

## 8. Testing requirement

Before integration, unit test:
- spatial zones
- apparent size
- approaching signal
- priority rules
- deduplication
- escalation
- resolution
- cloud schema validation

## 9. Research reproducibility

Every experiment run should be attributable to:
- configuration
- code version
- model version
- scenario ID
- timestamp

## 10. Logging

Use structured logs where practical.

Useful fields:
- timestamp
- event_id
- hazard_id
- source
- priority
- latency
- request_id
- outcome

## 11. Implementation order

Do not build everything simultaneously.

Recommended:

```text
domain
→ state
→ spatial
→ risk
→ events
→ alerts
→ local perception
→ cloud
→ digest
→ speech
→ persistence
→ integration
→ experiments
```

## 12. Completion rule

The MVP is not complete when the demo works once.

It is complete when:
- acceptance tests pass
- failure paths have been exercised
- baseline/proposed experiment can run
- metrics are collected
- architecture remains understandable
