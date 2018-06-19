MACARON User Guide
================

# Table of Contents

[//]: # (BEGIN automated TOC section, any edits will be overwritten on next source refresh)

* [Introduction](#introduction)
* [Installation](#installation)
    * [Operating System Guidelines](#operating-system-guidelines)
    * [Runtime Pre-requisite](#runtime-pre-requisite)
    * [Software Dependencies](#software-dependencies)
    * [Downloading the Source Code](#downloading-the-source-code)
    * [Contents of the Folder MACARON_GenMed](#contents-of-the-folder-macaron_genmed)
* [Running the MACARON](#running-the-macaron)
    * [Input Requirements](#input-requirements)
    * [Default Options](#default-options)
    * [demo Folder](#demo-folder)
    * [Advanced Options](#advanced-options)
* [MACARON Reporting Format](#macaron-reporting-format)
* [Known Limitations](#known-limitations)

[//]: # (END automated TOC section, any edits will be overwritten on next source refresh)

# Introduction

MACARON (Multi-bAse Codon-Associated variant Re-annotatiON) is a python framework to identify and re-annotate multi-base affected codons in whole genome/exome sequence data. Starting from a standard VCF file, MACARON identifies, re-annotates and predicts the amino acid change resulting from multiple single nucleotide variants (SNVs) within the same genetic codon. 

The information below includes how to install and run MACARON to filter a list of variant records (from VCF file) called by any existing SNP-based variant caller to identify SNVs with the same genetic codon and correct their corresponding amino acid change.

# Installation

### Operating System Guidelines

MACARON is know to run on LINUX UBUNTU 16.04 LTS. However, MACARON can be run on any other LINUX version.

### Runtime Pre-requisite

__1.__ MACARON is executable in __PYTHON v2.7 or later__. If the user has multiple PYTHON versions, please make sure that your running environment is set to the required version of PYTHON.

__2.__ Check your __JAVA__ version as MACARON is tested with:

      java -version
      openjdk version __"1.8.0_151"__
      OpenJDK Runtime Environment (build 1.8.0_151-8u151-b12-0ubuntu0.16.04.2-b12)
      OpenJDK 64-Bit Server VM (build 25.151-b12, mixed mode)

### Software Dependencies

Before running MACARON, please make sure that following software are installed properly:

__1.__ __Genome-Analysis Toolkit__ (https://software.broadinstitute.org/gatk/download/).

__2.__ __SnpEff__ (tested with __v4.3__ (build 2017-05-05 18:41). However, MACARON can also run with any older or newer version (http://snpeff.sourceforge.net/download.html).

__3.__ __SAMTools__ (tested with version __0.1.19__), however any version can be used.

__4.__ __Human Reference Genome__: Depends on user’s input.

__5.__ __SnpEff’s Human Annotation Database__: Depends on user’s input.

For __1__ and __2__, as long as they are compatible with JAVA, MACARON has no issues.

### Downloading the Source Code

The most prefered way to use the lastest version of MACARON is:

      git clone https://github.com/waqasuddinkhan/MACARON-GenMed-LabEx.git

or download the ZIP folder.

MACARON source code can also be downloaded from http://www.genmed.fr/images/publications/data/MACARON_GenMed.zip

After acquiring a release distribution of the source code, the build procedure is to unpack the zip file:

      unzip MACARON_GenMed.zip

### Contents of the folder MACARON_GenMed

* *MACARON*  –  The MACARON python code
* *MACARON_validate.sh*  –  a BASH-shell script to validate multi-SNVs located on the same read that affect the same genetic codon

# Running the MACARON

### Input Requirements

Before running MACARON, check these __input technical notes__ as the following limitations exist for either the input VCF file, or the required software dependencines:

- Chromosome (chr) notation should be compatible with both input VCF file and Human Reference Genome file, or vice versa,

- Sequence dictionaries of input VCF file and Human Reference Genome file should be the same,

- Input VCF file (should) suitably be annotated with ANNOVAR, and additionally with any other annotation software, e.g, VEP (https://www.ensembl.org/info/docs/tools/vep/index.html) if the user has a desire to get the full functionality of -f option (see below),

- Same Human Reference Genome file should be used for MACARON which is practiced earlier for alignemnt and (or) to call variant sets,

- Versions of input VCF file, Human Reference Genome file and SnpEff database file should be the same (hg19 / GRCh37 = SnpEff GRCh37.75) or (hg38 / GRCh38 = SnpEff GRCh38.86).

### Default Options

For a full list of MACARON executable options, run:

      python MACARON -h

By default, MACARON depends on the `GLOBAL VARIABLES` set in the script before run:

      ## GLOBAL VARIABLES (IMPORTANT: You can set the default values here)
      GATK="/home/wuk/software/GenomeAnalysisTK.jar"
      #GATK="/home/wuk/software/gatk-4.0.1.2/gatk-package-4.0.1.2-local.jar"
      HG_REF="/home/wuk/Working/gnme_refrnces/Homo_sapiens_assembly19.fasta"
      SNPEFF="/home/wuk/software/snpEff/snpEff.jar"
      SNPEFF_HG="GRCh37.75" ## SnpEff genome version

To run MACARON with __GATK <4.0__, simply type:

      python MACARON -i test_input.vcf

If running with __GATK >= 4.0__, make following changes:

      #GATK="/home/wuk/software/GenomeAnalysisTK.jar"
      GATK="/home/wuk/software/gatk-4.0.1.2/gatk-package-4.0.1.2-local.jar"
      HG_REF="/home/wuk/Working/gnme_refrnces/Homo_sapiens_assembly19.fasta"
      SNPEFF="/home/wuk/software/snpEff/snpEff.jar"
      SNPEFF_HG="GRCh37.75" ## SnpEff genome version

and run with:

      python MACARON -i test_input.vcf --gatk4

### demo Folder

To help verify a successful installation, MACARON includes a small demo data set:

* *variants_of_interest.vcf* –  a test VCF file to check the functionality of MACARON
* *MACARON_output.txt*  –  The output file generated by running the MACARON
* *sub1.chr22_21349676-21349677.sample02.bam*  –  a subset of BAM file used as input for MACARON_validate.sh
* *MACARON_validate.txt*  –  The output file with read count information of concerned pcSNV in sample02 (in this case).
(All files are referenced with hg19)

`cd` to `demo` folder and run:

      python ../MACARON -i variants_of_interest.vcf

MACARON_output.txt is the default output file name of MACARON. User can change it with `-o` option.

      python ../MACARON -i variants_of_interest.vcf -o variants_of_interest.txt

### Advanced Options

MACARON can be run by invoking paths directly set from the command-line:

```bash
python ../MACARON -i variants_of_interest.vcf --GATK /home/wuk/software/GenomeAnalysisTK.jar --HG_REF /home/wuk/Working/gnme_refrnces/Homo_sapiens_assembly19.fasta --SNPEFF /home/wuk/software/snpEff/snpEff.jar --SNPEFF_HG GRCh37.75
```
For __GATK >= 4.0__:

```bash
python ../MACARON -i variants_of_interest.vcf --gatk4 --GATK /home/wuk/software/ --HG_REF /home/wuk/Working/gnme_refrnces/Homo_sapiens_assembly19.fasta --SNPEFF /home/wuk/software/snpEff/snpEff.jar --SNPEFF_HG GRCh37.75
```
MACARON can add additional fields, besdies the dafault (see [MACARON Reporting Format](#macaron-reporting-format)) by using `-f` option:

* -f CSQ (if input VCF file is additionally annotated with VEP, the output txt file also has the same complete annotation for each variant record)

* -f EFF (if user wants to output SnpEff annotations in output txt file), or -f ANN (if SnpEff is used without -formatEff option)

* -f QUAL,DP,AF,Func.refGene,Gene.refGene,GeneDetail.refGene (this will keep any other default annotations of input VCF file and of ANNOVAR to output txt file)

-f can be used multiple times, e.g.,

* -f CSQ,DP,Func.refGene
or
* -f FILTER,EFF,CSQ,AF

The order of the fields in the output txt file depends on the order of INFO field headers used in `-f`.

```bash
python ../MACARON -i variants_of_interest.vcf --gatk4 --GATK /home/wuk/software/ --HG_REF /home/wuk/Working/gnme_refrnces/Homo_sapiens_assembly19.fasta --SNPEFF /home/wuk/software/snpEff/snpEff.jar --SNPEFF_HG GRCh37.75 -f QUAL,FILTER,SIFT_pred 
```
Without `-f` option, `QUAL` field is outputted as default.If user wants to keep `QUAL` along with any other field, `-f` should mentiond `QUAL` in addition to other field headers: `-f QUAL,FILTER,SIFT_pred`. If only `-f SIFT_pred` is used, `QUAL` field is over-written by `SIFT_pred` field.

# VALIDATING SNVs' PRESENCE ON THE SAME READ

To confirm the existence of multi-SNVs within the same genetic codon, an accessory BASH-shell script (available with MACARON) calculates the read count information of affected bases. Using SAMTools, generate a subset of your BAM file (it should be the same used to generate VCF file for MACARON run) covering 50 bps over each clustered SNVs.

example: 

```bash
samtools view –hb –L sub1.bed sample02.bam > sub1.chr22_21349676-21349677.sample02.bam`
```

```bash
sub1.bed
chr22	21349676
```

The naming format of output BAM file should be the same.

Once subset BAM files are generated, run MACARON_validate.sh.

example: `MACARON_validate.sh sub1.chr22_21349676-21349677.sample02.bam`

This will generate an output text file (`MACARON_validate.txt`) allowing the user for further analysis.

Additional files that will be needed are:

A Note on TECHNICAL ISSUES:






## REFERENCES
* Van der Auwera G.A., et al. (2013) From FastQ Data to High-Confidence Variant Calls: The Genome Analysis Toolkit Best Practices Pipeline, Curr Protoc Bioinformatics, 43:11.10.1-11.10.33.
* Cingolani, P., et al. (2012) A program for annotating and predicting the effects of single nucleotide polymorphisms, SnpEff: SNPs in the genome of Drosophila melanogaster strain w1118; iso-2; iso-3, Fly, 6, 80-92.
* McLaren, W., et al. (2010) Deriving the consequences of genomic variants with the Ensembl API and SNP Effect Predictor, Bioinformatics, 26, 2069-2070.
* Wang, K., Li, M. and Hakonarson, H. (2010) ANNOVAR: functional annotation of genetic variants from high-throughput sequencing data, Nucleic Acids Res, 38, e164.

*CONTACT: david-alexandre.tregouet@inserm.fr*

*VERSION: 0.3*
*VERSION DATE: 11th April, 2018*

If you use [MACARON](https://academic.oup.com/bioinformatics/advance-article-abstract/doi/10.1093/bioinformatics/bty382/4992149?redirectedFrom=fulltext), please cite:

*Khan W. et al. MACARON: a python framework to identify and re-annotate multi-base affected codons in whole genome/exome sequence data, Bioinformatics 2018*
