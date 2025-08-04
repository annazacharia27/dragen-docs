Final Outputs and Reports
==========================

The DRAGEN DNA Pipeline produces a comprehensive set of outputs that include aligned reads, variant calls, and quality control metrics. These are stored in the output directory, typically prefixed using the ``--output-file-prefix`` option.

Core Output Files
-----------------

**Alignment Files:**

- **BAM / CRAM / SAM**: Default output is a sorted and compressed BAM file. CRAM or SAM can be selected via ``--output-format``.

**Variant Call Files:**

- **VCF**: Contains SNV, INDEL, CNV, and SV variant calls. Separate files are generated for somatic and germline calls.
- **gVCF**: Generated using ``--vc-emit-ref-confidence GVCF``. Includes both variant and non-variant sites for joint genotyping workflows.

QC Metrics Reports
------------------

**Mapping and Alignment Metrics (.mapping_metrics.csv):**

- Input, duplicate, and unmapped reads
- Q30 base counts, indels, soft-clipped reads
- MAPQ scores and optional contamination estimates (if reference VCF is supplied)

**Variant Calling Metrics (.vc_metrics.csv):**

- Total variants (SNPs, INDELs, MNPs), biallelic vs multiallelic sites
- Heterozygous to Homozygous (Het/Hom) and Transition to Transversion (Ti/Tv) ratios
- De novo variant metrics (DQ score)
- Callability metrics across genome and custom regions
- Mitochondrial and somatic-specific metrics (LOD, SQ scores)

**Runtime Metrics (.time_metrics.csv):**

- Step-by-step runtime summary across pipeline stages (alignment, sorting, variant calling)

Coverage and Callability Reports
--------------------------------

**Coverage Files:**

- `_coverage_metrics.csv`, `_overall_mean_cov.csv`, and `_contig_mean_cov.csv`: Summarize average depth, coverage uniformity, and per-chromosome metrics.

- BED output (e.g., `_full_res.bed`, `_cov_report.bed`): Provide region-specific depth statistics.

**Callability Files:**

- BED format output (e.g., `target_bed_callability.bed`) summarizing PASSing genotype calls in callable regions (configured with ``--qc-coverage-region-i``).

**GC Bias Report (.gc_metrics.csv):**

- GC content vs. normalized coverage distribution, enabled via ``--gc-metrics-enable``.

Specialized Caller Outputs
--------------------------

**Copy Number Variants (CNVs):**

- `.cnv.vcf.gz`, `.seg.called.merged` (segmentation file), `.cnv.igv_session.xml` for IGV visualization

**Structural Variants (SVs):**

- VCFs and stats in ``sv/results/`` and ``sv/results/stats/`` directories (germline, somatic, or tumor-only)

**Repeat Expansions:**

- `.repeats.vcf.gz` and graph BAM files; used in disorders like Huntington's or Spinal Muscular Atrophy (SMA)

**SMA (Spinal Muscular Atrophy):**

- SMA-related metrics embedded in repeat VCF with SMN-specific annotations

**CYP2D6 Typing:**

- Drug metabolism report in `.cyp2d6.tsv`

**Ploidy Estimation and ROH (Runs of Homozygosity):**

- `.ploidy_estimation_metrics.csv`, `.ploidy.vcf.gz`
- `.roh.bed`, `.roh_metrics.csv` for homozygosity analysis

**UMI Metrics:**

- `.umi_metrics.csv` summarizing UMI grouping, error correction, and consensus read generation

Summary
-------

The DRAGEN DNA Pipeline produces a well-structured output package suited for downstream interpretation, visualization, and regulatory review. These files support reproducible workflows across both research and clinical applications.
