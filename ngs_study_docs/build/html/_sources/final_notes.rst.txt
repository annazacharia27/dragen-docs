Notes and Best Practices
=========================

The DRAGEN DNA Pipeline is a high-performance, hardware-accelerated platform built to deliver fast, accurate, and reproducible secondary analysis of NGS data. While the workflow automates most steps, attention to input quality, runtime monitoring, and post-run QC is essential for reliable outcomes.

Pre-run Preparation
-------------------

- **Validate Input Quality**: Use FastQC or the DRAGEN FastQC module to assess FASTQ file integrity.
- **Reference Compatibility**: Ensure that the chosen reference genome matches the DRAGEN pipeline version and has a prebuilt hash table.
- **Storage Requirements**: Confirm sufficient disk space and proper read/write permissions on input/output directories.

In-Process Review
-----------------

- **Monitor Logs**: Check `dragen.log` during runtime for warnings or early signs of failure.
- **Track Pipeline Duration**: Use `time_metrics.csv` to evaluate step-by-step runtime and optimize future runs.

Post-run Review
---------------

- **QC Metrics**: Review mapping and variant metrics (`.mapping_metrics.csv`, `.vc_metrics.csv`) to verify alignment quality and variant confidence.
- **Coverage & Callability**: Assess genome coverage using `.coverage_metrics.csv` and callability BED files.
- **VCF Interpretation**: Evaluate variant filter flags and quality fields (e.g., QUAL, GQ, SQ) in the VCF for downstream prioritization.

Troubleshooting
---------------

- **Log Files**: Refer to `dragen.log` for a concise error summary.
- **Exit Codes and Time Stamps**: Help pinpoint failure points during execution.
- **Support**: For advanced use cases or known issues, refer to the [Illumina DRAGEN User Guide](https://support.illumina.com) and release notes for the specific software version.

---

This document supports  standard operating procedures (SOPs) and software validation plans (SVPs) for NGS bioinformatics workflows by providing clear guidelines for consistent analysis, pipeline validation, and traceability in DRAGEN-based sequencing.
