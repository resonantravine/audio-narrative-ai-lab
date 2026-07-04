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
