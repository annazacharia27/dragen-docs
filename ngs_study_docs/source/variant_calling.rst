Variant Calling Quality Metrics
===============================

The DRAGEN DNA Pipeline outputs detailed variant-level quality control (QC) metrics in the 
`<prefix>.vc_metrics.csv` file. These help assess the accuracy, confidence, and distribution 
of SNVs, INDELs, and MNPs across the genome.

Metrics are reported for both **raw (pre-filtered)** and **filtered (post-filter)** variants.

General Variant Statistics
--------------------------

- **Samples analyzed**: Number of samples included in the VCF
- **Reads processed**: Input reads used for variant calling (duplicates/off-target reads excluded)
- **Total variants**: Combined count of SNPs, MNPs, and INDELs
- **Variant types**:

  - SNPs (Single Nucleotide Polymorphisms)
  - INDELs (Insertions and Deletions)
  - MNPs (Multi-nucleotide polymorphisms)
- **Biallelic vs. Multiallelic**:

  - Biallelic: One alternate allele
  - Multiallelic: ≥2 alternate alleles
- **Zygosity**:

  - Heterozygous vs. Homozygous variant counts
  - Het/Hom ratio: Indicates genotype balance
- **SNP Classification**:

  - Transitions (Ti): A↔G, C↔T
  - Transversions (Tv): Purine↔Pyrimidine
  - Ti/Tv Ratio: Higher ratio suggests higher variant calling specificity

De Novo Variant Metrics
-----------------------

DRAGEN identifies de novo variants not inherited from either parent based on a **De Novo Quality (DQ)** score.
The DQ score is a Phred-scaled confidence score indicating the probability that a variant is truly de novo. A higher DQ score signifies greater confidence.

- **VCF Annotation**: The ``INFO/DQ`` field provides the de novo status for each variant:
  
  - ``DeNovo``: High-confidence de novo variant (DQ above threshold)
  - ``LowDQ``: Lower-confidence de novo variant (DQ below threshold)

- **Customizable Thresholds**:

  - ``--qc-snp-denovo-quality-threshold`` for SNPs
  - ``--qc-indel-denovo-quality-threshold`` for INDELs

  These parameters allow users to control the minimum DQ score required for a variant to be flagged as de novo in the VCF.

Known vs. Novel Variants (dbSNP Comparison)
-------------------------------------------

If a reference database (e.g., dbSNP) is provided via ``--dbsnp``, DRAGEN compares each variant for known status.

- **Known variants**: Match entries in dbSNP
- **Novel variants**: Absent from dbSNP

Callability Metrics
-------------------

Callability measures the proportion of bases in the genome or target region that receive confident genotype calls.

- **Percent callability**: Fraction of non-N reference bases with a genotype call that passes quality filters.
- **Autosomal callability**: Callability calculated specifically for autosomal chromosomes.

**Custom region callability**:
DRAGEN allows up to three custom regions to be specified using BED files. This is useful for evaluating coverage and variant calling quality in targeted panels or exome regions.

- Provide BED files using: ``--qc-coverage-region-i`` (where ``i`` = 1, 2, or 3)
- Specify output file names using: ``--qc-coverage-reports-i``

Each custom region will generate a callability report that indicates the proportion of bases in that region with high-confidence calls.

- **Output**: `target_bed_callability.bed`


Chromosome Ratio Metrics
------------------------

These help evaluate sex karyotype, detect aneuploidies, and confirm sample identity.

- ChrX / ChrY SNP count ratio
- Average coverage for chrX, chrY, and mitochondria
- X/Y coverage ratio
- Mean/Median autosomal coverage ratio

Variant Quality Scores and Filters
----------------------------------

The DRAGEN DNA pipeline applies various quality scores and filters to ensure accurate identification of small variants, copy number variants (CNVs), and structural variants (SVs).

Small Variant Scores
^^^^^^^^^^^^^^^^^^^^

- **QUAL**: Confidence score (Phred-scaled) that a variant exists at the site.
- **GQ (Genotype Quality)**: Confidence in the accuracy of the genotype call.
- **QD (Quality by Depth)**: QUAL score normalized by sequencing depth.
- **DQ (De Novo Quality)**: Confidence score that a variant is de novo (not inherited).
- **SQ (Somatic Quality)**: Used in somatic workflows to assess somatic variant confidence.
- **LOD**: Used in mitochondrial variant calling instead of QUAL.

Small Variant Filters
^^^^^^^^^^^^^^^^^^^^^

- **Hard QUAL filters**: 
  - `DRAGENSnpHardQUAL`: SNPs with low QUAL.
  - `DRAGENIndelHardQUAL`: INDELs with low QUAL.
- **Other filters**: 
  - `LowDepth`, `PloidyConflict`, `lod_fstar`, `base_quality` (e.g., for mitochondrial or somatic variants).
- **De Novo filter (DN field)**: 
  - Annotates variants as `Inherited`, `LowDQ`, or `DeNovo` based on trio genotype and DQ threshold.
- **Somatic filters**: 
  - Applied in tumor-only or tumor-normal mode, e.g., `weak_evidence`, `panel_of_normals`, `low_af`.

CNV Quality Metrics
^^^^^^^^^^^^^^^^^^^

- **QUAL**: Overall quality score of the CNV call.
- **DQ**: Confidence for de novo CNVs.
- **CN**: Estimated copy number for the region.
- **Filters**: 
  - Examples include `cnvQual`, `cnvCopyRatio`, `SampleFT`, and `lowModelConfidence`.

SV Quality Metrics
^^^^^^^^^^^^^^^^^^

- **DQ**: De novo quality score for structural variants.
- **SOMATICSCORE**: Confidence score for somatic SVs.
- **Filters**: 
  - Examples include `IMPRECISE`, `MinQUAL`, `MaxDepth`, `NoPairSupport`, `MinSomaticScore`, etc.

General Notes
^^^^^^^^^^^^^
- DRAGEN also applies trimming, ALT-aware mapping, and duplicate marking to enhance overall variant quality.

These QC outputs help assess variant reliability, ensure appropriate filtering, and provide essential metrics for regulatory audits and SVP validation.
