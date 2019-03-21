---
title: "Peak calling with MACS2"
teaching: 15
exercises: 30
questions:
- "How do we go from aligned reads to transcription factor binding sites across the genome?"
objectives:
- "Be familiar with peak calling using the MACS2 software packages."
- "Be aware of the different arguments, and their default values, in MACS2 when peak calling." 
keypoints:
- "MACS2 is an easy to use peak caller with a range of different options that can be provided." 
---
# Installing MACS2

Installation of MACS2 requires that you have Python installed on your system. This is not usually an issue for any Linux and Mac machine, since these systems usually ship with Python already installed. Also, you will need *Numpy* and a *GCC* compiler installed. *Numpy* can be installed using the *pip* package manager of Python, while *GCC* comes installed with all Linux distributions (for Mac users, you will need to install X-Code). Once you have these packages installed, installing MACS2 is simply done using the following:

~~~
pip install MACS2
~~~
{: .bash}

One way to check if MACS2 is properly installed is to issue the command `macs2` at the terminal. You should see the following, which will indicate that MACS2 has been installed: 

~~~
usage: macs2 [-h] [--version]                                                                                                                                                                    {callpeak,bdgpeakcall,bdgbroadcall,bdgcmp,bdgopt,cmbreps,bdgdiff,filterdup,predictd,pileup,randsample,refinepeak}                                                                   ...                                                                                                                                                                    macs2: error: too few arguments 
~~~
{: .output}

# Peak callking with MACS2

## Predicting fragment size

In ChIP-seq, the reads sequenced usually represents the ends of the DNA fragment and not the exact
binding location. One common strategy that is used to recover the actual binding site is to extend
the reads to include the actual binding site. However, one needs to determine how long should the
reads be extended to. There are two ways of doing this, namely:

1. Make assumptions about the length. For instance, one commonly used value for transcription factor binding site is 200bp. 

2. Use the strand cross-correlation to estimate a fragment length. This can be done using `macs2 predictd`. 

For today's purpose, we will use the latter to estimate the size of our DNA fragment. As earlier mentioned, this will be done using `macs2 predictd`. Typing the following will have the list of arguments printed on STDOUT:

~~~
macs2 predictd -h 
~~~
{: .bash}

The list of arguments are as follows: 

~~~
usage: macs2 predictd [-h] -i IFILE [IFILE ...]
                      [-f {AUTO,BAM,SAM,BED,ELAND,ELANDMULTI,ELANDEXPORT,BOWTIE,BAMPE,BEDPE}]
                      [-g GSIZE] [-s TSIZE] [--bw BW] [-m MFOLD MFOLD]
                      [--outdir OUTDIR] [--rfile RFILE] [--verbose VERBOSE]

~~~
{: .output}

To use `predictd`, all we really need to provide is the *bam* file in the `-i` argument. `-g` will need to be provided if we are using a different genome (the default is `hs`, which is *Homo sapiens*). 


## Identification of binding sites

Peak calling in MACS2 is done using `macs2 callpeak`. There are a large number of different options that we can provide. We can call the **help** page using `macs2 callpeaks -h`. The following will be seen:

~~~
usage: macs2 callpeak [-h] -t TFILE [TFILE ...] [-c [CFILE [CFILE ...]]]
                      [-f {AUTO,BAM,SAM,BED,ELAND,ELANDMULTI,ELANDEXPORT,BOWTIE,BAMPE,BEDPE}]
                      [-g GSIZE] [--keep-dup KEEPDUPLICATES]
                      [--buffer-size BUFFER_SIZE] [--outdir OUTDIR] [-n NAME]
                      [-B] [--verbose VERBOSE] [--trackline] [--SPMR]
                      [-s TSIZE] [--bw BW] [-m MFOLD MFOLD] [--fix-bimodal]
                      [--nomodel] [--shift SHIFT] [--extsize EXTSIZE]
                      [-q QVALUE | -p PVALUE] [--to-large] [--ratio RATIO]
                      [--down-sample] [--seed SEED] [--tempdir TEMPDIR]
                      [--nolambda] [--slocal SMALLLOCAL] [--llocal LARGELOCAL]
                      [--broad] [--broad-cutoff BROADCUTOFF]
                      [--cutoff-analysis] [--call-summits]
                      [--fe-cutoff FECUTOFF]
~~~
{: .output}

For `callpeak`, the main arguments that we will provide are `-t`, `-c`, `--extsize` (which we get from `predictd`), `-n` and `-g` if we are not using the human genome. We will also specify `--nomodel` since we are specifying the fragment size. The additional option `--outdir` can be specified if we want the results of our analysis to be saved in another directory.

 Refer to the website [[https://github.com/taoliu/MACS#call-peaks]] for more information on the other arguments (such as `-m` and `-q` that can be changed depending on analysis needs). 

> ## Let's do this! 
>
> Perform peak calling using MACS2 to identify E2F1 binding loci across the genome, by first using
> `predictd` to estimate the fragment size and thereafter, specifying it as the desired extension
> size in the MACS2 peak calling option.
{: .challenge}

Once MACS2 is done, we will get a few files containing the genomic coordinates of the binding sites. Other than the `xlsx` file which is tabular, all the other files are based on the BED format with additional columns for other information. Of interest is the `peaks-narrowPeak` file, contains information on all the peaks found. In the next section of this session, we will annotate the files and thereafter, perform some simple downstream analysis. 
