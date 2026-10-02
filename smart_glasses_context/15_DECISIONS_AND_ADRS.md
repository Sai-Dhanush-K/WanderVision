# Architecture Decisions and ADRs

## ADR-001 Modular monolith

Decision:
Use one Python application with modular components.

Reason:
The prototype does not need distributed infrastructure.

Rejected:
- microservices
- separate services for perception/cloud/TTS

## ADR-002 In-memory live state

Decision:
Use a thread-safe StateStore in RAM.

Reason:
Live scene state changes frequently and must be fast.

SQLite is historical/configuration storage.

## ADR-003 SQLite persistence

Decision:
Use SQLite for metadata and event history.

Reason:
Simple, local, sufficient for a prototype.

Do not persist raw frames by default.

## ADR-004 Local-first safety

Decision:
Immediate safety alerts must not depend on cloud.

Reason:
Cloud introduces:
- latency
- connectivity failures
- transient API failures
- model uncertainty

## ADR-005 YOLOv8n

Decision:
Use YOLOv8n initially.

Reason:
Lightweight, practical, available for local experimentation.

The project is not proposing YOLO as research novelty.

## ADR-006 BoT-SORT

Decision:
Use BoT-SORT for tracking.

Reason:
The camera is moving and persistent tracking is needed.

## ADR-007 No metric depth in MVP

Decision:
Use image-space geometry and apparent proximity.

Reason:
Depth estimation would add significant complexity and hardware/model dependencies.

## ADR-008 Deterministic risk rules

Decision:
Use deterministic hazard/priority rules.

Reason:
- explainability
- testability
- limited project scope
- no need to train another model

## ADR-009 Cloud as observation source

Decision:
Cloud output becomes validated observations.

Cloud cannot directly:
- set P0
- trigger TTS
- mutate internal state without validation

## ADR-010 Cloud P0 restriction

Decision:
Cloud-only observations are capped at P1.

Reason:
Reduce impact of hallucination and latency.

## ADR-011 Periodic digest

Decision:
30-second digest with two frames 15 seconds apart.

Reason:
Provides semantic environmental context without continuous cloud streaming.

## ADR-012 Push-to-talk STT

Decision:
Use push-to-talk initially.

Reason:
Simpler and more privacy-friendly than always-listening speech recognition.

## ADR-013 No currency in MVP

Decision:
Currency/coin recognition is deferred.

Reason:
Not central to research question and increases scope.

## ADR-014 Face recognition deferred

Decision:
Face recognition is optional future work.

If added:
- local only
- explicit enrollment
- small enrolled set
- query-driven or explicitly enabled
- no cloud face upload

## ADR-015 No direct cognitive-load claim

Decision:
Measure redundant alerts and auditory alert rate.

Reason:
Cognitive load requires appropriate human-subject methodology.

## ADR-016 No exact distance

Decision:
Use apparent proximity.

Reason:
No calibrated depth sensor/model in MVP.

## ADR-017 No navigation system

Decision:
Do not implement route planning/GPS/maps.

Reason:
The research focus is perception and information delivery.

## ADR-018 Bounded work queues

Decision:
Frame buffers, cloud queues, and TTS queues must be bounded.

Reason:
Prevent memory growth and stale work.

## ADR-019 Domain/provider separation

Decision:
Domain models must not depend directly on OpenCV/Gemini/TTS implementations.

Reason:
Makes testing and future replacement easier.

## ADR-020 No raw image retention by default

Decision:
Frames are ephemeral.

Reason:
Privacy and storage minimization.
