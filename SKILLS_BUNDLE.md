# audio-narrative-ai-lab — Skills Bundle

> For review. Status definitions and all skill specifications.
> Generated: 2026-07-04

---

## Status definitions

| Status | Meaning | File convention |
|---|---|---|
| `mature` | Stable purpose, inputs, outputs, procedure, constraints, and review checklist. Used repeatedly in real workflows. | `SKILL.md` |
| `incubating` | Used in at least one real case, but procedure or output schema is not yet stable. | `STATUS.md` |
| `concept` | Promising direction, not yet tested enough to be used as a skill. | README only |

---

## Mature skills

---

## audio-transcribe-translation
## Purpose

Transcribe source audio, segment speakers and languages, and produce timestamped translations for use in audio narrative production workflows.

This is the first skill invoked in the non-fiction podcast branch. It takes raw interview or field recording audio and produces structured, editable text that downstream skills — structure extraction, voice alignment, prompt building — can consume.

---

## What this skill is for

Use this skill when the input is a real audio recording containing one or more speakers, potentially in multiple languages, and the goal is to produce:

- a full transcript with speaker labels
- language identification per segment
- timestamped translation into a target language

This skill is especially useful when:

- the source recording is multilingual,
- the recording will later be cut and structured for a podcast piece,
- the translation needs to stay close to the original rhythm and speaking pace,
- downstream editorial work depends on accurate timing anchors.

---

## What this skill is not for

This skill is not:

- a live transcription service,
- a subtitle / caption formatting tool,
- a final translation for publication (translation output is a working draft for human revision),
- a speaker diarization system at the level of forensic voice identification.

---

## Input

- audio file path or local reference
- optional known languages in the recording
- optional speaker count hint
- optional target translation language (default: English)
- optional segment granularity preference

### Minimal input example

```text
- audio: interview.wav
- known languages: Chinese, English, French
- target language: English
```

---

## Output

The skill should return:

### 1. Transcript
A segment-by-segment transcript with:
- start time (seconds)
- end time (seconds)
- speaker label (or `unknown` if uncertain)
- detected language
- original text in source language
- English translation (working draft)

### 2. Summary metadata
- total duration
- detected language distribution
- speaker count (estimated)
- notable segments flagged for editorial attention (e.g. emotionally dense, topic shifts, strong source presence)

### Example output format

```json
{
  "segments": [
    {
      "start": 0.0,
      "end": 4.2,
      "speaker": "A",
      "language": "zh",
      "original": "我觉得学语言这件事...",
      "translation": "I think learning a language...",
      "flags": []
    }
  ],
  "meta": {
    "duration": 563.0,
    "languages": {"zh": 0.45, "en": 0.30, "fr": 0.25},
    "speaker_count": 2
  }
}
```

---

## Transcription approach

This skill recommends using local, offline-capable ASR models:

| Model | Strength |
|---|---|
| `faster-whisper` + `large-v3` | High accuracy, multilingual, runs on Apple Silicon |
| `mlx-whisper` | Optimized for Apple Neural Engine, lower memory |

### Language handling
- When source languages are known in advance, pass them as a prompt hint.
- For mixed-language segments, prefer the dominant language per segment.
- Mark uncertain classifications explicitly.

---

## Translation principles

1. **Working draft, not final** — The translation is for editorial use, not publication. Human revision is expected.
2. **Stay close to rhythm** — The translation should respect the original segment boundaries and speaking pace so that downstream alignment work is possible.
3. **Preserve source texture** — Hesitations, repetitions, and incomplete sentences should be reflected in the translation where they carry editorial value.
4. **Flag what's uncertain** — If a segment is hard to hear, mark it rather than guessing.

---

## Real usage in this project

In this lab, this skill is used as the entry point for the non-fiction podcast workflow:

```text
interview recording
→ audio-transcribe-translation
→ structure extraction
→ three-track editorial decisions
→ voiceover / translation audio generation
→ scene prompt building
→ final evaluation
```

