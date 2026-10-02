# Domain Model

## 1. Vocabulary

### Observation

An interpreted piece of evidence produced by a subsystem.

Examples:
- YOLO detected person
- cloud detected possible pothole
- user asked a question

An observation is NOT automatically a hazard.

### TrackedEntity

A persistent representation of an object across frames.

### Hazard

A risk-bearing interpretation of one or more observations.

Example:

person + front-center + walking corridor + increasing apparent size
→ potential collision hazard

### Event

A meaningful change in the lifecycle of a hazard.

Examples:
- created
- escalated
- resolved
- reappeared

### Alert

A user-facing communication generated from an event.

### State

Current live system truth maintained by `StateStore`.

### History

Persistent metadata stored in SQLite.

SQLite is historical/configuration storage, not the source of truth for current scene state.

## 2. Core separation

The system must preserve:

Observation != Hazard != Event != Alert

Example:

YOLO:
person detected

does not imply:

danger

The hazard engine determines whether the observation becomes relevant risk.

The event engine determines whether something meaningful changed.

The alert manager determines whether the user should hear something.

## 3. Sources

Initial source enum:

```text
LOCAL_YOLO
CLOUD_VLM
USER
SYSTEM
```

Future extension:
```text
FACE
CURRENCY
```

## 4. Domain relationships

```text
Observation
    |
    | contributes evidence
    v
Hazard
    |
    | changes state
    v
Event
    |
    | produces
    v
Alert
```

Multiple observations can support one hazard.

Example:

LOCAL_YOLO(person)
+
tracking(size increasing)
+
zone(front_center)
=
Hazard(potential_collision)

A cloud observation may add supporting evidence to an existing hazard or create a separate semantic hazard.

## 5. Threat categories

### T1 Immediate collision

Examples:
- rapidly approaching vehicle
- moving cyclist entering walking path
- large object immediately blocking path

### T2 Path obstruction

Examples:
- chair
- box
- tree
- pole
- garbage
- debris
- construction barrier

Only relevant when spatially related to the walking path.

### T3 Environmental/structural hazard

Examples:
- pothole
- hole
- open drain
- drop-off
- stairs
- construction
- fire/smoke
- unusual road obstruction

Many of these are cloud-first in the MVP.

### T4 Traffic/navigation hazard

Examples:
- moving vehicle
- blocked sidewalk
- crosswalk
- traffic light
- road crossing context

A traffic sign is not automatically a hazard.

### T5 Informational object

Examples:
- person
- shop
- bus
- building
- parked vehicle
- landmark
- ordinary sign

Normally silent unless relevant, queried, or included in digest.

## 6. Important semantic rule

Object class does not determine priority by itself.

Example:

```text
person + far away + outside corridor
→ P3
```

while:

```text
person + central corridor + large + increasing
→ P0/P1
```

depending on configured thresholds.

Similarly:

```text
car + parked + outside path
→ P3
```

while:

```text
car + moving + entering path
→ P0/P1
```

## 7. Limitations

The system is camera-centric.

It does not know:
- exact metric distance
- exact pedestrian intent
- true walking trajectory
- complete 3D geometry

Head direction is not assumed to equal walking direction.

The system should therefore use conservative language such as:
- approaching
- potentially blocking
- possible hazard

rather than absolute claims when evidence is uncertain.
