# Implementation Plan

## 1. Implementation philosophy

Build from the inside out.

Do not start by building the complete camera + cloud + speech application.

First make the domain logic deterministic and testable.

## 2. Phase 1: project skeleton

Create:
- Python environment
- dependency file
- `.env.example`
- `.gitignore`
- application package
- test package
- configuration system
- logging

## 3. Phase 2: domain

Implement:
- enums
- Observation
- BoundingBox
- TrackedEntity
- Hazard
- Alert
- Event
- UserState
- DigestState
- SystemState

Write unit tests immediately.

## 4. Phase 3: StateStore

Implement:
- thread-safe state
- add/update/remove operations
- snapshot
- TTL cleanup

Test without camera.

## 5. Phase 4: spatial engine

Implement:
- normalized coordinates
- bottom-center
- zones
- walking corridor
- apparent size
- size growth

Test with synthetic bounding boxes.

## 6. Phase 5: local perception

Implement:
- camera adapter
- YOLOv8n adapter
- BoT-SORT adapter
- conversion to Observation/TrackedEntity

Keep model-specific code outside domain models.

## 7. Phase 6: hazard engine

Implement deterministic rules.

Start with:
- approaching vehicle
- path obstruction
- approaching person
- stationary relevant person
- environmental semantic hazard from cloud

Do not attempt every possible hazard.

## 8. Phase 7: priority engine

Implement:
- P0
- P1
- P2
- P3
- escalation/de-escalation

Write unit tests for boundary cases.

## 9. Phase 8: event lifecycle

Implement:
- create
- update
- escalate
- resolve
- reappear
- archive

## 10. Phase 9: alert manager

Implement:
- deduplication
- cooldown
- suppression
- priority queue
- P0 interruption
- concise message generation

## 11. Phase 10: TTS

Create a TTS adapter.

The domain should not depend on a specific speech library.

## 12. Phase 11: cloud adapter

Implement:
- multimodal request
- timeout
- retry
- structured response
- validation
- conversion to Observation

Keep provider-specific code inside `cloud/`.

## 13. Phase 12: digest

Implement:
- 30-second scheduler
- two-frame capture
- asynchronous cloud request
- digest result integration

Verify that local inference continues during cloud requests.

## 14. Phase 13: STT/query

Use push-to-talk initially.

Flow:

```text
Push-to-talk
→ STT
→ query router
→ current frame + state
→ cloud
→ response
→ TTS
```

## 15. Phase 14: SQLite

Persist:
- events
- alert metadata
- configuration if useful
- experiment logs

Do not persist raw frames by default.

## 16. Phase 15: integration

Run:
- camera
- detector
- tracker
- state
- hazard engine
- alert manager
- TTS
- cloud
- digest

as one modular application.

## 17. Phase 16: failure testing

Intentionally test:
- unplug camera
- cloud timeout
- cloud 503
- invalid JSON
- TTS failure
- tracker reset
- long-running execution

## 18. Phase 17: benchmark

Measure:
- inference latency
- alert latency
- CPU
- RAM
- alerts/minute
- cloud latency
- redundant alerts

## 19. Phase 18: research evaluation

Run baseline and proposed system on the same scenario recordings.

Collect structured results.

## 20. Stretch goals

Only after MVP works:
- currency recognition
- face recognition
- uncertainty-triggered cloud escalation
- better local semantic detector
- optional hardware
- IMU
