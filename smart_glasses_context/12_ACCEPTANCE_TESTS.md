# Acceptance Tests

## AT-01 Camera

Given a connected camera:
- application starts
- frames are captured
- frame timestamps are available

Pass:
- continuous capture for test duration without application crash

## AT-02 YOLO

Given a visible supported object:
- detector produces a valid observation
- confidence is in `[0,1]`
- bounding box is valid

## AT-03 Tracking

Given a persistent object:
- track ID remains stable across consecutive frames when tracking succeeds
- system does not create a new alert every frame

## AT-04 Spatial zones

Given an object in each configured image region:
- correct zone is produced

## AT-05 Apparent size

Given objects of different apparent image sizes:
- normalized size metric is calculated
- classification is deterministic

## AT-06 Approaching signal

Given a tracked object with sustained apparent-size growth:
- approaching state eventually becomes true

Given one noisy size jump:
- approaching state should not immediately become true

## AT-07 Hazard creation

Given a configured high-risk scene:
- appropriate hazard is created

Given an irrelevant object:
- no unnecessary high-priority hazard is created

## AT-08 Priority

Given P0 conditions:
- priority is P0

Given cloud-only high-severity observation:
- priority cannot exceed P1

## AT-09 Deduplication

Given an unchanged persistent hazard:
- first alert is spoken
- repeated equivalent alerts are suppressed

## AT-10 Escalation

Given an existing P1 hazard that becomes P0:
- new P0 alert is generated despite cooldown

## AT-11 Resolution

Given a hazard whose supporting entity disappears beyond TTL:
- hazard eventually resolves

## AT-12 Reappearance

Given a resolved hazard that reappears:
- new event can be generated

## AT-13 TTS priority

P0:
- interrupts current speech

P1:
- enters high-priority queue

P2:
- does not block P0/P1

## AT-14 Cloud validation

Given valid cloud JSON:
- observation is accepted

Given malformed JSON:
- response is rejected

## AT-15 Cloud failure

Given cloud timeout:
- local YOLO continues
- local hazards continue
- application remains operational

## AT-16 Cloud 503

Given transient cloud 503:
- bounded retry occurs
- local processing remains uninterrupted

## AT-17 Digest

Given digest enabled:
- frame A captured
- frame B captured approximately 15 seconds later
- cloud request occurs
- result is validated

## AT-18 Privacy

During normal execution:
- raw frames are not written to permanent storage
- API key is not written to logs

## AT-19 Bounded state

After long-running test:
- observation collection remains bounded
- frame buffer remains bounded
- low-priority queues do not grow indefinitely

## AT-20 Explainability

For every spoken hazard alert:
- hazard ID exists
- supporting evidence exists
- priority rule can be identified
- event ID exists
