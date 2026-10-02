# Smart Assistive Glasses Context Pack

This folder contains the project context and engineering contracts for the smart assistive glasses B.Tech research prototype.

ALL context is in smart_glasses_context folder

Start with `18_CONTEXT_INDEX.md`.

The architecture is intentionally a modular monolith with:
- local YOLOv8n perception
- BoT-SORT tracking
- stateful hazard assessment
- P0-P3 priority
- event lifecycle and alert suppression
- asynchronous cloud multimodal scene understanding
- 30-second digest
- push-to-talk user queries
- TTS
- SQLite metadata persistence

Currency/coin recognition and face recognition are deferred extensions.

Do not add microservices, Redis, Celery, LangChain, RAG, agents, depth estimation, navigation, or custom ML training unless explicitly justified and documented as an architecture change.
