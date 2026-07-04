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
