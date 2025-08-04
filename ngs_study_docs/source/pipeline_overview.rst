Pipeline Setup and Input Requirements
=====================================

The DRAGEN DNA Pipeline is engineered to accelerate secondary analysis of Next-Generation Sequencing (NGS) data. 
Built on the DRAGEN Bio-IT Platform, it integrates FPGA-based acceleration with optimized software for 
read mapping, alignment, sorting, duplicate marking, and haplotype-based variant calling.

Compared to conventional tools (e.g., BWA-MEM + GATK-HC), DRAGEN processes a 30x whole human genome in under 30 minutes, 
with improved SNP and INDEL calling accuracy in validation studies.

In addition to small variant detection, the pipeline optionally supports:

- Copy Number Variant (CNV) calling
- Structural Variant (SV) detection
- Repeat expansion and ploidy estimation

Workflow Overview
-----------------

The DRAGEN DNA pipeline executes a streamlined sequence of high-speed modules:

1. **Read Mapping and Alignment**
   - Uses FPGA-accelerated BWA implementation
   - Supports ALT-aware and decoy-contig reference mapping

2. **Sorting and Duplicate Marking**
   - Coordinate sorting of aligned reads
   - PCR duplicate reads are marked or optionally removed

3. **Variant Calling**
   - Haplotype-based detection of SNVs and INDELs
   - Optional CNV and SV calling modules

4. **QC Metrics Generation**
   - Alignment and variant metrics are generated in `.csv`, `.vcf`, and `.gvcf` formats

Supported Applications
----------------------

DRAGEN supports a range of DNA sequencing use cases:

- Germline Whole Genome Sequencing (WGS)
- Whole Exome Sequencing (WES)
- Targeted gene panels
- Tumor/normal matched somatic variant analysis (licensed module)
- Repeat expansion detection (optional module)

Input Requirements
------------------

The following input formats are accepted:

- **File Types:** FASTQ, BAM, CRAM, Illumina BCL (converted via cBCL), GVCF
- **Compression Support:** Uncompressed, gzip/bgzip, ORA-compressed FASTQ
- **Read Formats:** Single-end and paired-end supported
- **Coverage Guidelines:**
  - WGS: Up to 300x
  - Tumor/Normal: Recommended 200x (tumor) / 100x (normal)

**Note:**  
If using BAM as input, DRAGEN ignores existing alignments and performs realignment. Paired-end BAMs must be sorted 
and contain correct read pairing for accurate processing.

Reference Genome Preparation
----------------------------

DRAGEN requires a prebuilt reference genome hash table, created using the `--build-hash-table` command.  
Illumina provides standard reference builds (e.g., hg19, GRCh38) with:

- ALT-aware liftover support (`--ht-alt-liftover`)
- Decoy contig integration to reduce false positives (`--ht-decoys`)
- Optimized indexing for fast seed lookups

Users may also generate custom references, adjusting parameters such as seed length and memory allocation.  
Once loaded, the reference stays resident in memory until system reboots, enabling faster re-runs.

File Compatibility Notes
------------------------

- **FASTQ files** must follow Illumina lane/sample naming conventions
- **BAM files** must be query-name sorted prior to alignment
- **Sample metadata** (e.g., `RGID`, `RGSM`) must be specified at run time

---

