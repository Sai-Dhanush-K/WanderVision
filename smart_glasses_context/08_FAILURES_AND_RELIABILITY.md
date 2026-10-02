# Failure States and Reliability

## 1. Design principle

The system must degrade safely.

The main rule is:

> Cloud failure must never stop local perception and local alerting.

## 2. Camera failure

Symptoms:
- device disconnected
- frame read failure

Behavior:
1. mark camera degraded
2. stop claiming current scene awareness
3. attempt reconnection
4. resume after successful capture

Never fabricate scene state.

## 3. Detector failure

If YOLO crashes or returns invalid output:
- log error
- mark local perception degraded
- attempt model recovery/reinitialization
- continue other nondependent functions where possible

Do not create fake detections.

## 4. Tracker failure

If tracking fails:
- reinitialize tracker
- allow new tracks
- temporarily rely on current-frame detections
- avoid repeated alert storms during recovery

## 5. Cloud timeout

Behavior:
- stop waiting after configured timeout
- log failure
- continue local processing
- mark digest/query degraded

## 6. Cloud HTTP/API error

For transient errors:
- bounded retry
- exponential backoff

For persistent errors:
- fail request
- continue local operation

## 7. Invalid cloud response

Never feed malformed cloud output into the hazard engine.

Pipeline:

```text
invalid response
→ reject
→ log
→ mark request failed
```

## 8. STT failure

Behavior:
- retry if appropriate
- provide concise failure indication
- do not block camera loop

## 9. TTS failure

Behavior:
- retry once
- log failure
- continue hazard processing

The hazard should remain in state even if speech failed.

## 10. SQLite failure

SQLite is not the live state source.

Therefore:
- keep live state in RAM
- log persistence failure
- retry persistence
- do not block local safety loop on database writes

## 11. Stale observations

Every observation has a lifetime.

After TTL:
- remove from active state
- retain only historical metadata if required

## 12. Bounded queues

Cloud requests, TTS alerts, and frame buffers must have bounded capacity.

If overloaded:
- discard stale low-priority work
- preserve P0/P1 processing
- do not allow old P2/P3 messages to accumulate indefinitely

## 13. Cloud request cancellation

If a new urgent local hazard appears while a cloud request is running:
- local processing continues
- urgent alert does not wait
- cloud result can arrive later
- stale cloud result should be checked before mutating state

## 14. Stale cloud results

Before applying a cloud observation:
- check timestamp
- check TTL
- check whether the corresponding scene is still relevant

A stale observation must not revive an old hazard incorrectly.

## 15. System degraded state

Possible states:

```text
NORMAL
DEGRADED_CAMERA
DEGRADED_LOCAL_PERCEPTION
DEGRADED_CLOUD
DEGRADED_TTS
DEGRADED_PERSISTENCE
```

Multiple degraded conditions can coexist.

## 16. Recovery

Every degraded subsystem should attempt recovery without restarting the entire application when practical.
