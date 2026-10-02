# State Schema

## 1. StateStore

`StateStore` is the application source of truth for current live state.

It should expose methods rather than allowing arbitrary dictionary mutation.

Example conceptual API:

```python
state_store.add_observation(observation)
state_store.update_entity(entity)
state_store.get_snapshot()
state_store.add_hazard(hazard)
state_store.update_hazard(hazard)
state_store.resolve_hazard(hazard_id)
state_store.add_alert(alert)
state_store.update_user_state(...)
state_store.update_digest_state(...)
```

The internal implementation must be thread-safe.

Preferred simple approach:
- `threading.RLock`
- controlled mutations
- snapshot reads

Do not introduce a distributed state system.

## 2. SystemState

```text
SystemState
├── SceneState
│   ├── observations
│   ├── tracked_entities
│   └── hazards
│
├── AlertState
│   ├── active_alerts
│   ├── cooldowns
│   └── last_spoken
│
├── UserState
│   ├── mode
│   ├── active_query
│   └── preferences
│
└── DigestState
    ├── enabled
    ├── collected_frames
    ├── last_digest
    └── next_digest
```

## 3. Observation schema

```text
Observation
├── id: UUID
├── timestamp: datetime
├── source: ObservationSource
├── type: string
├── confidence: float
├── location: SpatialLocation?
├── bbox: BoundingBox?
└── attributes: dict
```

Confidence should be normalized to `[0, 1]`.

## 4. BoundingBox

```text
BoundingBox
├── x1
├── y1
├── x2
└── y2
```

Prefer normalized coordinates internally where practical.

## 5. SpatialLocation

Initial values:

```text
OUTSIDE
LEFT
FRONT_LEFT
FRONT_CENTER
FRONT_RIGHT
RIGHT
```

Spatial location is camera-centric.

## 6. TrackedEntity

```text
TrackedEntity
├── track_id
├── object_type
├── confidence
├── bbox
├── bottom_center
├── zone
├── apparent_size
├── size_change_rate
├── first_seen
├── last_seen
├── current_risk
└── status
```

Status:

```text
ACTIVE
LOST
RESOLVED
```

## 7. Apparent size

No metric distance.

Possible definition:

```text
bbox_area / image_area
```

Alternative:
```text
bbox_height / image_height
```

Choose one during implementation and use it consistently.

Classify:
- SMALL
- MEDIUM
- LARGE

## 8. Hazard

```text
Hazard
├── hazard_id
├── type
├── category
├── priority
├── confidence
├── location
├── related_entities
├── supporting_observations
├── created_at
├── updated_at
├── last_alerted_at
└── status
```

Status:
```text
ACTIVE
MONITORING
RESOLVED
```

## 9. Alert

```text
Alert
├── alert_id
├── hazard_id
├── event_id
├── priority
├── message
├── created_at
├── spoken_at
├── status
└── suppression_reason
```

Status:
```text
PENDING
SPOKEN
SUPPRESSED
CANCELLED
```

## 10. UserState

```text
UserState
├── mode
├── active_query
└── preferences
```

Modes:

### NORMAL
- P0 immediate
- P1 immediate
- P2 contextual
- P3 silent

### DIGEST
- P0 immediate
- P1 immediate
- P2 digest
- P3 digest/silent

### MINIMAL
- P0 immediate
- P1 concise
- P2 suppressed
- P3 suppressed

P0/P1 must not be disabled by MINIMAL mode.

## 11. DigestState

```text
DigestState
├── enabled
├── collected_frames
├── last_digest
└── next_digest
```

The frame buffer must be bounded.

## 12. TTLs

The following should be configuration values:

```text
TRACK_TTL
OBSERVATION_TTL
HAZARD_TTL
CLOUD_OBSERVATION_TTL
ALERT_COOLDOWN
```

Exact values should be tuned through testing rather than presented as scientifically established constants.

## 13. State invariants

1. Every active hazard has a unique ID.
2. Every alert references a hazard.
3. Every alert references the event that caused it.
4. Every hazard has supporting evidence.
5. Expired observations are removed.
6. Live state remains bounded.
7. Cloud results cannot bypass validation.
8. Raw frames are not stored in long-lived state.
