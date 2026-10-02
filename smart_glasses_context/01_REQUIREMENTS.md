# Requirements Specification

## 1. Functional requirements

### R1 Camera input

The system shall continuously capture frames from a camera.

Development environment:
- laptop webcam

Target concept:
- wearable camera

Camera capture must not depend on cloud availability.

### R2 Local perception

The system shall perform local object detection using a lightweight detector.

Initial model:
- YOLOv8n

The local detector provides observations such as:
- class
- confidence
- bounding box

The detector is not itself responsible for deciding whether something is dangerous.

### R3 Tracking

The system shall maintain object identity across frames.

Initial tracker:
- BoT-SORT

Rationale:
- the camera is moving
- persistent object identity is required
- camera-motion compensation is useful

Every active tracked entity has a `track_id`.

### R4 Scene state

The system shall maintain an in-memory representation of the current environment.

State includes:
- observations
- tracked entities
- hazards
- active alerts
- user mode
- digest status
- recent changes

### R5 Hazard assessment

The system shall interpret observations using context.

Inputs include:
- object type
- confidence
- image-space location
- walking corridor membership
- apparent size
- apparent size growth
- persistence
- scene context

### R6 Priority

Each active hazard receives:
- P0
- P1
- P2
- P3

Priority controls speech urgency.

### R7 Alert management

The system shall:
- announce new important hazards
- suppress repeated unchanged hazards
- escalate when risk increases
- re-alert after meaningful changes
- resolve hazards that disappear

### R8 TTS

TTS shall provide concise feedback.

Target:
- P0/P1: approximately 5-8 words

Priority behavior:
- P0 interrupts current speech
- P1 is immediate or queued
- P2 is contextual/deferred
- P3 is silent by default

### R9 User queries

The user can issue spoken questions.

Examples:
- What is in front of me?
- Is anything blocking my path?
- Describe my surroundings.

The query can trigger cloud multimodal analysis.

### R10 Periodic digest

Default digest:
- interval: 30 seconds
- image A: at digest start
- image B: 15 seconds later
- cloud analysis after image B

Digest processing must be asynchronous.

### R11 Cloud scene understanding

The cloud model provides higher-level semantic observations that local YOLO may not provide.

Examples:
- pothole
- stairs
- construction obstruction
- unusual environmental hazard
- scene context

### R12 Local-first safety

Local safety processing shall continue when:
- cloud times out
- cloud returns 503
- cloud returns invalid JSON
- internet is unavailable

Cloud failure must not stop local hazard processing.

### R13 Privacy

Default:
- raw frames are ephemeral
- selected frames sent to cloud only when required
- raw frames are not permanently stored
- logs contain metadata, not images
- face recognition, if added later, is local

### R14 Logging

Log enough information for debugging and evaluation:
- timestamps
- event IDs
- source
- hazard type
- priority
- alert result
- latency
- cloud request result
- failures

### R15 Configurability

Configurable values include:
- detector confidence
- inference interval
- tracking TTL
- observation TTL
- hazard TTL
- alert cooldown
- apparent-size thresholds
- approaching threshold
- digest interval
- digest frame gap
- cloud timeout
- retry count

## 2. Non-functional requirements

### NFR1 Responsiveness

Local hazard processing should remain responsive even while cloud requests are running.

### NFR2 Bounded memory

The system must not accumulate unlimited:
- frames
- observations
- events
- alerts
- logs

### NFR3 Fault isolation

Failure in one subsystem should not crash unrelated functionality.

### NFR4 Reproducibility

Configuration and dependencies should be pinned/documented.

### NFR5 Testability

Risk rules, priority rules, event lifecycle, schema validation, and deduplication must be testable without a camera.

### NFR6 Explainability

For every alert, the system should be able to explain:
- which hazard produced it
- which observations supported it
- which rule produced its priority
- why it was spoken or suppressed

### NFR7 Privacy by default

No unnecessary raw image retention.

## 3. Out-of-scope requirements

Not required for MVP:
- precise navigation
- route planning
- GPS
- metric depth
- full pedestrian trajectory prediction
- continuous face recognition
- currency recognition
- coin recognition
- hardware glasses integration
