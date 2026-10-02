# Research Context and Literature Positioning

## 1. Problem area

Assistive wearable systems for visually impaired users combine sensing, interpretation, and feedback.

A central challenge is not merely detecting objects. The system must decide:
- what matters
- when it matters
- how urgently it should be communicated
- whether repeating information is useful

The project therefore focuses on information delivery and orchestration.

## 2. Relevant literature

### Kathiria et al. (2024)

"Assistive systems for visually impaired people: A survey on current requirements and advancements."

Neurocomputing 606, 128284.

ScienceDirect:
https://www.sciencedirect.com/science/article/abs/pii/S0925231224010555

Use for:
- broad assistive-system landscape
- object/text/scene recognition
- wearable systems
- requirements and advancements

### Zhang et al. (2024)

"Advancements in Smart Wearable Mobility Aids for Visual Impairments: A Bibliometric Narrative Review."

Sensors.

https://www.mdpi.com/1424-8220/24/24/7986

Use for:
- wearable mobility aids
- sensory substitution
- auditory/haptic feedback
- smart glasses
- cognitive load discussion
- multitasking
- usability and privacy considerations

Important:
Do not represent this paper as proving the proposed architecture is novel.

### Vocal-Eyes (2026)

"Vocal-Eyes: AI-Powered Smart Glasses for the Blind Using Transformer-Based Architecture and Scene Graph Generation."

Technologies.

https://www.mdpi.com/2227-7080/14/7/384

Use for:
- scene graph based descriptions
- multimodal smart glasses
- periodic spoken environmental awareness
- explicit concern about continuous feedback overload
- latency/continuous streaming limitations

The paper is especially relevant because it illustrates that continuously describing a scene is undesirable.

### Lupu et al. (2020)

"Cognitive and Affective Assessment of Navigation and Mobility Tasks for the Visually Impaired via Electroencephalography and Behavioral Signals."

Sensors.

https://www.mdpi.com/1424-8220/20/20/5821

Use for:
- cognitive load in assistive navigation
- comparison of auditory/haptic feedback
- evidence that assistive feedback can impose cognitive demands

Do not claim this proves the proposed system reduces cognitive load.

### Obstacle Detection Display for Visually Impaired (2017)

Frontiers in ICT.

https://www.frontiersin.org/journals/ict/articles/10.3389/fict.2017.00023/full

Use for:
- information overload
- balance between available information and human processing

### Comparing Tactile to Auditory Guidance for Blind Individuals (2019)

Frontiers in Human Neuroscience.

https://www.frontiersin.org/journals/human-neuroscience/articles/10.3389/fnhum.2019.00443/full

Use for:
- auditory versus tactile guidance
- environmental noise effects

### Hersh (2022)

"Wearable Travel Aids for Blind and Partially Sighted People: A Review with a Focus on Design Issues."

Sensors.

https://www.mdpi.com/1424-8220/22/14/5454

Use for:
- wearability
- usability
- comfort
- reliability
- battery
- cost
- user requirements
- social acceptance

Do NOT claim this paper specifically proposed the project's message-prioritization mechanism.

### ForeSightGuide (2026)

"ForeSightGuide: An Anticipatory Framework toward Accurate and Low-Redundancy Guidance for the Visually Impaired."

arXiv:
https://arxiv.org/abs/2608.18993

Use for:
- low-redundancy guidance
- predictive filtering
- redundant/false-positive alerts
- cognitive overload motivation

Important:
This means the project must not claim that low-redundancy prioritization itself is an untouched research problem.

### YOLOv8-based XR smart glasses work

MDPI Electronics:
https://www.mdpi.com/2079-9292/14/3/425

Use for:
- lightweight YOLOv8-based perception
- XR/smart-glasses context
- spatial/risk-oriented object handling

Do not assume its hardware performance is equal to this project's hardware.

## 3. Research positioning

The project should be framed as:

> A lightweight stateful edge-cloud orchestration and information-delivery prototype for assistive smart glasses.

Not:

> A new AI model for blind people.

Not:

> The first system to solve information overload.

Not:

> A system that eliminates cognitive load.

## 4. Proposed novelty/contribution

Potential contribution:
- common state representation for heterogeneous perception sources
- event lifecycle for persistent hazards
- local-first safety policy
- cloud semantic augmentation
- priority-aware adaptive speech
- redundant-alert suppression
- evaluation under cloud/resource constraints

## 5. Key distinction

The research question is about system-level behavior:

```text
What should the system communicate?
When should it communicate it?
How often should it repeat it?
What happens when cloud perception fails?
```

rather than:

```text
Can we detect a person?
```

## 6. Claims that require evidence

Do not claim:
- improved cognitive load without human study
- superior usability without user study
- safety guarantees
- universal performance
- exact distance
- exact trajectory prediction

The experiment should support only claims measured by the chosen protocol.
