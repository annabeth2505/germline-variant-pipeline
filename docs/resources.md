# Resources 

## About this document
This pipeline implements a standard, published best-practices workflow using
established open-source tools. It does **not** introduce novel algorithms — its
contribution is the correct orchestration, parameterization, and independent
validation of community-standard tools. This document records the methodology it
follows, the tools and reference data it uses (with citations), and resources for
going deeper.

## Methodology
This pipeline follows the **GATK Best Practices** workflow for germline short
variant discovery (SNPs + indels):

    align (BWA-MEM) -> mark duplicates -> BQSR -> HaplotypeCaller -> benchmark (hap.py)

Each step and its parameters follow the Broad Institute's published best-practices
workflow: https://gatk.broadinstitute.org (Best Practices > Germline short variant
discovery).

## Tools used

| Tool | Role in this pipeline | Source | Citation |
|------|----------------------|--------|----------|
| BWA-MEM | Align reads to the reference | https://bio-bwa.sourceforge.net | Li H (2013), arXiv:1303.3997 |
| samtools / bcftools | Sort, index, QC, VCF handling | https://www.htslib.org | Danecek P et al. (2021), GigaScience 10(2) |
| GATK4 (MarkDuplicates, BaseRecalibrator, ApplyBQSR, HaplotypeCaller) | Clean BAM + call variants | https://gatk.broadinstitute.org | McKenna A et al. (2010), Genome Res; DePristo M et al. (2011), Nat Genet; Poplin R et al. (2018), bioRxiv |
| hap.py | Benchmark calls vs. truth set | https://github.com/Illumina/hap.py | Krusche P et al. (2019), Nat Biotechnol 37 (GA4GH benchmarking) |

## Reference & resource data

| Data | Purpose | Source |
|------|---------|--------|
| GRCh38 no-alt analysis set | Reference genome | https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/000/001/405/GCA_000001405.15_GRCh38/seqs_for_alignment_pipelines.ucsc_ids/ |
| GIAB HG002 v4.2.1 benchmark (VCF + high-confidence BED) | Truth set for benchmarking | https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/release/AshkenazimTrio/HG002_NA24385_son/NISTv4.2.1/GRCh38/ |
| GATK resource bundle: dbSNP138, Mills, known_indels | Known sites for BQSR | https://storage.googleapis.com/gcp-public-data--broad-references/hg38/v0/ |

GIAB benchmark reference: Zook JM et al. (2019), Nat Biotechnol 37.

## Learning resources (ordered)
1. **GATK Best Practices — Germline short variant discovery** (read first, in full):
   the canonical workflow this pipeline implements. https://gatk.broadinstitute.org
2. **Tool docs** (reference, as needed): BWA manual (bio-bwa.sourceforge.net/bwa.shtml);
   samtools (htslib.org/doc/samtools.html).
3. **Benchmarking**: hap.py README (github.com/Illumina/hap.py); Genome in a Bottle
   project (nist.gov, GIAB GitHub).
4. **nf-core/sarek** (https://nf-co.re/sarek): a production Nextflow pipeline doing
   this same workflow — read its module code to see it written as a real pipeline.
5. **nf-core training** (https://training.nextflow.io): learn Nextflow properly by
   building pipelines.

## Provenance statement
The commands in this repository follow the standard GATK Best Practices germline
workflow, using the open-source tools and public reference data cited above. The
pipeline's correctness is demonstrated empirically by benchmarking its output against
the independent GIAB HG002 truth set (see results/), rather than asserted.
