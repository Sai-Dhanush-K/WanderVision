# Smart Assistive Glasses Prototype - Project Context

## 1. Project overview

This project is a B.Tech research prototype for assistive smart glasses for visually impaired users.

The project is NOT primarily about inventing a new computer-vision model. The research focus is the architecture and information-delivery policy used to combine:

- lightweight local visual perception
- object tracking
- stateful hazard assessment
- priority-based alerts
- event lifecycle management
- selective cloud multimodal scene understanding
- adaptive spoken feedback

The central research question is:

> Can a lightweight stateful edge-cloud information-delivery architecture reduce redundant assistive alerts while preserving timely delivery of important hazards?

The system should be a real, working prototype. It should not be a collection of disconnected demos.

## 2. User/developer context

The developer is a B.Tech CSE student with stronger backend/software experience than ML/CV experience.

Known comfortable technologies:
- Python
- Django/DRF
- PostgreSQL/SQLite
- Git/GitHub
- basic cloud/devops

Newer technologies for this project:
- computer vision
- YOLO
- object tracking
- multimodal vision-language models
- STT/TTS integration

Therefore:
- architecture must remain understandable
- avoid unnecessary frameworks
- avoid premature abstraction
- explain why each component exists
- prefer simple Python modules over infrastructure-heavy solutions

## 3. Core architectural decision

Use a modular monolith.

One Python application/process initially.

Do NOT introduce:
- microservices
- Redis
- Celery
- LangChain
- RAG
- autonomous agents
- separate API server unless a concrete requirement appears

The application should have clean module boundaries without distributed-system complexity.

## 4. High-level architecture

Camera
→ OpenCV
→ YOLOv8n
→ BoT-SORT
→ Observations
→ StateStore
→ Hazard Assessment
→ Priority Engine
→ Event Lifecycle
→ Alert Manager
→ TTS

Cloud path:

StateStore / Digest Scheduler
→ selected frames + relevant state
→ multimodal cloud model
→ validated structured observations
→ StateStore
→ Hazard Assessment
→ Priority Engine
→ Alert Manager

User query path:

User
→ STT
→ Query Router
→ cloud multimodal model
→ validated response/observations
→ TTS

## 5. Core principle

Model output is evidence, not truth.

The architecture separates:

Observation
→ Hazard
→ Event
→ Alert

Therefore:
- a detected person is not automatically a threat
- a cloud statement is not automatically an emergency
- an alert is an application decision, not a model decision

## 6. MVP scope

### Included

- camera capture
- local YOLOv8n object detection
- BoT-SORT tracking
- image-space spatial zones
- apparent proximity estimation
- apparent expansion / approaching signal
- stateful scene representation
- contextual hazard assessment
- P0-P3 priority system
- event lifecycle
- alert deduplication
- local TTS
- push-to-talk STT
- user cloud queries
- 30-second cloud digest
- two digest frames, 15 seconds apart
- structured cloud request/response
- schema validation
- failure/degraded states
- event logging
- baseline vs proposed experiment

### Explicitly deferred

- currency recognition
- coin recognition
- face recognition
- IMU
- metric depth
- depth camera
- navigation/maps
- custom YOLO training
- custom risk ML model
- RAG
- LangChain
- agents
- microservices
- dedicated wearable hardware

## 7. Research contribution

The project should NOT claim novelty in:
- object detection itself
- YOLO
- basic tracking
- message prioritization in general
- information overload as a newly discovered problem

Existing literature already discusses assistive information overload, cognitive load, adaptive feedback, low-redundancy guidance, and wearable usability.

The intended contribution is an implementation/evaluation of a lightweight stateful edge-cloud information-delivery architecture that combines:
- local fast perception
- persistent scene state
- contextual hazard assessment
- event lifecycle
- selective speech
- periodic semantic cloud analysis

## 8. Scientific caution

Do not claim:
- exact distance without depth calibration
- direct cognitive-load reduction without a human-subject cognitive-load study
- that prioritization itself is novel
- that the system guarantees safety
- that cloud VLM output is ground truth

Use terms such as:
- apparent proximity
- apparent expansion
- potential hazard
- assistive alert
- redundant auditory feedback
- engineering target
- prototype evaluation
