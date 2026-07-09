# HIS-Agent Evaluation Plan

This document describes the evaluation intended for the HIS-Agent EMNLP demo submission.

## Evaluation Goal

The core evaluation question is:

Can LLM-based semantic role correction improve doctor/patient attribution and downstream HIS task drafting when paired with Diart speaker diarization?

The system should be evaluated at two levels:

1. Component-level role attribution.
2. End-to-end voice-to-task quality.

## Data

Use simulated two-speaker outpatient conversations. Do not use real protected health information.

Recommended dataset size for the demo paper:

- 30 to 60 recorded dialogues.
- 1 to 3 minutes per dialogue.
- Two speakers per dialogue.
- Balanced doctor-first and patient-first openings.
- Scenarios covering patient lookup, department update, chief complaint update, visit type update, and no-action clinical discussion.

Each dialogue should have:

- Gold utterance boundaries or reviewed ASR final turns.
- Gold role label for each utterance: `Doctor` or `Patient`.
- Gold intended HIS task, if the dialogue implies one.
- Optional noise condition metadata.

## Systems to Compare

| System | Description |
| --- | --- |
| Default Mapping | `speaker_0 -> Doctor`, `speaker_1 -> Patient` without semantic correction |
| Latest Diart Segment | Attach the latest available Diart segment to each ASR turn |
| Time-Overlap Diart | Attach the Diart speaker with maximum time overlap |
| Steady-State Semantic Correction | Time-overlap Diart plus LLM correction after stricter sample thresholds |
| HIS-Agent | Time-overlap Diart plus first-round semantic correction and later steady-state correction |

## Metrics

### Role Attribution Accuracy

Utterance-level percentage of final turns whose role matches the gold role.

### Character-Weighted Role Accuracy

Role accuracy weighted by transcript length. This reduces the effect of many short filler turns.

### Correction Gain

Absolute and relative improvement over Diart-only role assignment.

### Wrong Override Rate

Percentage of turns where semantic correction changes an originally correct role into an incorrect role.

### Correction Latency

Number of final turns and seconds before the system first reaches the correct doctor/patient mapping.

### Task Draft Accuracy

Whether the final editable task captures the correct patient, target field, target value, and action type.

### Manual Edit Count

Number of manual corrections needed before the transcript/task can be accepted.

## Suggested Result Table

| Method | Role Acc. | Char-Wtd Role Acc. | Wrong Override | Latency Turns | Task Draft Acc. |
| --- | ---: | ---: | ---: | ---: | ---: |
| Default Mapping | TBD | TBD | TBD | TBD | TBD |
| Latest Diart Segment | TBD | TBD | TBD | TBD | TBD |
| Time-Overlap Diart | TBD | TBD | TBD | TBD | TBD |
| Steady-State Semantic Correction | TBD | TBD | TBD | TBD | TBD |
| HIS-Agent | TBD | TBD | TBD | TBD | TBD |

## Available Evaluation Hooks

The repository already includes voice and loop evaluation commands:

```bash
npm install
npm run loop:voice-role
npm run loop:voice-task-equivalence
npm run loop:perf
npm run test:e2e
```

These should be paired with a small curated voice-case set before final paper submission.

## Human Evaluation

For a lightweight demo-track user study, recruit 6 to 10 participants and ask them to complete 3 voice tasks each.

Compare:

- Diart-only transcript review.
- HIS-Agent with semantic role correction.

Measure:

- Time to acceptable transcript.
- Number of manual speaker/role edits.
- Number of task edits.
- User rating of role-label trustworthiness.
- User rating of task-draft usefulness.

## Reporting Guidance

Report both successes and failure modes. Important failure categories include:

- ASR turn boundary errors.
- Diart speaker switches on overlap speech.
- LLM role flips after a correct mapping.
- Ambiguous clinical dialogue with no clear page action.
- Downstream task drafting errors despite correct roles.
