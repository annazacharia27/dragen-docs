Read-Level Quality Control and Alignment Review
================================================

The DRAGEN DNA Pipeline provides a comprehensive quality control (QC) framework for both raw read data and aligned sequences. 
This section outlines the key tools and output metrics for ensuring input quality, read mappability, and alignment accuracy, 
which are essential for reliable variant calling and regulatory validation workflows.

FASTQ Quality Control
---------------------

Raw FASTQ data is evaluated using the DRAGEN FastQC module, which is automatically executed during any ``map-align`` run. 
It captures critical per-base and per-read quality metrics, modeled after the industry-standard FastQC tool.

Key outputs:

- **fastqc_metrics.csv** — Summarizes read-level QC statistics
- **trimmer_metrics.csv** — Logs read trimming outcomes

Common QC checks include:

- Mean read quality (Phred scores)
- Base-level quality distribution
- Base composition by position (A, C, G, T, N)
- GC content and GC bias
- Adapter/k-mer sequence presence
- Read length histograms
- Quantile-based Phred score distributions

Optional:  
For rapid QC without alignment, use ``--fastqc-only=true`` to run FastQC alone (~70% faster).

FASTQ Trimming and Filtering
----------------------------

DRAGEN supports hardware-accelerated read trimming during alignment. Both **hard trimming** (removes bases) and **soft trimming** (retains bases but ignores them during mapping) are supported.

Trimming types:

- **Fixed-Length Trimming** — Removes bases from 5' end
- **Poly-G Trimming** — Removes artifacts from two-channel chemistry (enabled by default)
- **Quality Trimming** — Trims 3' bases below a quality threshold (e.g., ``--trim-min-quality``)
- **Adapter Trimming** — Detects and removes adapter contamination
- **Ambiguous Base Trimming** — Removes trailing Ns
- **Min/Max Length Filtering** — Discards too-short reads or enforces uniform read length

Key trimming metrics (from `trimmer_metrics.csv`):

- Total reads and bases before/after trimming
- Average bases trimmed per read
- Filtered reads below length threshold
- Residual Poly-G tail detection
- Breakdown by trimming strategy (e.g., adapter, quality)

Recommended Input Quality Thresholds
------------------------------------

These thresholds serve as practical QC gates before proceeding to alignment and variant calling.

.. list-table:: Recommended Quality Thresholds
   :widths: 30 30
   :header-rows: 1

   * - Metric
     - Recommended Threshold
   * - % Bases ≥ Q30
     - ≥ 85%
   * - Mean Read Length
     - 100-150 bp
   * - Adapter Content
     - < 5%
   * - GC Content (human DNA)
     - 40-45%
   * - Duplication Rate
     - < 20% (preferred)
   * - Post-trim Min Read Length
     - ≥ 35 bp

BCL-to-FASTQ Conversion QC
--------------------------

When using Illumina BCL files as input, DRAGEN handles base calling and demultiplexing. Conversion QC includes:

- Sample demultiplexing by barcode (with mismatch tolerance)
- Adapter trimming and UMI tagging
- Lane merging and detection of index hopping

Key outputs (in `Reports/` directory):

- `Demultiplex_Stats.csv`
- `Adapter_Metrics.csv`
- `Top_Unknown_Barcodes.csv`

Alignment Quality Control
--------------------------

After alignment, DRAGEN produces multiple QC outputs that assess mapping accuracy and suitability for downstream analysis.

Main output: `mapping_metrics.csv`

Reported metrics include:

- Total input, mapped, and unmapped reads
- Properly paired read percentage
- PCR duplicate rate
- Indel counts and soft-clipping rates
- Q30 base percentages post-alignment
- MAPQ score distribution
- Contamination estimation (via optional known VCF file)

Coverage Evaluation
-------------------

DRAGEN outputs a set of files to evaluate genome coverage:

- `wgs_coverage_metrics.csv`
- `wgs_hist.csv`
- `wgs_overall_mean_cov.csv`
- `gc_metrics.csv`

Key coverage and bias metrics:

- Average genome-wide coverage
- Uniformity across genome or panel
- Percentage genome covered at ≥10x
- Coverage by chromosome (autosomes, chrX, chrY, Mitochondrial chromosome(MT))
- GC bias and dropout in high/low GC regions

These reports help assess whether sequencing was successful, mapping was accurate, and the data is suitable for confident variant calling.

