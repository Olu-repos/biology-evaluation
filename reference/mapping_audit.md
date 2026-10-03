# GSE168204 count/metadata audit

## Provenance and integrity

- Count file: `GSE168204_MGH_counts.csv.gz`; SHA-256 `4e3ec53c42b7c487a746201eef1cb546d61bede8faedaf9137e3d4ae39cd38ff`.
- Metadata input: `sample_metadata_raw.csv`; SHA-256 `9101d887b8d0cdda138c37630b4a093eac0ee22dea12c77ce11ade0e1d1bd1e7`.
- Source URLs: https://ftp.ncbi.nlm.nih.gov/geo/series/GSE168nnn/GSE168204/suppl/GSE168204_MGH_counts.csv.gz and https://ftp.ncbi.nlm.nih.gov/geo/series/GSE168nnn/GSE168204/soft/GSE168204_family.soft.gz.
- GEO records: 27. Count columns: 27 biological samples plus `Gene`, an empty separator and `gene`. After removing `_bam` and punctuation in names, all 27 columns map uniquely to all 27 GSM records. See `sample_metadata_curated.csv` for every mapping; this matching rule does not prove specimen identity independently.
- Left matrix: 75,253 distinct gene keys × 26 MGH samples; all entries nonnegative integer strings.
- Right matrix: 75,209 distinct gene keys × one `148-12-13-14_S10` sample; 44 trailing rows are empty on the right. The two gene keys match by row in only 562 of 75,209 populated right-side rows. Their overlap by name is 75,038; 215 left-only and 171 right-only. Never treat the appended values as if they shared the left-side row key.

## Biological units and comparisons

- MGH block: 26 biopsies from 12 provisional patient IDs, with PRE 10 biopsies (10 patients; R 5, NR 5) and ON 16 biopsies (9 patients; R 4, NR 12).
- ON group by batch and response: {('1', 'R'): 3, ('2', 'NR'): 6, ('3', 'NR'): 3, ('4', 'NR'): 3, ('4', 'R'): 1}. Batch 4 is the only ON batch containing both outcomes (1 R, 3 NR biopsies). Batch 1 has only R; batches 2 and 3 only NR.
- ON response by provisional patient: 3 R and 6 NR. Patient 530 contributes two ON R biopsies; IPIPD1001 contributes six ON NR biopsies; patient 51 contributes two ON NR biopsies. Same-day GSM records for IPIPD1001 require specimen/technical-replicate review.
- Batch and platform are nested: batches 1–3 use GPL18573, batch 4 uses GPL11154 in the MGH block. For ON biopsies, a model with intercept + response + batch has matrix rank 5/5, but adding platform has rank 5/6. Full rank for response + batch does **not** establish useful overlap: the adjusted response comparison is anchored in one batch-4 R specimen against three NR specimens, including two from one patient.
- Therapy labels include anti-PD1, anti-PDL1 and anti-PD1+anti-CTLA4. Do not describe this entire set as anti-PD1 monotherapy.
- GSM5133918 is labelled PRE but has recorded day +21; GSM5133931 is PRE at day -433. Check the study's timing definition before declaring either a strict baseline. Site and tumour composition are not in the supplied sample table.

## Provisional decision

Use the 26-sample MGH partition for the worked **audit of an AI biological analysis**. The expected safe conclusion is a transparent exploratory description with strong limits; do not demand a gene biomarker or clinical prediction claim. Exclude the appended sample from a joined count model pending a verified gene-key join and compatible assay annotation. Freeze eligibility and reference rules after independent review of specimen identity, treatment timing and protocols. This audit itself has not fitted a differential-expression model or run gene-level QC.