The raw transcript and translation serve as the editorial base for all downstream creative decisions.

---

## Constraints

- Do not auto-correct or "clean up" the source transcript before preserving the original.
- Do not remove hesitations, fillers, or incomplete utterances without flagging them.
- Do not produce a finalized, publishable translation — this is a working tool, not a final deliverable.
- Keep the output structured for machine consumption (JSON-first, render for humans second).

---

## Review questions

After running transcription and translation, the editor should ask:

- Are speaker labels consistent throughout?
- Are language boundaries correctly identified?
- Do the timestamps feel accurate enough for downstream alignment?
- Are translation drafts close enough to the original rhythm for editing?
- Are there flagged segments that need re-listening?
- Is the output structured cleanly enough for the next skill in the chain?

---

## Working definition

`audio-transcribe-translation` is the first-stage input skill in the non-fiction podcast workflow. It converts raw multilingual audio into structured, translated text that enables all subsequent editorial and generative work.

It is foundational and immediately shareable because transcription is a universal need across all branches of this lab.

---

## audio-scene-prompt-builder
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

---

## podcast-piece-evaluation-checklist
# podcast-piece-evaluation-checklist

> Status: **mature** — stable procedure, eight review dimensions, used in real production.

## Purpose

Final human review of a short interview-to-podcast piece before release. Produces a structured checklist and a release readiness decision.

## Use when

- A rough cut, fine cut, or near-final mix exists.
- The piece combines original interview voice, translated speech, narrator/voiceover, and optional ambience/BGM.
- You need to decide: is this ready to share?

## Do not use for

- Automatic quality scoring.
- Content moderation or privacy auditing.
- Replacing actual listening and human editorial judgment.

## Inputs

- Audio (rough mix or near-final mix)
- Section structure or outline
- Optional: transcript, translation text, voiceover script, privacy notes, target audience

## Outputs

A structured checklist + one release decision.

### Release decision (pick one)
- `ready_with_minor_fixes`
- `needs_structural_revision`
- `needs_mix_revision`
- `not_ready_for_release`

---

## Procedure

Evaluate the piece across eight dimensions. For each, check pass/fail and note any fix priority (high / medium / low).

### 1. Clarity

Can a listener follow the piece without reading a transcript?

| Check | Warning signs |
|---|---|
| Opening establishes the situation quickly | Too many ideas introduced too early |
| Section boundaries are legible | Voiceover says too much before source voice is heard |
| Voiceover helps clarity, doesn't over-explain | Translation enters too late to keep pace |

### 2. Density

Does the piece have enough breathing space?

| Check | Warning signs |
|---|---|
| Gaps exist between key speech layers | Every gap is filled |
| Sections don't feel packed | All three voice layers constantly active |
| Pacing feels breathable | No rest between original voice and translation |

### 3. Source presence

Does the original interview still matter as an encounter?

| Check | Warning signs |
|---|---|
| Original voice carries reality and trace | Reduced to token decoration only |
| Piece feels grounded in a real encounter | Voiceover dominates entirely |
| Source rhythm preserved | Final edit sounds like reconstructed text |

### 4. Voice balance

Does each section have a clear lead layer?

| Check | Warning signs |
|---|---|
| One layer leads per section | All three carry equal weight at once |
| Translation doesn't overwhelm | Translation duplicates everything literally |
| Voiceover doesn't over-explain | Listener can't tell which layer is primary |

### 5. Music / ambience

Does the bed support rather than compete?

| Check | Warning signs |
|---|---|
| Bed stays under translated speech | Bed too loud under key lines |
| Intro/outro not over-produced | Music imposes emotion the voices don't need |
| Transitions don't distract | Transition sounds more attention-grabbing than content |

### 6. AI feel

Does the piece still feel shaped by human editing?

