![Bamboo logo](https://github.com/CycloneSEQ-Bioinformatics/_Bamboo/blob/chs_branch/bamboo_logo/bamboo_t1.png)

## 目录

- Bamboo
- Installation
- Usage
- Example Usage
- Output
- Documentation
- Contribution

# Bamboo

a tool for quality control and error profiling of long-read sequencing data.


## Installation

```sh
git clone https://github.com/CycloneSEQ-Bioinformatics/Bamboo.git
cd Bamboo
conda env create --file environment.yml --name bamboo_env
conda activate bamboo_env
cd Bamboo_tar && cat bamboo.tar.gz* |tar -zxv
#option (Long term method to add the bamboo path to the environment variable)
echo "export PATH=$PWD:$PATH" >>~/.bashrc
source ~/.bashrc
bamboo --help
#Temporarily add the bamboo path to the environment variable
export PATH=$PWD:$PATH
bamboo --help
```

## Usage
```
Sequence Analysis  
-i, --sequence_path : Path to the input FASTQ file(s). Multiple files can be provided and will be concatenated before analysis.  
--sample_size : Sample size for processing (default: 100000).  
--seed : Random seed for sampling (default: 42).  
Alignment Analysis  
-b, --bam_path : Path to the input BAM file.  
-r, --reference_path : Path to the reference FASTA file.  
--realign : Re-align reads if BAM file does not use --eqx parameter.  
--minimap2_path : Path to Minimap2 executable (default: minimap2).  
--minimap2_args : Command line arguments for Minimap2 (default: "-ax map-ont --eqx --secondary=no -t 8").  
--samtools_path : Path to samtools executable (default: samtools).  
General Arguments  
-o, --output_dir : Directory to save output figures and reports (default: bamboo_report).  
--keep-intermediates : Do not remove intermediate data files generated in the analyses.  
```
## Example Usage
```
bamboo -i input1.fastq input2.fastq -o output_dir --sample_size 50000 --seed 123
bamboo -b input.bam -r reference.fasta -o output_dir --realign
bamboo --sequence_path Bamboo-main/test/data/ecoli_hifi.reads.fastq.gz --reference_path Bamboo-main/test/data/ecoli.reference.fasta.gz -o test_bamboo_fastqtobam2 --sample_size 10000
```
## Output
eg:

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
For detailed documentation, please visit [the Bamboo wiki](https://github.com/CycloneSEQ-Bioinformatics/Bamboo/wiki).



### Contact

- chenghansen@genomics.cn
- zhangjiayuan@genomics.cn
- lianming@genomics.cn


### License

 [LICENSE.txt](https://github.com/CycloneSEQ-Bioinformatics/Bamboo/main/blob/LICENSE.txt)
