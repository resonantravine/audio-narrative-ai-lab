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
