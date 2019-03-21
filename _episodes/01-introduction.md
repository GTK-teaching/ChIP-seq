---
title: "Introduction"
teaching: 15
exercises: 0
questions:
- "What are the main steps in analyzing ChIP-seq data?"
objectives:
- "Be familiarized with the major steps involved in ChIP-seq data analysis."
keypoints:
- "ChIP-seq data analysis involves alignment, peak calling and downstream analysis, the latter of which depends on the specific question the researcher aims to address."
---

# Introduction 

Chromatin immunoprecipitation (ChIP) followed by massively parallel sequencing (ChIP-seq) is becoming increasingly popular as the method of choice for the analysis of transcription factor binding sites as well as histone modifications. An overview of the method has been discussed in lecture. As an NGS method, a huge amount of data in the form of reads will be obtained. As bioinformaticians, our job is to identify gene loci which are enriched for the binding of said transcription factor. Further, our job also often involves interpreting the massive number of binding events that are reported. 

# Overview of today's session 
For today's class, a set of aligned reads in the **bam** format is provided as the starting point of our practical. These reads were aligned to the reference genome using *bowtie2*, which is assumed that everyone is familiar with using. The session will be broken into 3 sections, namely: (1) peak calling, (2) annotation and (3) motif enrichment analysis. 
The data for today is a subset of aligned reads taken from GEO (accession ID: GSE67809), which studies the genome-wide binding loci of the transcription factor E2F1. Two files -- a ChIP and an input -- are provided. 

> ## Software packages required
>
> The software packages that are required for today includes (1) *macs2* for peak calling.
> Installation instruction for each package will be provided prior to the commencement of analysis using said package.
{: .callout}


> ## Getting the files for today
>
> The files for todays class can be downloaded from atlas using the following commands:
>
> `scp guests@atlas.cbis.nus.edu.sg:/home/guests/chipseq/*-sub.bam.gz .`
> `scp guests@atlas.cbis.nus.edu.sg:/home/guests/refGene.bed.gz .`
>
> The above commands will have the files downloaded to the present directory from which you run these commands (indicated by `.` in our `scp` command). Thereafter, you will need to decompress these files. This can be done using the following commands:
>
> `gzip -d *.gz`
> `gzip -d refGene.bed.gz`
{: .callout}

 
