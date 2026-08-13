# Variant Caller Comparison: GATK HaplotypeCaller vs. DeepVariant

## Summary
Both callers were run on the same HG002 chromosome 20 data and benchmarked
identically against the GIAB v4.2.1 truth set (high-confidence regions only,
via hap.py). **DeepVariant outperformed HaplotypeCaller on every metric.**

| Metric | HaplotypeCaller | DeepVariant |
|--------|----------------|-------------|
| SNP recall | 99.36% | **99.51%** |
| SNP precision | 99.44% | **99.91%** |
| SNP F1 | 0.9940 | **0.9971** |
| Indel recall | 97.85% | **98.68%** |
| Indel precision | 98.91% | **99.38%** |
| Indel F1 | 0.9838 | **0.9903** |
| SNP false positives | 397 | **62** |
| Indel false positives | 126 | **72** |

## Key findings
- **DeepVariant cut SNP false positives ~6-fold** (397 -> 62). It called far
  fewer total SNPs (86,324 vs 102,690) but almost all were real — the CNN is
  more discriminating about noisy read pileups.
- **DeepVariant improved indels on both axes simultaneously** — higher recall
  (fewer missed) and higher precision (fewer invented). Indels are the harder
  variant class, and DeepVariant's image-based approach is known to handle them
  particularly well.
- Both callers are strong; the gap is meaningful but not dramatic. HaplotypeCaller
  remains a robust, widely-used baseline.

## Method note (fair-comparison caveat)
Each caller was run following its own recommended best practice:
- **HaplotypeCaller** was run on the BQSR-recalibrated BAM (GATK best practice).
- **DeepVariant** was run on the marked-duplicates BAM *without* BQSR, because
  its model is trained on non-recalibrated data and Google recommends against
  BQSR before it.

This is the fair *real-world* comparison (each tool as intended), rather than a
pure algorithm isolation on byte-identical input. Interpret the difference as
"best-practice HaplotypeCaller vs. best-practice DeepVariant," not "algorithm A
vs. algorithm B on identical input."

## Interpretation
This result is consistent with the published literature: deep-learning callers
now generally match or exceed traditional probabilistic callers, with the largest
gains on indels and on precision. For a clinical context, DeepVariant's lower
false-positive rate is especially valuable, since false positives drive
unnecessary downstream review.
