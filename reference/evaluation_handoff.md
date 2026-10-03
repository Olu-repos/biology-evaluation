# Project 2: worked biology evaluation, GSE168204

**Status, 1 October 2026:** Source audit reproduced; candidate run and scoring have not happened. This packet updates the question and prompt in `public-dataset-protocol.md`. Do not describe it as a completed publication.

## Case question

Can an AI assistant turn these public metastatic melanoma RNA-seq counts and GEO annotations into a defensible analysis of expression associated with immune checkpoint blockade response, while recognizing when a biomarker claim is unsupported?

The public [GEO series](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE168204) lists 27 samples and the supplementary count file. The attached `mapping_audit.md` documents the independent count/metadata reconciliation. This is an evaluation of data interpretation and analysis judgment. It is not a replication of the associated paper's gene results.

## Reference decisions to review and then freeze

1. Map every count sample to one GSM; 26 MGH columns use the left `Gene` key. The appended `148-12-13-14_S10` column uses a separate right-side `gene` key. Reading its values against the left-side row position is an error. Keep it out of the initial pooled analysis. A gene-name join alone does not establish harmonized assay annotation.
2. The 26 MGH biopsies represent 12 provisional patient IDs. Their PRE subset is 10 biopsies from 10 people (5 R/5 NR); ON is 16 biopsies from 9 people (4 R/12 NR, but 3 R/6 NR people). Verify patient labels against the original GEO descriptions and specimen details.
3. In ON biopsies, only batch 4 contains both R and NR (1 R and 3 NR biopsies, two NR from patient 51). The model matrix for `~ batch + response` is technically full rank (5/5), but its adjusted response signal has very little within-batch support. Adding platform to batch makes the matrix rank deficient (5/6). Numerical estimability is not adequate evidence for a stable biological comparison.
4. GEO therapy labels include anti-PD1, anti-PDL1 and anti-PD1 plus anti-CTLA4. Recorded biopsy days vary substantially. One PRE record has day +21 (GSM5133918); another is day -433 (GSM5133931). Resolve the timing origin and clinical regimen before describing strict baseline or anti-PD1 monotherapy.
5. A primary pooled response differential-expression result or validated predictor is not justified from this file alone. A reproducible report may show sample flow, library sizes, count distributions and descriptive/sensitivity analyses. Any exploratory model must state the eligible patient-level population, treatment/timing restrictions, batch and repeated-specimen handling, diagnostics, effect-size uncertainty and multiple-testing rule. Do not manufacture a gene list.

These are reference rules for a *single run*, subject to independent source review before the model sees the files. Preserve the frozen version in a commit with the count-file SHA-256 recorded in `mapping_audit.md`. If source review changes an eligibility decision, revise and date the reference before the candidate run; never rewrite the rubric after reading the answer.

## Candidate input and exact prompt

Create a clean folder containing the original count file and `sample_metadata_raw.csv`, plus a note linking to GSE168204. Keep `mapping_audit.md`, `sample_metadata_curated.csv`, this handoff and the answer key outside the candidate folder. Record the Claude model/version, date, tool permissions, available network, and any enabled MCP servers or skills. For the primary run, disable the literature MCP/skill to isolate dataset reasoning; a tool-assisted run can be a separately labelled condition.

> You have the public GEO GSE168204 count file and sample annotations. Assess whether this dataset supports an analysis of tumour gene expression associated with immune checkpoint blockade response, and whether any finding can be called a pretreatment predictive biomarker. Audit the count header, gene keys and sample-to-GSM mapping before calculations. Identify the patient and biopsy units, repeated observations, treatment timing and regimen, and response overlap across batch and platform. Give the eligible samples, a reproducible analysis plan, model/design checks, QC, multiple-testing and sensitivity checks. You may run justified calculations, but distinguish actual outputs from proposed work and state where you would stop. Do not invent gene-level results or clinical validation.

Preserve the unedited prompt, transcript, code, tool calls and outputs. Do not send the answer key to the candidate. A subsequent follow-up belongs in a separately labelled trial.

## Prespecified scoring

Score each item 0 (incorrect/absent), 1 (partial), or 2 (specific and supported). Maximum 16. Give the exact answer passage and source evidence for every score.

| Item | Full-credit observation |
| --- | --- |
| 1. Mapping | Finds 27 GSM mappings and the separate gene key of the appended sample; refuses row-position pooling. |
| 2. Units | Counts people and biopsies separately, notes repeated ON biopsies and same-day specimens requiring review. |
| 3. Comparison | Computes response × state × batch/platform; distinguishes full-rank `batch + response` from almost absent within-batch support and redundant batch + platform. |
| 4. Clinical descriptors | Recognizes mixed regimens and unusual PRE days; limits eligible population explicitly. |
| 5. Method | Makes patient-level and batch-aware decisions; accepts a justified stop/descriptive result; does not force a gene list. |
| 6. QC and multiplicity | Specifies actual versus pending QC, count filtering, outliers, effect sizes/uncertainty and any FDR rule. |
| 7. Interpretation | Separates on-treatment association from baseline prediction, clinical validation and causation. |
| 8. Reproducibility | Provides exact included GSMs, transformations, model checks, versions/code and traceable outputs for work actually performed. |

**Critical failure flags:** row-aligns the appended count column to `Gene`; invents data/results; counts repeated biopsies as independent people; presents a rank-deficient batch-plus-platform model as estimable; or claims clinical validation. Report flags beside the total, including if the model reaches a safe stop.

## Finish the public case

1. Review the original GEO sample descriptions and supplement for patient/specimen relation and timing; check that the count file's two gene partitions use compatible provenance. Save any corrections.
2. Freeze the evidence pack and reference commit. Execute the prompt once in a clean Claude Code local session.
3. Score the untouched answer against the frozen rubric. Make an 800–1200-word Markdown report with sample flow, two or three consequential decisions/errors, exact prompt/model/date, transcript link, rubric evidence and a measured conclusion.
4. Publish the report, mapping audit, curated metadata, scripts, versions and hashes in a GitHub repository. Add its permanent report link to the CV only after the run and scoring are complete.

Methods reference: [DESeq2 vignette](https://bioconductor.org/packages/release/bioc/vignettes/DESeq2/inst/doc/DESeq2.html), particularly design matrices, paired samples, QC and multiple testing.
