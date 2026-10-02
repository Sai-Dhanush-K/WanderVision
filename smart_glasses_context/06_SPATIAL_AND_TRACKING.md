# Spatial Representation and Tracking

## 1. Why image-space reasoning

The MVP has no calibrated depth sensor.

Therefore the system uses:
- bounding boxes
- normalized coordinates
- bottom-center position
- image-space zones
- apparent size
- apparent size change

It must not claim exact physical distance.

## 2. Bottom-center

For a bounding box:

```text
x_center = (x1 + x2) / 2
y_bottom = y2
```

Use `(x_center, y_bottom)` as an approximate ground-contact point.

This is often more useful for path relevance than the bounding-box centroid.

## 3. Walking corridor

Use a normalized trapezoidal region representing the approximate forward walking path.

Conceptual image:

```text
+-------------------------+
|                         |
|       \         /       |
|        \       /        |
|         \     /         |
|          \   /          |
|           \ /           |
|           / \           |
|          /   \          |
+-------------------------+
```

The exact polygon should be a configuration parameter.

## 4. Zones

Initial zone labels:

```text
OUTSIDE
LEFT
FRONT_LEFT
FRONT_CENTER
FRONT_RIGHT
RIGHT
```

A point inside the central trapezoid is more relevant than a point outside it.

## 5. Apparent size

Possible normalized metric:

```text
bbox_area / image_area
```

Store:
- current apparent size
- previous apparent size
- size change rate

Classify:
- SMALL
- MEDIUM
- LARGE

Thresholds are configurable.

## 6. Approaching signal

A single frame is insufficient.

Require:
- same track
- multiple observations
- sufficient tracking duration
- consistent apparent-size growth

Avoid declaring approaching based on one noisy jump.

## 7. BoT-SORT

Use BoT-SORT through the supported tracking interface.

Reason:
- moving camera
- persistent track IDs
- camera-motion compensation is useful

Do not implement custom tracking unless necessary.

## 8. Tracking limitations

Tracking IDs are not permanent identities.

A track may:
- disappear
- be recreated
- switch under occlusion
- be lost in crowded scenes

The event system must tolerate this.

## 9. Track disappearance

Do not instantly resolve a hazard because one frame missed the object.

Use a configurable `TRACK_TTL`.

If the object remains unseen beyond TTL:
- mark track LOST
- reassess related hazards
- resolve if evidence no longer supports them

## 10. Moving camera limitation

The system detects apparent image-space movement.

This does not automatically mean the object itself is moving.

Camera movement can change bounding-box size.

Therefore:
- tracking is useful
- apparent expansion is only a proxy
- no exact trajectory claims in MVP

## 11. Path prediction

Do not build full pedestrian trajectory prediction in MVP.

The system is camera-centric and uses:
- spatial relevance
- apparent proximity
- apparent expansion
- persistence

as practical approximations.
