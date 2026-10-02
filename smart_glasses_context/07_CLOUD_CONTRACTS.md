# Cloud Multimodal Contracts

## 1. Purpose

The cloud multimodal model provides semantic scene understanding that is difficult or unavailable to the lightweight local detector.

It is an auxiliary perception source.

It is not the application's decision maker.

## 2. Cloud use cases

### A. Periodic digest

Every 30 seconds:
- capture frame A
- wait 15 seconds
- capture frame B
- send both
- request a concise environmental summary

### B. User query

User asks:
- What is ahead?
- Is anything blocking my path?
- Describe my surroundings.

Send:
- current frame
- relevant state
- user query

### C. Optional uncertainty escalation

Later, if desired:
- local system identifies a potentially relevant unknown object
- cloud is asked to interpret it

This should be disabled initially unless implementation is straightforward.

## 3. Request contract

Conceptual:

```json
{
  "request_id": "uuid",
  "request_type": "DIGEST",
  "timestamp": "ISO-8601",
  "mode": "NORMAL",
  "images": [
    {
      "id": "frame_a",
      "timestamp": "ISO-8601"
    },
    {
      "id": "frame_b",
      "timestamp": "ISO-8601"
    }
  ],
  "current_state": {
    "active_hazards": [],
    "tracked_entities": [],
    "recent_changes": []
  }
}
```

Actual image transport is handled by the model SDK/API adapter and should not be represented as giant strings inside internal domain objects.

## 4. User query request

```json
{
  "request_id": "uuid",
  "request_type": "USER_QUERY",
  "timestamp": "ISO-8601",
  "query": "What is blocking my path?",
  "current_state": {
    "active_hazards": [],
    "tracked_entities": []
  },
  "image": {
    "timestamp": "ISO-8601"
  }
}
```

## 5. Response contract

Cloud output should be structured.

```json
{
  "observations": [
    {
      "type": "possible_pothole",
      "severity": "high",
      "location": "front_right",
      "confidence": 0.78,
      "description": "Possible pothole in walking path"
    }
  ]
}
```

## 6. Response validation

Pipeline:

```text
Cloud response
     ↓
Parse JSON
     ↓
Schema validation
     ↓
Range validation
     ↓
Observation conversion
     ↓
StateStore
```

Reject if:
- invalid JSON
- missing required fields
- confidence outside `[0,1]`
- unknown invalid enum values
- excessive description length
- malformed location

## 7. Cloud prompt behavior

The model should be instructed to:
- describe visible evidence
- distinguish possible vs clear observations
- provide approximate image location
- provide confidence
- avoid inventing unseen objects
- return only the requested structured schema
- not issue direct safety commands
- not claim exact distances
- not claim certainty when uncertain

## 8. Cloud cannot bypass application rules

Bad:

```text
Cloud → "STOP!"
```

Correct:

```text
Cloud
→ observation
→ validation
→ state
→ hazard engine
→ priority engine
→ alert manager
→ TTS
```

## 9. Cloud priority restriction

Cloud-only evidence cannot produce P0.

Maximum:
```text
P1
```

## 10. Digest output

Digest should summarize relevant changes rather than narrate every object.

Potential result:
- "Possible pothole ahead on the right."
- "Construction barrier partly blocks the sidewalk."
- "No major path obstruction detected."

Avoid long descriptions.

## 11. Timeouts and retries

Cloud requests must have:
- timeout
- bounded retry
- exponential backoff
- maximum attempts

A 503 must not block local processing.

## 12. Image handling

Before sending:
- resize if needed
- use appropriate format
- avoid unnecessarily large payloads
- delete temporary files after request

Do not retain cloud frames permanently by default.

## 13. SDK boundary

Keep Google/Gemini-specific code inside a cloud adapter.

The rest of the application should depend on an interface such as:

```python
class VisionLanguageClient:
    async def analyze_digest(...): ...
    async def answer_query(...): ...
```

This keeps provider-specific details isolated.
