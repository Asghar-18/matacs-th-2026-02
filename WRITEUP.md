# AI-Assisted Proposal Triage Design

## 1. AI vs. Rules

I would use AI primarily to understand unstructured proposal documents. A PDF parser extracts text and page references, and an LLM extracts relevant information such as licences, insurance, project experience, pricing, timelines, warranties, and team details. The extraction output must follow a predefined JSON schema and include supporting evidence for each field.

I would use ordinary code for file validation, schema validation, mandatory pass/fail checks, score calculation, and ranking. These operations have predictable inputs and outputs, so using an LLM would introduce unnecessary variability. The LLM does not make the final eligibility or award decision; it extracts information for the rules engine to evaluate.

## 2. Consistency and Explainability

Each category has a versioned criteria configuration containing mandatory requirements, weighted criteria, and scoring parameters. The scoring engine uses deterministic formulas. Given the same validated proposal data and criteria version, it will always produce the same scores and ranking.

Each extracted value is stored with its supporting source text and page reference. Reviewers can inspect the original document, extracted value, evidence, applied scoring formula, and resulting points. The system also records the model identifier, prompt version, extraction output, and scoring configuration used for each assessment. Explanations are generated from the actual score breakdown; an LLM-generated summary is supplementary and cannot change the score.

## 3. Human in the Loop

Human review takes place after extraction and again after ranking. A reviewer can inspect extracted information, correct extraction errors, resolve uncertain mandatory requirements, and request clarification from bidders where necessary. Missing or conflicting information is flagged rather than silently treated as compliant.

A reviewer may override an assessment result, but must provide a reason. The original extraction and calculated score remain available for comparison. The audit log records uploads, document hashes, criteria versions, extracted values, reviewer corrections, score calculations, overrides, user IDs, timestamps, and the final award decision. The system provides a recommendation, not an automatic procurement decision.

## 4. Changing Criteria

Criteria are versioned and never modified in place. If an administrator changes a weight, mandatory requirement, or scoring formula, the system creates a new criteria version. Existing assessments remain associated with their original version.

An administrator or reviewer can explicitly request reassessment using the new version. All proposals in the comparison must be rescored against the same criteria version to ensure a fair comparison. Previous rankings remain available for audit purposes, and the criteria version is displayed with every result.

## 5. Risks and Safeguards

- **Incorrect extraction:** Store page-level evidence, validate output schemas, and require human verification for uncertain or important fields.
- **Hallucinated information:** Accept only values supported by document evidence. Missing information remains unknown.
- **Prompt injection:** Treat proposal contents as untrusted data. Instructions embedded in uploaded documents must never change system instructions, criteria, scoring rules, or ranking behaviour.
- **Bias and inconsistent scoring:** Apply the same versioned scoring formulas to every proposal and avoid subjective characteristics unrelated to configured criteria.
- **Document manipulation:** Preserve original files, calculate document hashes, and maintain an audit trail of corrections.
- **Missing information:** Flag incomplete mandatory requirements and request clarification rather than assuming compliance.
- **AI service failures and cost:** Use document-size and token limits, bounded retries, caching, and asynchronous processing. Reuse extraction results when the document and prompt version have not changed.

The objective is to reduce manual document-review effort while keeping eligibility, scoring, and final decisions transparent, reproducible, and under human control.

## Scoring assumptions used for the sample

The assessment specifies criterion weights but does not prescribe normalization formulas. For the sample ranking, I assume:

- Experience earns up to 30 points, with 10 qualifying projects (100 kW or more within five years) earning full marks.
- Price earns up to 25 points relative to the lowest-priced proposal that passes mandatory checks: `lowest eligible price / proposal price * 25`. D is included in the eligible-price baseline because its mandatory requirements are stated, even though its timeline is unknown and it is held for clarification.
- Timeline earns up to 15 points: `(16 - completion weeks) / 16 * 15`. A missing timeline is flagged, not assigned an assumed value.
- Warranty is split equally between panel warranty (25-year benchmark) and workmanship warranty (12-year benchmark), capped at 7.5 points each.
- Team and approach is split equally across a named project lead, crew, safety plan, and monitoring plan (3.75 points each).

These are explicit demonstration assumptions, not requirements stated by the assessment. In a production system, normalization methods and benchmarks would be configured by the administrator for each category. A proposal with missing required scoring information is held for review rather than silently receiving a fabricated score. Applying these assumptions gives A = 86.76, E = 80.62, and C = 73.80. B is ineligible because the bidder does not establish its own licence, and D is flagged because its completion timeline is unknown. The ranked order is therefore A, E, C; the figures are illustrative and depend on the stated assumptions.
