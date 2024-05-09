# mcdr-mtb-utilities-v2
## Introduction

mcdr-mtb-utilities-v2 provides script to generate VCF files from *Mycobacterium tuberculosis* WGS data. These VCF files can be used as input for the webserver of [**mcdr-mtb-v2**](http://bicresources.jcbose.ac.in/ssaha4/mcdr-mtb-v2/) that performs prediction of drug resistance from Variant calling format (VCF) files.

## Scripts

There are 2 different scripts:

* variant_call.sh - Generate VCF file from MTB WGS data (from .fastq to .vcf)
* merge_vcf.sh - Merge multiple VCF files into a single merged.vcf

## Pre-requisites
* trim-galore (version `0.6.7`) - quality check and trimming of read sequences
* bwa (`0.7.17-r1188`) - reference based alignment
* samtools (`1.13`)- processing the BAM files
* freebayes (`v1.3.6`) - variant calling
* libvcflib-tools (`1.0.7`) - processing the VCF files
* libvcflib-dev (`1.0.7`)	- processing the VCF files
* bgzip (`1.13+ds`) - zipping files

## Installation
  Step 1: Install dependent packages/tools<br/>
   *For ubuntu*

      sudo apt-get install trim-galore bwa samtools freebayes libvcflib-tools libvcflib-dev bgzip

The installation steps for the different packages/tools are given in the following links:

* trim-galore - https://github.com/FelixKrueger/TrimGalore
* bwa - https://github.com/lh3/bwa
* samtools, bcftools, bgzip(htstools) - http://www.htslib.org/download/
* freebayes - https://github.com/freebayes/freebayes
* vcflib - https://github.com/vcflib/vcflib

  R should be installed in the user system/PC. R installation steps are given in https://cran.r-project.org/.

  Step 2: Install mcdr-mtb-utilities-v2

   I. Download the software from GitHub repository

   Create a clone of the repository

      git clone https://github.com/AbhirupaGhosh/mcdr-mtb-utilities-v2

  **Note:** Creating a clone of the repository requires git to be installed.

  The git can be installed using

    sudo apt-get install git

   **OR**

  Download using wget

    wget https://github.com/AbhirupaGhosh/mcdr-mtb-utilities-v2/archive/refs/heads/main.zip

    unzip main.zip

  **Note:** wget can be installed using

      sudo apt-get install wget

  II. Make the shell scripts executable

      chmod +x INSTALLATION_DIR/mcdr-mtb-utilities-v2 config.sh variant_call.sh merge_vcf.sh

  `INSTALLATION_DIR` = Directory where mcdr-mtb-utilities-v2 is installed

  III. update the paths in config.sh (optional)

   the `config.sh` looks like

      freebayes_path=/usr/bin/freebayes
      samtools_path=/usr/bin/samtools
      bwa_path=/usr/bin/bwa
      trim_galore_path=/usr/bin/trim_galore
      vcflib_path=/usr/bin/vcflib
      bgzip_path=/usr/bin/bgzip
      bcftools_path=/usr/bin/bcftools
      trim_galore_cores=4
      bwa_mem_cores=4
      samtools_cores=4


  **Note:** It shows the default paths of the executables files for `freebayes`, `samtools`, `bwa`, `trim galore!`, `vcflib`, `bgzip` and `bcftools`. The users need to update the paths of the executables, in case these tools were installed in ways other than the `apt-get install` command.

## Usage

  Initially change the directory to the directory where mcdr-mtb-utilities-v2 is installed

      cd INSTALLATION_DIR/mcdr-mtb-utilities-v2

Different operations can be performed by calling the appropriate scripts with two command-line arguments: `INPUT_DIR` and `OUTPUT_DIR`.

`INPUT_DIR` = the path (absolute or relative) of the folder containing the input files.

`OUTPUT_DIR` = the path (absolute or relative) of the folder in which `mcdr-mtb-utilities-v2` will store the outputs.

The executable script, and contents of `INPUT_DIR` and `OUTPUT_DIR` depends on the choice of operations. The different operations are explained below.

### 1. Generate VCF file from MTB WGS data (from .fastq to .vcf)

    ./variant_call.sh INPUT_DIR OUTPUT_DIR

`INPUT_DIR` must contain paired end FASTQ files (ISOLATE1_1.fastq.gz & ISOLATE1_2.fastq.gz) of 1 isolate.

`OUTPUT_DIR` will contain a folder for each ISOLATE ID (ISOLATE_DIR).

Each folder will contain
* the VCF file (ISOLATE1.vcf)
* the intermediate BAM files (ISOLATE1.bam, ISOLATE1_sorted.bam)

### 2. Generate merged VCF file from multiple MTB VCFs

	 ./merge_vcf.sh INPUT_DIR OUTPUT_DIR

`INPUT_DIR` must contain One or more VCFs (ISOLATE1.vcf, ISOLATE2.vcf) of MTB isolates.

`OUTPUT_DIR` will contain the merged.vcf file along with the compressed VCF files and their index files.

## Demo

### 1. Generate VCF file from MTB WGS data
#### a. Single isolate

  1. Create an Input directory

 		 mkdir /home/user/Input_Dir1

  2. Get Data

  Download the whole genome sequencing FASTQ files of a MTB isolate run, ERR137249 (ERR137249_1.fastq & ERR137249_2.fastq) from https://www.ebi.ac.uk/ena/browser/view/ERR137249

  3. Store these files in `Input_Dir1`
  4. Create an Output directory

 		 mkdir /home/user/Output_Dir1

  5. Go to the `mcdr-mtb-utilities-v2` installation directory

 		 cd INSTALLATION_DIR/mcdr-mtb-utilities-v2

  6. Run variant-call.sh

 		 ./variant-call.sh /home/user/Input_Dir1/ /home/user/Output_Dir1/

`Input_Dir1` contains ERR137249_1.fastq, ERR137249_2.fastq

`Output_Dir1` contains -
* Folder - ERR137249
* ERR137249.tsv

The ERR137249 folder contains -

* reference folder - reference genome and index files
* Trim galore outputs - ERR137249_1_val_1.fq.gz, ERR137249_2_val_2.fq.gz, ERR137249_1_trimming_report.txt, ERR137249_2_trimming_report.txt
* Bwa-mem output - ERR137249.bam
* Intermediate BAM files - ERR137249_fix.bam, ERR137249_namesort.bam, ERR137249_positionsort.bam, ERR137249_markdup.bam
* BAM index - ERR137249.bam.bai
* Freebayes output - ERR137249.vcf

#### b. Multiple isolates

  1. Create an Input directory

    mkdir /home/user/Input_Dir2

  2. Get Data

Download the whole genome sequencing FASTQ files of MTB ISOLATE runs, ERR137249 (ERR137249_1.fastq & ERR137249_2.fastq) and SRR1103491 (SRR1103491_1.fastq & SRR1103491_2.fastq) from https://www.ebi.ac.uk/ena/browser/view/ERR137249 and https://www.ebi.ac.uk/ena/browser/view/SRR1103491

  3. Store these files in `Input_Dir2`
  4. Create an Output directory

    mkdir /home/user/Output_Dir2

  5. Go to the `mcdr-mtb-utilities-v2` installation directory

    cd INSTALLATION_DIR/mcdr-mtb-utilities-v2

  6. Run merge-VCF.sh

    ./merge-VCF.sh /home/user/Input_Dir1/ /home/user/Output_Dir2/

`Input_Dir2` contains ERR137249_1.fastq, ERR137249_2.fastq, SRR1103491_1.fastq, SRR1103491_2.fastq

`Output_Dir2` contains -
* Two folders - ERR137249, SRR1103491
* merged.vcf

Each of the ERR137249 and SRR1103491 named folder contains -

* reference folder - reference genome and index files
* Trim galore outputs - ISOLATENAME_1_val_1.fq.gz, ISOLATENAME_2_val_2.fq.gz, ISOLATENAME_1_trimming_report.txt, ISOLATENAME_2_trimming_report.txt
* Bwa-mem output - ISOLATENAME.bam
* Intermediate BAM files - ISOLATENAME_fix.bam, ISOLATENAME_namesort.bam, ISOLATENAME_positionsort.bam, ISOLATENAME_markdup.bam
* BAM index - ISOLATENAME.bam.bai
* Freebayes output - ISOLATENAME.vcf

## Team

**Abhirupa Ghosh, Sudipto Bhattacharjee and Sudipto Saha**

## Disclaimer

The scripts were developed and tested on the Ubuntu Operating system.





