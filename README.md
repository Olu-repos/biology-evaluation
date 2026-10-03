# Evaluating Claude Code on a public melanoma RNA-seq dataset

This case examines one Claude Code run asked to assess whether public [GEO GSE168204](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE168204) supports an analysis of immune checkpoint blockade response and a pretreatment biomarker claim. The answer was scored **12/16** against a reference rubric prepared before the run. This score describes one response to one prompt; no gene-level differential expression or clinical validation was performed.

Claude caught an important data problem: the count CSV places a 26-sample gene table beside a separately keyed sample. It also separated patients from repeated biopsies and rejected a clinical biomarker claim. The review found gaps in its specimen inference, batch/design reporting and multiple-testing argument. The original specimen and timing records do not yet establish a strict baseline cohort for every PRE label.

## Read the evaluation

1. [Worked case study](./worked-evaluation.md) — question, observed response, score and interpretation.
2. [Evidence-linked scorecard](./audit/claude-session1-scorecard.md) — eight rubric items and passages supporting each mark.
3. [Exact candidate prompt](./claude-code-stage/PROMPT.txt) and [path-redacted execution transcript](./claude-code-stage/claude-code-full-execution-transcript1-public.txt) — commands, failures, first answer and later repair.
4. [Source and provenance review](./audit/publication-readiness-2026-10-02.md) — source checks, reference chronology and unresolved specimen/timing questions.
5. [Reference handoff and rubric](./reference/evaluation_handoff.md), [mapping audit](./reference/mapping_audit.md) and [curated sample map](./reference/sample_metadata_curated.csv) — material held outside the candidate workspace.

The count matrix and publisher's Supplementary Data 1 are available from the [GEO series](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE168204) and the [published study](https://www.nature.com/articles/s41467-021-26299-4). The case study records their hashes where relevant. The candidate's later script repair is identified as follow-up work and does not alter the first-answer score.

## Status of the original handoff checklist

The files in `reference/` preserve the pre-run handoff and audit as written. In particular, “Finish the public case” in `evaluation_handoff.md` is a historical plan, not a statement that every action was completed. At the time this public package was prepared on 2 October 2026:

| Handoff step | What happened |
| --- | --- |
| 1. Review source and specimens | GEO, the paper and Supplementary Data 1 were reviewed. Same-day specimen identity, the origin of day zero and per-sample processing remain unresolved; the appended count partition was kept separate. |
| 2. Freeze and run | Claude Code ran once with the count file, raw metadata and prompt. The reference files have recorded creation times before the run, but no pre-run Git commit or signed snapshot of the exact Windows-side files was supplied. |
| 3. Score and write | The first answer was scored 12/16 and the worked evaluation was written. The later script repair is reported separately. |
| 4. Publish and link | This package includes the report, reference mapping, curated metadata, prompt and redacted transcript. The reviewed scripts and logs are not included here; the transcript shows the first-run commands and outputs. |

## Scope

This is an evaluation of an AI-assisted scientific analysis plan. The sample-level specimen relationship, precise timing origin and predictive biomarker validity remain unresolved. Any new analysis of gene expression or published signature scores should be reported as a separate, dated study.
