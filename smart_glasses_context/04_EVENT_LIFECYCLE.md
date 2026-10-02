# Event Lifecycle and Alert Management

## 1. Event lifecycle

Core lifecycle:

```text
OBSERVED
  ↓
CORRELATED
  ↓
ASSESSED
  ↓
PRIORITIZED
  ↓
ALERT_PENDING
  ↓
ALERTED
  ↓
MONITORED
  ↓
REASSESSED / RESOLVED
  ↓
ARCHIVED
```

## 2. Observation lifecycle

An observation:
1. enters the system
2. is normalized
3. is correlated with tracked entities/hazards where possible
4. contributes to hazard assessment
5. eventually expires

## 3. Hazard creation

Create a hazard when evidence satisfies a configured rule.

Example:

```text
vehicle
+
front_center
+
walking corridor
+
high confidence
+
apparent expansion
=
approaching_vehicle hazard
```

## 4. Hazard persistence

A hazard that remains materially unchanged should not continuously create alerts.

Example:

```text
person remains stationary
in same location
same risk
same priority
```

Do not say:

> Person.

every frame.

## 5. Hazard escalation

A new alert may be generated when:

- priority increases
- apparent size increases materially
- object enters the central walking corridor
- approaching signal becomes stronger
- a new supporting observation changes risk
- the user changes mode/direction
- hazard reappears after resolution

## 6. Hazard resolution

Resolve when:
- tracked entity disappears for configured TTL
- hazard evidence expires
- hazard leaves relevant corridor
- cloud observation expires and has no remaining evidence
- contextual rule is no longer satisfied

Do not immediately delete resolved hazards from history. Keep metadata for evaluation.

## 7. Alert suppression

A repeated alert should be suppressed if:
- same hazard/entity
- same or equivalent priority
- same relevant location
- no material risk change
- inside cooldown period

Suppression is a first-class event.

## 8. Suppression breaking conditions

Cooldown can be broken when:
- priority increases
- apparent size changes significantly
- approaching state changes
- hazard enters a more dangerous zone
- object disappears and reappears
- user changes mode
- new evidence materially changes interpretation

## 9. Alert queue

Suggested behavior:

```text
P0
 ↓
interrupt current TTS
 ↓
speak immediately

P1
 ↓
high-priority queue

P2
 ↓
deferred/context queue

P3
 ↓
discard/silent
```

The queue must prevent stale low-priority messages from blocking safety alerts.

## 10. TTS interruption

P0 must be able to interrupt current speech.

Implementation should expose something conceptually like:

```python
tts.speak(message, priority=P0)
tts.cancel_current()
```

Exact TTS engine is an adapter decision.

## 11. Alert message rules

P0/P1:
- short
- concrete
- spatial when useful
- avoid unnecessary explanation

Examples:
- "Vehicle approaching from right."
- "Obstacle directly ahead."
- "Person approaching from left."
- "Possible pothole ahead."

Avoid:
- long descriptions
- repeated confidence values
- model jargon

## 12. Important distinction

A threat is not a speech event.

The system may detect:
- hazard
- no alert

because the hazard is already known and unchanged.

This distinction is central to reducing redundant auditory feedback.
