# Configuration and Thresholds

## 1. Principle

Thresholds should be configurable.

Do not scatter magic numbers throughout source code.

## 2. Example configuration

```yaml
camera:
  device: 0
  capture_fps: 20

perception:
  model: yolov8n.pt
  confidence: 0.45
  inference_interval_ms: 250

tracking:
  tracker: botsort.yaml
  track_ttl_seconds: 1.5

spatial:
  corridor:
    top_width: 0.30
    bottom_width: 0.80
    top_y: 0.35
    bottom_y: 1.00

proximity:
  small_threshold: ...
  medium_threshold: ...
  large_threshold: ...
  approaching_min_growth: ...

alerts:
  cooldown_seconds: 5
  p0_interrupt: true
  p1_queue: true

digest:
  enabled: true
  interval_seconds: 30
  frame_gap_seconds: 15

cloud:
  timeout_seconds: 8
  max_retries: 2

state:
  observation_ttl_seconds: ...
  hazard_ttl_seconds: ...
```

These are examples. Exact values must be measured/tuned.

## 3. Secrets

API keys must come from environment variables or a local `.env` file excluded from Git.

Never:
- commit keys
- put keys in Markdown
- paste keys into source
- store keys in SQLite

## 4. Configuration sources

Recommended precedence:

1. environment variables for secrets
2. configuration file for behavior
3. safe defaults in code

## 5. Threshold tuning

Thresholds should be tuned using the experimental scenarios.

Do not tune until the system perfectly handles only one demo scene.

Prefer:
- multiple scenarios
- held-out scenarios
- documented values

## 6. Latency targets

Initial engineering targets:

| Component | Target |
|---|---:|
| Local YOLO inference | ≤ 700 ms |
| Local decision logic | ≤ 100 ms |
| Urgent alert start | ≤ 1.5 s |
| STT after user stops | ≤ 2 s |
| Cloud query | ≤ 8 s |
| Digest response | ≤ 10 s after second frame |
| State update | < 50 ms |
| Dedup decision | < 50 ms |

These are engineering targets, not safety guarantees.

Actual measurements must be reported in experiments.
