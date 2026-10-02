# Concurrency and Module Architecture

## 1. Goal

Keep local safety processing independent of slow cloud operations.

Do not create microservices.

Use one Python process with separated workers/threads/queues.

## 2. Proposed execution model

```text
Camera Capture Worker
        ↓
Frame Buffer
        ↓
Local Perception Worker
        ↓
State / Hazard Engine
        ↓
Alert Manager
        ↓
TTS Worker

Cloud Worker ← Digest Scheduler
      ↑
      └── User Query Router

Persistence Worker
```

## 3. Camera capture

Camera capture should run continuously.

Target:
- capture around 15-30 FPS if hardware permits

Do not necessarily run YOLO on every captured frame.

## 4. Local inference

Initial target:
- approximately 3-5 inference cycles/sec

This is an engineering starting point, not a fixed requirement.

The implementation should measure actual:
- inference time
- FPS
- CPU
- RAM

## 5. Frame buffer

Use a bounded latest-frame buffer.

For local perception:
- newest frame matters more than old frames

For digest:
- explicitly capture the required two frames

Avoid an unlimited frame queue.

## 6. StateStore concurrency

Use a thread-safe `StateStore`.

Simple initial design:
- `threading.RLock`
- controlled mutation methods
- snapshot method

Example:

```python
with state_store.lock:
    snapshot = state_store.snapshot()
```

Prefer returning a snapshot rather than exposing internal mutable collections.

## 7. Cloud worker

Cloud work must run outside the local perception loop.

Possible implementation:
- dedicated thread
- `concurrent.futures`
- asyncio task

Choose the simplest reliable option.

Do not introduce Celery/Redis.

## 8. TTS worker

TTS should have a priority-aware queue.

Queue behavior:
- P0 interrupts
- P1 priority queue
- P2 deferred queue
- P3 discarded

## 9. Digest scheduler

The scheduler:
1. waits for interval
2. captures frame A
3. waits frame gap
4. captures frame B
5. submits cloud request
6. returns control immediately to local system

## 10. Persistence worker

Database writes should not block the camera/perception loop.

A simple bounded event queue is sufficient.

If implementation complexity becomes unnecessary, low-frequency SQLite writes may happen synchronously outside the critical loop.

## 11. User query

Push-to-talk is the initial STT activation strategy.

Reason:
- simpler
- fewer false activations
- less privacy complexity
- easier prototype evaluation

Always-listening/wake-word mode is deferred.

## 12. Module boundaries

Recommended:

```text
app/
├── main.py
├── config.py
│
├── camera/
│   └── capture.py
│
├── perception/
│   ├── detector.py
│   ├── tracker.py
│   └── observations.py
│
├── domain/
│   ├── models.py
│   ├── enums.py
│   └── state.py
│
├── spatial/
│   ├── zones.py
│   └── proximity.py
│
├── hazards/
│   ├── assessor.py
│   └── rules.py
│
├── events/
│   ├── lifecycle.py
│   └── correlation.py
│
├── alerts/
│   ├── manager.py
│   ├── queue.py
│   └── tts.py
│
├── cloud/
│   ├── client.py
│   ├── schemas.py
│   └── prompts.py
│
├── digest/
│   └── scheduler.py
│
├── speech/
│   └── stt.py
│
├── persistence/
│   ├── database.py
│   └── repository.py
│
├── logging/
│   └── structured.py
│
└── tests/
```

The exact structure can change slightly during implementation, but responsibilities should remain separated.

## 13. Dependency direction

Preferred:

```text
camera → perception → domain/state → hazards → events → alerts
                                      ↑
                                    cloud
```

Domain code should not depend directly on:
- OpenCV
- Gemini SDK
- TTS library
- hardware-specific APIs

Use adapters at boundaries.
