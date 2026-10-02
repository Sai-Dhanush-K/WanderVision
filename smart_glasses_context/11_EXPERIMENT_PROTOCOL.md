# Research Experiment Protocol

## 1. Research question

> Can a lightweight stateful edge-cloud information-delivery architecture reduce redundant assistive alerts while preserving timely delivery of important hazards?

## 2. Hypotheses

### H1

The proposed stateful adaptive policy reduces redundant alert rate compared with the baseline.

### H2

The proposed policy maintains an acceptable critical hazard miss rate.

### H3

The additional orchestration does not introduce unacceptable alert latency.

## 3. Baseline

```text
Camera
 ↓
YOLOv8n
 ↓
Confidence filter
 ↓
Basic cooldown
 ↓
TTS
```

The baseline may use a basic cooldown to prevent pathological frame-by-frame repetition.

It does not use:
- tracking-aware hazard state
- contextual hazard lifecycle
- adaptive priority
- state-based suppression
- periodic cloud digest

## 4. Proposed

```text
Camera
 ↓
YOLOv8n
 ↓
BoT-SORT
 ↓
Observations
 ↓
StateStore
 ↓
Hazard Assessment
 ↓
Priority Engine
 ↓
Event Lifecycle
 ↓
Alert Manager
 ↓
TTS
```

Cloud operates asynchronously:

```text
Digest / User Query
 ↓
Cloud VLM
 ↓
Validated observations
 ↓
StateStore
 ↓
Hazard Engine
```

## 5. Scenario set

Initial scenarios:

1. stationary person
2. approaching person
3. person outside corridor
4. parked car
5. approaching vehicle
6. bicycle crossing
7. obstacle center
8. obstacle side
9. pothole
10. construction obstruction
11. stairs
12. blocked sidewalk
13. crowded scene
14. multiple pedestrians
15. vehicle + pedestrian
16. obstacle + pedestrian
17. changing scene
18. mixed environment

## 6. Ground truth

Each scenario must have documented ground truth.

At minimum:

```text
scenario_id
scene_description
expected_critical_hazards
expected_important_hazards
expected_informational_objects
expected_relevant_objects
```

Do not evaluate only by whether the demo "sounds good."

## 7. Primary metric

### Redundant Alert Rate

```text
redundant alerts
-----------------
total alerts
```

A redundant alert is substantially the same hazard information repeated while:
- priority has not materially changed
- risk has not materially changed
- location/relevance has not materially changed

Define this operationally before scoring.

## 8. Critical Hazard Miss Rate

```text
critical hazards missed
-----------------------
critical hazards present
```

Define what counts as a critical hazard before running the experiment.

## 9. Alert latency

Measure:

```text
first valid hazard evidence
→ alert initiation
```

Report:
- mean
- median
- p95 where sample size permits
- maximum where useful

## 10. Alerts per minute

Measure auditory load proxy:

```text
total alerts
-------------
session duration
```

Do not call this cognitive load.

## 11. Cloud usage

Measure:
- cloud requests/session
- images/session
- failed requests
- average cloud latency

## 12. Resource usage

Measure:
- CPU
- RAM
- local inference latency
- effective local inference rate

## 13. Reproducibility

Run baseline and proposed systems on:
- same scenarios
- same recordings
- same hardware where possible
- same detector model
- same confidence threshold

## 14. Human study boundary

Unless a formal human-subject experiment is conducted, do not claim:
- reduced cognitive load
- improved user comfort
- improved usability
- improved accessibility

Instead say:
- reduced redundant auditory feedback
- reduced alert frequency
- preserved measured hazard response

## 15. Results table

Recommended:

| Metric | Baseline | Proposed |
|---|---:|---:|
| Redundant Alert Rate | | |
| Critical Hazard Miss Rate | | |
| Mean Alert Latency | | |
| Median Alert Latency | | |
| Alerts/minute | | |
| Cloud requests/session | N/A | |
| CPU | | |
| RAM | | |

## 16. Limitations to report

- small controlled scenario set
- camera-centric perception
- no metric depth
- limited cloud model validation
- no human-subject cognitive-load study
- development hardware differs from eventual wearable hardware
- YOLOv8n is not a complete hazard detector
- cloud VLM can hallucinate
- tracking can fail under occlusion
