# MATACS – AI Proposal Triage Design

**Candidate:** Asghar Ali  
**Reference:** MATACS-TH-2026-02  
**Time spent:** [Replace with your actual time spent before submitting the Google Form]

## Overview

This repository contains my design for an AI-assisted contractor proposal triage system. The proposed system extracts structured information from uploaded proposals, validates it against configurable evaluation criteria, calculates deterministic scores, and produces an explainable ranking for human review.

The system assists procurement reviewers; it does not make autonomous award decisions.

## Deliverables

- [WRITEUP.md](WRITEUP.md) — answers to the five assessment questions.
- [PROMPTS.md](PROMPTS.md) — prompts for proposal extraction and evidence verification.
- [ranking.json](ranking.json) — expected ranking of the five sample proposals.
- [diagram/process-flow.pdf](diagram/process-flow.pdf) — process flow from criteria setup and upload through human review.

## Design principles

- Use AI to extract information from unstructured documents.
- Use deterministic code for eligibility checks, scoring, and ranking.
- Keep evaluation criteria configurable and versioned.
- Store evidence alongside extracted values.
- Flag missing or ambiguous information instead of guessing.
- Keep a human responsible for the final award decision.
- Maintain an audit trail of assessments and reviewer changes.

## AI tool usage

I used ChatGPT to help explore the architecture, scoring methodology, edge cases, and draft the assessment deliverables. I reviewed and adjusted the suggestions rather than accepting them directly. For example, I corrected an initial scoring calculation after checking it against the stated formulas, and rejected the assumption that an electrical subcontractor's licence automatically satisfies the bidder-licensing requirement. I also treated the instruction embedded in Proposal C as untrusted document content, not as an instruction to the AI system.

## Repository structure

```text
matacs-th-2026-02/
├── README.md
├── WRITEUP.md
├── PROMPTS.md
├── ranking.json
└── diagram/
    └── process-flow.pdf
```