| Check | Warning signs |
|---|---|
| Voiceover rhythm is natural | Too regular, too synthetic |
| Translated speech has human pacing | Mechanically timed |
| Ambience doesn't sound generic | Preset-like, flattened texture |

### 7. Ending quality

Does the ending feel earned?

| Check | Warning signs |
|---|---|
| Returns to central mood or question | Abrupt stop |
| Leaves enough air | Over-explains the meaning |
| Relates to earlier motifs | Music tells the audience what to feel |

### 8. Release readiness

Is this actually ready?

| Check | Warning signs |
|---|---|
| Coherent enough for public listening | Unresolved privacy uncertainty |
| Consent situation is clear | Confusing structure |
| Remaining problems are minor | Temporary placeholders still present |

---

## Fix priority assignment

After evaluating all eight dimensions, classify each issue:

| Priority | Definition | Examples |
|---|---|---|
| **High** | Blocks release | Core meaning unclear, translation/source balance broken, privacy unresolved, section order broken |
| **Medium** | Reduces quality, doesn't block understanding | BGM slightly too present, VO pacing too flat, one section too dense |
| **Low** | Optional polish | Intro can be softer, one overlap can be shorter, ending bed can fade more gently |

---

## Constraints

- Do not reduce to a single numeric score.
- Do not claim "publishable" without noting remaining risks.
- Technical polish is not more important than source presence or structure.
- Prefer actionable notes over vague praise.

---

## Relation to other skills

Used after: `interview-podcast-structure-extractor`, `three-track-podcast-editing-guide`, `audio-scene-prompt-builder`.

```text
structure extraction → three-track editing → ambience/BGM → rough mix → evaluation-checklist → final revision
```

---

## Working definition

`podcast-piece-evaluation-checklist` is a reusable review skill for determining whether a short AI-assisted interview-to-podcast piece is clear, balanced, grounded, and ready enough to share. Its purpose is to support final human judgment with a structured editorial checklist.

---

# Incubating skills

## interview-podcast-structure-extractor
# interview-podcast-structure-extractor

> ⚠️ **Emerging skill** — Still being extracted from real production workflows. The version below describes the intended capability and current design direction, not a finished, packaged skill.

## Intended purpose

Turn transcript text into a short podcast-ready structure: opening, question blocks, answer blocks, section order, and suggested cuts.

This skill will sit between `audio-transcribe-translation` and `three-track-podcast-editing-guide` in the non-fiction workflow.

## Current status

The structural extraction logic has been used in practice for the Parisian Meets Mandarin case study, but it is not yet formalized into a standalone, reusable skill file. The core decision-making patterns are understood; the spec is still being written.

## Design direction

- Input: full transcript + translation from `audio-transcribe-translation`
- Output: section breakdown with lead voice suggestions, cut points, and structural narrative arc
- Works closely with `three-track-podcast-editing-guide` for voice-layer decisions

## See also

- `three-track-podcast-editing-guide` (emerging)
- `podcast-piece-evaluation-checklist` (emerging)

## three-track-podcast-editing-guide
# three-track-podcast-editing-guide

> ⚠️ **Emerging skill** — The full editorial decision framework has been drafted and tested against real production material, but the skill is not yet packaged as a reusable, self-contained guide. The version below is a working stub.

## Intended purpose

Guide the editorial balance among three main voice layers in a short interview-to-podcast workflow:

- original interviewee voice
- translated interviewee voice track
- interviewer or narrator voiceover

## Current status

The three-layer decision logic (semantic, rhythmic, music-interaction) has been applied during the Parisian Meets Mandarin case study. Four editorial patterns (A–D) and a section-by-section guide format are drafted but not yet finalized as a standalone skill specification.

## Core design

At any moment, one voice layer should lead. The skill helps answer: which one, when, and why — across three dimensions: semantics, rhythm, and music interaction.

## See also

- `interview-podcast-structure-extractor` (emerging)
- `podcast-piece-evaluation-checklist` (emerging)
- `audio-scene-prompt-builder` (mature)
