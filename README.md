# audio-narrative-ai-lab

**A production lab for blending field recordings with generative AI voice/sound models to craft audio narrative demos.**

---

## What this lab does

This is a practice-led repository. It starts from recorded reality — field recordings, mixed-language interviews, or workshop audio — and explores how those materials can become short audio narrative works through a combination of:

- listening and source interpretation
- narrative structuring
- transcription and translation
- AI voiceover / soundscape generation (via models like Doubao Audio Gen 1.0)
- human DAW editing and refinement

This repo is **not** a single monolithic agent. It documents **real demo production workflows** and extracts **reusable skills** from them — serving as both a production notebook and a portfolio archive.

Together, these branches explore two complementary questions:
how real recordings can become fictional sound stories,
and how real conversations can become edited non-fiction audio pieces.

---

## Demos

### 1. Fiction — A Black Box in the Park

A short soft sci-fi / fairy-tale audio piece developed from a real field recording made during a children's listening workshop in Beijing's Longtan Lake Park.

**Key source anchors**: breath, *xiū xiū* sounds, *nǐ hǎo*, *xiǎo bǎobèi*, *kuài shuìjiào* — treated as high-salience sonic and linguistic cues.

🎧 [A Black Box in the Park.mp3](https://raw.githubusercontent.com/resonantravine/audio-narrative-ai-lab/main/examples/bridge-black-box/A%20Black%20Box%20in%20the%20Park.mp3)

### 2. Non-fiction — Parisian Meets Mandarin

A production case study transforming a small mixed-language interview (Chinese / English / French) into a publishable short podcast.

**Process**: original interviewee voice + English interviewer VO + English translation track for French speech, aligned with human editing and AI-generated ambience / BGM.

📁 [Parisian Meets Mandarin.mp3](https://raw.githubusercontent.com/resonantravine/audio-narrative-ai-lab/main/non-fiction-case/Parisian%20Meets%20Mandarin.mp3)

---

## Workflows

### Fiction workflow
```
field recording → listening map → anchor extraction → story premise
→ act structure → generation prompts → multi-take audio generation
→ human review → final edit / mix
```

### Non-fiction workflow
```
interview recording → transcript → timestamped translation
→ structure extraction → human revision → track separation
→ AI voiceover generation (interviewer + translation)
→ three-track timing alignment → ambience/BGM prompt design
→ audio generation → final human refinement
```

---

## Reusable skills

This repo treats skills as reusable production units extracted from real demo work.

### Status definitions

| Status | Meaning | File convention |
|---|---|---|
| `mature` | Stable purpose, inputs, outputs, procedure, constraints, and review checklist. Used repeatedly in real workflows. | `SKILL.md` |
| `incubating` | Used in at least one real case, but procedure or output schema is not yet stable. | `STATUS.md` |
| `concept` | Promising direction, not yet tested enough to be used as a skill. | README mention only |

---

### Mature

#### `audio-transcribe-translation`
Transcribe source audio, segment speakers/languages, and produce timestamped translation.

📄 [`skills/audio-transcribe-translation/SKILL.md`](skills/audio-transcribe-translation/SKILL.md)

#### `audio-scene-prompt-builder`
Build generation-ready prompts for scene-level audio support such as intros, outros, ambience, transitions, and subtle BGM beds. Grounded in real prompt work for models such as Doubao Audio Generation 1.0.

📄 [`skills/audio-scene-prompt-builder/SKILL.md`](skills/audio-scene-prompt-builder/SKILL.md)

#### `podcast-piece-evaluation-checklist`
Final human review of a short interview-to-podcast piece across eight dimensions: clarity, density, source presence, voice balance, music/ambience, AI feel, ending quality, and release readiness.

📄 [`skills/podcast-piece-evaluation-checklist/SKILL.md`](skills/podcast-piece-evaluation-checklist/SKILL.md)

### Incubating

#### `three-track-podcast-editing-guide`
Guide the editorial relationship among original interviewee voice, translation track, and interviewer / narrator voiceover. The most original methodological contribution in this repo.

📄 [`skills/three-track-podcast-editing-guide/STATUS.md`](skills/three-track-podcast-editing-guide/STATUS.md)

#### `interview-podcast-structure-extractor`
Turn transcript text into a short podcast-ready structure: opening, question blocks, answer blocks, section order, and suggested cuts.

📄 [`skills/interview-podcast-structure-extractor/STATUS.md`](skills/interview-podcast-structure-extractor/STATUS.md)

### Concept

#### `privacy-sensitive-interview-podcast`
A lighter release-note / consent / exposure strategy layer for non-fiction work using real voices.

---

## Design principles

1. **Source first** — Work remains traceable to the original recording.
2. **Listen deeper** — Narrative cues aren't always the most textually obvious ones (breath, distance, naming, ambient texture).
3. **Bounded generation** — AI performs best when constrained by source anchors, duration, act structure, and continuity rules.
4. **Human in the loop** — AI is part of the workflow, not a replacement for editorial judgment.

---

## Background: Why generative audio models matter here

> *Optional deep-dive — skip if you just want to see the demos.*

This lab explores newer models (e.g., Doubao Audio Generation 1.0) that move beyond basic TTS toward:

- single-pass audio generation combining dialogue, SFX, and music
- stronger long-form voice consistency
- zero-shot generation from minimal prompts

In this lab, these models are treated as production tools rather than autonomous creators: they are most useful when paired with source material, structural decisions, and human editing. In practice, this shifts the editor's role from generating isolated lines to deciding *when* to rely on near-finished AI scenes, and *when* to break the process back into manual editing.

---

## Repository structure

```
.
├── README.md
├── docs/                    # workflow docs, case studies
├── skills/                  # reusable production skills
├── examples/                # fiction demos
├── non-fiction-case/        # interview-to-podcast case
└── tests/
```

---

## Current status

| Branch | Status |
|---|---|
| Fiction — A Black Box in the Park | ✅ Demo complete; narrow workflow / agent logic documented |
| Non-fiction — Parisian Meets Mandarin | ✅ Demo complete; case-study packaging in progress |
| Reusable skills | 📋 Partially extracted; further formalization in progress |
