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
usage: bamboo [-h] [-b BAM_PATH] [-r REFERENCE_PATH] [--realign] [--minimap2_path MINIMAP2_PATH] [--minimap2_args MINIMAP2_ARGS]
              [--samtools_path SAMTOOLS_PATH] [-i SEQUENCE_PATH [SEQUENCE_PATH ...]] [-o OUTPUT_DIR] [--sample_size SAMPLE_SIZE] [--seed SEED]
              [--keep-intermediates]

Bamboo: a tool for quality control and error profiling of long-read sequencing data.

optional arguments:
  -h, --help            show this help message and exit

Sequence analyses:
  Arguments for sequence analyses.

  -i SEQUENCE_PATH [SEQUENCE_PATH ...], --sequence_path SEQUENCE_PATH [SEQUENCE_PATH ...]
                        Path to the input FASTQ file. If multiple input files are supplied, they will be concatenated before analyses. (default:
                        None)

Alignment analyses:
  Arguments for alignment analyses.

  -b BAM_PATH, --bam_path BAM_PATH
                        Path to the input BAM file. (default: None)
  -r REFERENCE_PATH, --reference_path REFERENCE_PATH
                        Path to the reference FASTA file. (default: None)
  --realign             Re-align sampled reads using Minimap2. Use this option if the input BAM file does not contain x/= CIGAR operations.
                        (default: False)
  --minimap2_path MINIMAP2_PATH
                        Path to Minimap2 executable. (default: minimap2)
  --minimap2_args MINIMAP2_ARGS
                        Command line arguments for Minimap2 (default: -ax map-ont --eqx --secondary=no -t 8)
  --samtools_path SAMTOOLS_PATH
                        Path to samtools executable. (default: samtools)

General arguments:
  General input/output arguments.

  -o OUTPUT_DIR, --output_dir OUTPUT_DIR
                        Directory to save output figures and reports. (default: bamboo_report)
  --sample_size SAMPLE_SIZE
                        The number of reads to be analyzed. Use --sample_size -1 to disable random sampling and analyze all reads in the input
                        data. (default: 100000)
  --seed SEED           Random seed for sampling. (default: 42)
  --keep-intermediates  Do not remove intermediate data files generated in the analyses. (default: False)
```



## Example output


```
output_dir
├── /sequence/
│  ├──fastq_overall_stat.txt
│  ├──fastq_report.html
│  ├──length_and_quality
│  │  ├──cumulative_plot.png
│  │  ├──length_distribution.png
│  │  ├──qc_ref_free_head_qua.png
│  │  ├──qc_ref_free_tail_qua.png
│  │  ├──quality_distribution_across_read.png
│  │  ├──quality_distribution.png
│  │  ├──read_len_vs_q.png
│  ├──read_content
│  │  ├──dimer_event_dis.png
│  │  ├──dimer_len_dis.png
│  │  ├──homo_event_dis.png
│  │  ├──homo_len_dis.png
│  │  ├──tail_base_content.png
│  │  ├──dimer_freq_per10kb.png
│  │  ├──head_base_content.png
│  │  ├──homo_freq_per10kb.png
│  │  ├──reads_gc.png
├── /alignment/
│  ├──bam_info.txt
│  ├──bam_report.html
│  ├──error_and_bias
│  │  ├──gc_distribution.png
│  │  ├──gc_vs_coverage.png
│  │  ├──indel.png
│  │  ├──overall_error_rate.png
│  │  ├─Genome_Fraction_Coverage.png
│  │  ├─per_read_error.png
│  │  ├─read_quality_vs_identity.png
│  │  ├─substitution_error_profile.png
│  │  ├─whole_genome_coverage.png
│  ├──homopolymer_and_dimer
│  │  ├──dimer_error_overall.png
│  │  ├──dimer_error_subplot.png
│  │  ├──dimer_heatmap_overall.png
│  │  ├──dimer_heatmaps_subplot.png
│  │  ├──homo_error_overall.png
│  │  ├──homo_error_subplot.png
│  │  ├──homo_heatmap_overall.png
│  │  ├──homo_heatmaps_subplot.png
└──combined_report.html

```

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

