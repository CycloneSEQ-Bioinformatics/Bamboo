## Project Status: Sunset Notice

> [!IMPORTANT]
> **This repository (Bamboo) is no longer under active development or maintenance.**

For the latest features, improved performance, and a more user-friendly experience in long-read quality control and error profiling, we strongly recommend all users transition to our new tool: **Rosa**.

### Why Switch to Rosa?

* **Expanded Functionality:** Provides more comprehensive QC metrics and detailed descriptions to assist with result interpretation.
* **Enhanced Usability:** Features a streamlined installation process and numerous bug fixes for a smoother experience.
* **Active Support:** Our team is highly responsive; please reach out by opening a GitHub issue.

**Get started here: [https://github.com/CycloneSEQ-Bioinformatics/Rosa](https://github.com/CycloneSEQ-Bioinformatics/Rosa)**



![Bamboo logo](bamboo-logo.png)



# Bamboo

a tool for quality control and error profiling of long-read sequencing data.


## Installation

```sh
# Install samtools and Minimap2
conda create -n bamboo_env samtools minimap2

# Check the latest version of Bamboo
BAMBOO_VERSION=$(curl -s https://api.github.com/repos/CycloneSEQ-Bioinformatics/Bamboo/releases/latest | grep "tag_name" | awk -F'"' '{print $4}' | sed 's/^v//')
echo $BAMBOO_VERSION 

# Download Bamboo executable
wget https://github.com/CycloneSEQ-Bioinformatics/Bamboo/releases/download/v$BAMBOO_VERSION/bamboo-$BAMBOO_VERSION.tar.gz

# Unzip
tar xvzf bamboo-$BAMBOO_VERSION.tar.gz

# Update file permission to allow bamboo to be executed.
cd bamboo-$BAMBOO_VERSION
chmod +x bamboo

# Test bamboo installation and show help message.
./bamboo --help
```

## Example Usage
```
bamboo -i input1.fastq input2.fastq -o output_dir --sample_size 50000 --seed 123
bamboo -b input.bam -r reference.fasta -o output_dir --realign
bamboo --sequence_path Bamboo-main/test/data/ecoli_hifi.reads.fastq.gz --reference_path Bamboo-main/test/data/ecoli.reference.fasta.gz -o test_bamboo_fastqtobam2 --sample_size 10000
```

## Command-line arguments

```
usage: bamboo [-h] [-b BAM_PATH] [-r REFERENCE_PATH] [--realign] [--minimap2_path MINIMAP2_PATH] [--minimap2_args MINIMAP2_ARGS] [--samtools_path SAMTOOLS_PATH]
              [--align_all] [-i SEQUENCE_PATH [SEQUENCE_PATH ...]] [-o OUTPUT_DIR] [-t THREADS] [--sample_size SAMPLE_SIZE] [--seed SEED] [--keep-intermediates]

Bamboo v0.2.0: a tool for quality control and error profiling of long-read sequencing data.

optional arguments:
  -h, --help            show this help message and exit

Sequence analyses:
  Arguments for sequence analyses.

  -i SEQUENCE_PATH [SEQUENCE_PATH ...], --sequence_path SEQUENCE_PATH [SEQUENCE_PATH ...]
                        Path to the input FASTQ file. If multiple input files are supplied, they will be concatenated before analyses. (default: None)

Alignment analyses:
  Arguments for alignment analyses.

  -b BAM_PATH, --bam_path BAM_PATH
                        Path to the input BAM file. (default: None)
  -r REFERENCE_PATH, --reference_path REFERENCE_PATH
                        Path to the reference FASTA file. (default: None)
  --realign             Re-align sampled reads using Minimap2. Use this option if the input BAM file does not contain x/= CIGAR operations. (default: False)
  --minimap2_path MINIMAP2_PATH
                        Path to Minimap2 executable. (default: minimap2)
  --minimap2_args MINIMAP2_ARGS
                        Command line arguments for Minimap2 (default: -ax map-ont --eqx --secondary=no -t 8)
  --samtools_path SAMTOOLS_PATH
                        Path to samtools executable. (default: samtools)
  --align_all           When `--bam_path` is not specified, perform alignment for all input reads (rather than just the sampled reads) to the reference genome.
                        Aligning all reads will improve accuracy of sequencing coverage analyses, but can take a considerable amount of time. (default: False)

General arguments:
  General input/output arguments.

  -o OUTPUT_DIR, --output_dir OUTPUT_DIR
                        Directory to save output figures and reports. (default: bamboo_report)
  -t THREADS, --threads THREADS
  --sample_size SAMPLE_SIZE
                        The number of reads to be analyzed. Use --sample_size -1 to disable random sampling and analyze all reads in the input data. (default:
                        100000)
  --seed SEED           Random seed for sampling. (default: 42)
  --keep-intermediates  Do not remove intermediate data files generated in the analyses. (default: False)
```



## Example output


```
output_dir
├── alignment
│   ├── Bamboo_report.ref_based.html
│   ├── bam_info.txt
│   ├── Coverage_and_bias
│   │   ├── gc_vs_coverage.png
│   │   ├── Genome_Fraction_Coverage.png
│   │   └── whole_genome_coverage.png
│   ├── genomic_coverage.temp.pickle
│   ├── Low_complexity_regions
│   │   ├── dimer_error_overall.png
│   │   ├── dimer_error_subplot.png
│   │   ├── dimer_heatmap_overall.png
│   │   ├── dimer_heatmaps_subplot.png
│   │   ├── homodimer_induced_errors.png
│   │   ├── homo_error_overall.png
│   │   ├── homo_error_subplot.png
│   │   ├── homo_heatmap_overall.png
│   │   ├── homo_heatmaps_subplot.png
│   │   └── homo_induced_errors_homolen.png
│   ├── Sequencing_accuracy
│   │   ├── error_along_readsite.event.png
│   │   ├── error_along_readsite.len.png
│   │   ├── indel_size.png
│   │   ├── long_reads_errors.png
│   │   ├── overall_error_rate.png
│   │   ├── per_read_error.png
│   │   ├── read_quality_vs_identity.png
│   │   ├── short_reads_errors.png
│   │   └── substitution_error_profile.png
└── sequence
│   ├── Bamboo_report.ref_free.html
│   ├── fastq_overall_stat.txt
│   ├── length_and_quality
│   │   ├── cumulative_plot.png
│   │   ├── length_distribution.png
│   │   ├── quality_distribution_across_read.png
│   │   ├── quality_distribution.png
│   │   ├── read_head_quality.png
│   │   ├── read_length_vs_quality.png
│   │   └── read_tail_quality.png
│   ├── read_content
│   │   ├── head_base_content.png
│   │   ├── heteropolymer_event_distribution.png
│   │   ├── heteropolymer_frequency_per10kb.png
│   │   ├── heteropolymer_length_distribution.png
│   │   ├── homopolymer_event_distribution.png
│   │   ├── homopolymer_frequency_per10kb.png
│   │   ├── homopolymer_length_distribution.png
│   │   ├── reads_gc.png
│   │   └── tail_base_content.png
├── Bamboo_report.combined.html
```

To review the findings presented in the web report, we have provided an illustrative example. Please access the document "[Bamboo_report.combined.html](./test/data/Bamboo_report.combined.html)" for detailed examination.

Please download and open html file to check the result with Web Browser, such as Google Chrome and so on.

## Documentation

For detailed documentation, please visit our [wiki](https://github.com/CycloneSEQ-Bioinformatics/Bamboo/wiki).

## Feedbacks

Please [raise an issue on GitHub](https://github.com/CycloneSEQ-Bioinformatics/Bamboo/issues) if you have any questions, suggestions or encountered any bugs. 

## Authors

- 程瀚森 Hansen Cheng (chenghansen@genomics.cn)
- 张嘉远 Jia-Yuan Zhang (zhangjiayuan@genomics.cn)
- 连明 Ming Lian (lianming@genomics.cn)

## License

Bamboo is distributed under the GPLv3 license. Source code will we released shortly. 

