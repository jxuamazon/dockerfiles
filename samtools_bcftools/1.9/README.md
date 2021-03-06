# This image belongs to a larger project called Bioinformatics Docker Images Project (http://pegi3s.github.io/dockerfiles)
## (Please note that the original software licenses still apply)

This image facilitates the usage of [SAMtools-BCFtools](http://www.htslib.org/), a suite of programs for interacting with high-throughput sequencing data.

To see `SAMtools-BCFtools` options, just run:  `docker run --rm pegi3s/samtools_bcftools samtools --help` or `docker run --rm pegi3s/samtools_bcftools bcftools --help`.

# Using the SAMtools-BCFtools image in Linux
You should adapt and run the following command: `docker run --rm -v /your/data/dir:/data pegi3s/samtools_bcftools <samtools_or_bcftools> <options>`

In this command, you should replace:
- `/your/data/dir` to point to the directory that contains the input files you want to analyze.
- `<samtools_or_bcftools>` to the name of either `SAMtools` or `BCFtools` executable you want to use.
- `<options> ` with the specific options of the `SAMtools` or `BCFtools` application. These options will include the input/output files, which should be referenced under `/data/`.

For instance, if you want to generate a high quality VCF file from an input SAM file, you should run the following three steps: 

- `docker run --rm -v /your/data/dir:/data pegi3s/samtools_bcftools samtools view -bS /data/aln-pe.sam > aln-pe.bam`

- `docker run --rm -v /your/data/dir:/data pegi3s/samtools_bcftools bash -c "samtools mpileup -uf /data/chr19_KI270866v1_alt.fasta /data/aln-pe.bam | bcftools call -mv > /data/var.raw.vcf"`

- `docker run --rm -v /your/data/dir:/data pegi3s/samtools_bcftools bash -c "bcftools filter -s LowQual -e '%QUAL<20 || DP>100' /data/var.raw.vcf  > /data/var.flt.vcf"`

# Test data
To test the previous commands, the input SAM file used is available [here](https://raw.githubusercontent.com/pegi3s/dockerfiles/master/samtools_bcftools/1.9/test_data/aln-pe.sam), and the input FASTA file is available [here](https://raw.githubusercontent.com/pegi3s/dockerfiles/master/samtools_bcftools/1.9/test_data/chr19_KI270866v1_alt.fasta).

# Using the SAMtools-BCFtools image in Windows

Please note that data must be under the same drive than the Docker Toolbox installation (usually `C:`) and in a folder with write permissions (e.g. `C:/Users/User_name/`).

As in the Linux case, to run an application, you should adapt and run the following command: `docker run --rm -v "/c/Users/User_name/dir/":/data pegi3s/samtools_bcftools <samtools_or_bcftools> <options>`