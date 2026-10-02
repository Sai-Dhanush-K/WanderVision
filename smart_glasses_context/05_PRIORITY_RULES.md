# Priority and Risk Rules

## 1. Priority hierarchy

```text
P0 Emergency
P1 Urgent
P2 Important
P3 Informational
```

Higher number means lower urgency.

## 2. P0 definition

P0 requires strong local evidence of immediate physical danger.

Typical requirements:

```text
high-confidence local evidence
+
high-risk spatial relationship
+
immediate or rapidly increasing apparent proximity
```

Examples:
- vehicle rapidly entering walking corridor
- large obstacle immediately ahead
- moving object with strong collision relevance

Cloud-only evidence cannot create P0.

## 3. P1 definition

P1 means prompt attention is useful but evidence is not sufficient for P0.

Examples:
- path obstruction
- cyclist entering relevant path
- stationary object blocking corridor
- possible pothole
- possible stairs
- construction obstruction
- cloud-detected environmental hazard

## 4. P2 definition

P2 is useful contextual information without immediate danger.

Examples:
- relevant nearby person
- parked vehicle
- environmental feature
- nonurgent scene change

## 5. P3 definition

P3 is informational and normally silent.

Examples:
- distant objects
- ordinary background people
- irrelevant vehicles
- generic environmental objects

## 6. Risk dimensions

Risk assessment should consider:

### Object risk
What is the object or scene feature?

### Spatial relevance
Is it in the approximate walking corridor?

### Apparent proximity
How large is its image-space representation?

### Apparent expansion
Is its representation growing across tracked observations?

### Persistence
Is the evidence stable?

### Confidence
How reliable is the source?

### Context
Are multiple observations consistent?

## 7. Example rule

Conceptual:

```text
IF
    object_type == vehicle
    AND zone in {FRONT_CENTER, FRONT_LEFT, FRONT_RIGHT}
    AND confidence >= configured threshold
    AND approaching == true
THEN
    hazard_type = APPROACHING_VEHICLE
    priority = P0
```

This is an engineering rule, not a universal safety guarantee.

## 8. Stationary person rule

Do NOT automatically alert for every person.

Example:

```text
person
+ outside corridor
→ P3

person
+ corridor
+ stationary
+ not approaching
→ P2 or P1 depending on obstruction relevance

person
+ corridor
+ apparent expansion
+ relevant path
→ P1/P0 depending on thresholds
```

## 9. Traffic sign rule

Traffic signs are environmental information.

A sign must not automatically generate a P0/P1 alert merely because it exists.

A sign may become relevant through:
- user query
- cloud semantic interpretation
- navigation context added later

## 10. Unknown object rule

Unknown/low-confidence objects should not produce strong emergency alerts without additional evidence.

Possible behavior:
- retain as observation
- request cloud interpretation if configured
- avoid P0

## 11. Cloud priority cap

Cloud-only observations:

```text
maximum priority = P1
```

Rationale:
- cloud model may hallucinate
- network latency exists
- local safety path must remain authoritative for immediate hazards

## 12. Priority transitions

Possible:

```text
P3 → P2
P2 → P1
P1 → P0
```

and de-escalation:

```text
P0 → P1
P1 → P2
P2 → resolved
```

A priority escalation can trigger a new alert even inside cooldown.

## 13. No learned risk model in MVP

Use deterministic rules first.

Do not train a risk classifier unless the project later demonstrates a concrete need.
