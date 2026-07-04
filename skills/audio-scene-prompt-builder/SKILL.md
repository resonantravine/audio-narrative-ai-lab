## Purpose

Build generation-ready prompts for scene-level audio support in short-form audio narrative production.

This skill is designed for situations where the editor already knows the function of the audio layer — for example an intro bed, outro bed, transition air, ambience layer, or subtle music bed — but wants a stronger, more controllable prompt for a model such as **Doubao Audio Generation 1.0**.

It does not generate final audio by itself.
It transforms human scene intent into reusable prompt instructions.

---

## What this skill is for

Use this skill when the project needs bounded support audio such as:

- intro
- outro
- transition air
- scene ambience
- subtle tonal bed
- restrained background music
- short reflective transition music

This skill is especially useful in workflows where:

- the spoken structure is already stable,
- the editor wants support material rather than dominant music,
- the generation model can produce near-finished audio scenes from a single prompt,
- prompts still need human shaping to avoid over-generation.

---

## What this skill is not for

This skill is not for:

- writing full scripts
- replacing editorial structure
- generating full long-form stories
- deciding final mix levels
- guaranteeing release-ready output from one generation
- replacing human review

---

## Input

The human should provide as much of the following as possible:

- `scene`
- `function`
- `duration`
- `mood`
- `setting`
- `time_of_day`
- `location`
- `texture`
- `foreground_elements`
- `background_elements`
- `what_to_avoid`
- `language_presence`
- `music_role`
- `release_context`

### Minimal input example

```text
scene: Paris summer street near a café
function: intro
duration: 15 seconds
mood: light, warm, observational
what_to_avoid: clear dialogue, ad-like music, strong beat
```

---

## Output

The skill should return:

- 1 main prompt
- optionally 1–2 alternate prompts
- optional intensity variants such as:
  - safer
  - subtler
  - denser

Each prompt should be generation-ready for models like Doubao Audio Generation 1.0.

---

## Core design principles

### 1. Function first
The prompt must match the production function of the sound.
An intro is not the same as a scene bed.
A transition is not the same as a reflective ending.

### 2. Atmosphere before spectacle
In this repo's workflow, support audio usually helps spoken material rather than dominating it.
So the default should be restrained, breathable, and non-intrusive.

### 3. Scene realism matters
If the source project is grounded in real recordings, generated support audio should feel connected to a plausible place, time, and acoustic situation.

### 4. Avoid generic "cinematic" drift
The skill should actively suppress:

- ad music
- vlog music
- generic inspirational soundtrack energy
- over-sentimental piano
- over-large trailer scoring
- excessive beat-driven structure

### 5. Human intent remains primary
The prompt should reflect the editor's scene intention, not replace it.

---

## Prompt categories

### `intro_bed`
For opening a piece with mood and setting.
Usually short, light, scene-establishing.

### `outro_bed`
For release, fading out, or leaving a final atmosphere.
Usually more open and less detailed than intro.

### `scene_ambience`
For location-based environmental support.
May include indistinct human presence, air, movement, room tone, or urban texture.

### `transition_air`
For short bridges between ideas or sections.
Usually sparse, low-information, and non-melodic.

### `subtle_music_bed`
For lightly musical support beneath speech.
Should usually avoid obvious melody or attention-grabbing rhythm.

### `reflective_transition`
For moments where the piece moves from concrete speech into thought, reflection, or thematic expansion.

---

## Prompt-building logic

The skill should construct prompts in this order:

1. define the production function
2. define the scene / location
3. define the emotional tone
4. define sound elements that may appear
5. define what must remain indistinct or restrained
6. define what to avoid
7. define duration and ending behavior

### Prompt skeleton

A strong prompt usually contains:

- what to generate
- where it happens
- how it should feel
- what should be audible
- what should not be too obvious
- what should be avoided
- how it should end

---

## Example prompt template

```text
Generate a {duration} {function} audio layer for a short audio narrative.

Scene:
{scene_description}

Mood:
{mood_description}

Sound characteristics:
{sound_characteristics}

Include:
{include_list}

Keep indistinct or low in the mix:
{background_or_blurred_elements}

Avoid:
{avoid_list}

The result should feel {final_role_in_piece} and should end {ending_behavior}.
```

---

## Real usage in this project

This skill is grounded in real prompt work already used in this lab, especially for:

- Paris multilingual street ambience
- intro / outro beds
- subtle tonal support under spoken podcast pieces
- philosophical transitions from language to understanding
- black-humor city music sketches
- intimate, action-based emotional subtext
- tea shop / mahjong / café-adjacent scene atmospheres

Prompts are usually:

1. described by the human editor,
2. refined by GPT,
3. sent to Doubao Audio Generation 1.0,
4. evaluated and selected by the human,
5. adjusted in the DAW afterward.

---

## Doubao Audio Generation 1.0 relevance

This skill is particularly relevant for models like **Doubao Audio Generation 1.0**, because such models are moving beyond isolated line-level synthesis and toward scene-level generation.

That makes prompt quality much more important.
A single prompt may influence:

- dialogue feel
- ambience density
- non-verbal gestures
- music placement
- transition smoothness
- the sense of a near-finished scene

For that reason, the prompt must be specific enough to guide the scene, but restrained enough to avoid over-generation.

---

## Constraints

- Do not assume the model should generate full dialogue unless explicitly asked.
- Default to indistinct background voices in ambience scenes.
- Default to low-intrusion music beds when supporting spoken content.
- Avoid over-describing every sound; leave room for natural generation.
- Keep prompts production-oriented, not literary.
- Prefer clarity of function over decorative description.

---

## Typical outputs

### Output style A — main prompt only
Return a single polished prompt.

### Output style B — main + alternate
Return:
- one recommended prompt
- one lighter / safer variant
- one denser / more expressive variant

### Output style C — categorized set
Return a small grouped set such as:
- intro version
- transition version
- outro version

---

## Review questions

After generating audio from this skill, the editor should ask:

- Does the generated audio serve the spoken structure?
- Is it too musical, too dramatic, or too generic?
- Are background voices too clear?
- Does the scene feel grounded in the intended location?
- Does it help the piece breathe?
- Is the result better treated as a printed mix texture or should it be regenerated more narrowly?

---

## Working definition

`audio-scene-prompt-builder` is a reusable skill for turning human scene intent into controllable generation prompts for support audio layers in short-form audio narrative production.

It is one of the most shareable and immediately reusable skills in this repo because it already reflects repeated real production use.
