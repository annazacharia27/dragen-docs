Variant Calling Quality Metrics
===============================

The DRAGEN DNA Pipeline outputs detailed variant-level quality control (QC) metrics in the 
``<prefix>.vc_metrics.csv`` file. These help assess the accuracy, confidence, and distribution 
of SNVs, INDELs, and MNPs across the genome.

Metrics are reported for both **raw (prefiltered)** and **filtered (postfilter)** variants.

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

- **VCF field**: ``INFO/DQ`` indicates ``DeNovo`` or ``LowDQ``
- **Score thresholds** (adjustable):
  - ``--qc-snp-denovo-quality-threshold``
  - ``--qc-indel-denovo-quality-threshold``

Known vs. Novel Variants (dbSNP Comparison)
-------------------------------------------

If a reference database (e.g., dbSNP) is provided via ``--dbsnp``, DRAGEN compares each variant for known status.

- **Known variants**: Match entries in dbSNP
- **Novel variants**: Absent from dbSNP

Callability Metrics
-------------------

Callability measures the proportion of bases with confident genotype calls.

- **Percent callability**: Fraction of non-N reference bases with PASS genotype
- **Autosomal callability**: Callability restricted to autosomes
- **Custom region callability**: Up to 3 BED files can be supplied with:
  - ``--qc-coverage-region-i``
  - ``--qc-coverage-reports-i``

Output file: ``target_bed_callability.bed``

Chromosome Ratio Metrics
------------------------

These help evaluate sex karyotype, detect aneuploidies, and confirm sample identity.

- ChrX / ChrY SNP count ratio
- Average coverage for chrX, chrY, and mitochondria
- X/Y coverage ratio
- Mean/Median autosomal coverage ratio

Variant Quality Scores and Filters
----------------------------------

DRAGEN uses optimized confidence scoring to reduce false positives.

- **QUAL**: Confidence in variant existence
- **GQ (Genotype Quality)**: Confidence in genotype call
- **QD (Quality by Depth)**: QUAL normalized by read depth
- **LOD (Log Odds)**: Used for mitochondrial calls

Mitochondrial-specific thresholds:

- ``--vc-lod-call-threshold``
- ``--vc-lod-filter-threshold``

Somatic-specific scoring (if tumor/normal pipeline enabled):

- **SQ (Somatic Quality)**: Replaces QUAL/GQ
- Thresholds:
  - ``--vc-sq-call-threshold``
  - ``--vc-sq-filter-threshold``

These QC outputs help assess variant reliability, ensure appropriate filtering, and provide essential metrics for regulatory audits and SVP validation.
