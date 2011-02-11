# sickle - A windowed adaptive trimming tool for FASTQ files using quality

## About

Most modern sequencing technologies produce reads that have
deteriorating quality towards the 3'-end. Incorrectly called bases
here negatively impact assembles, mapping, and downstream
bioinformatics analyses.

Sickle is a tool that uses sliding windows along with quality and length
thresholds to determine when quality is sufficiently low to trim the 
3'-end of reads.  It will also discard reads based upon the length threshold.
It takes the quality values and slides a window across them whose length is 0.1
times the length of the read.  If this value is less than 1, then the window
is set to be equal to the length of the read.  Otherwise, the window slides
along the quality values until the average quality in the window drops 
below the threshold.  At that point the algorithm determines where in the 
window the drop occurs and cuts both the read and quality strings there. 
However, if the cut point is less than the minimum length threshold, then
the read is discarded entirely. 

sickle supports four types of quality values: illumina, solexa, phred, and sanger.

## Requirements 

Sickle requires a C compiler; GCC or clang are recommended. Sickle
relies on Heng Li's kseq.h, which is bundled with the source.

Sickle also requires Zlib, which can be obtained at
<http://www.zlib.net/>.

## Building and Installing Sickle

To build Sickle, enter:

    make

Then, copy or move "sickle" to a directory in your $PATH.

## Usage

Sickle has two modes to work with both paired-end and single-end
reads: `sickle se` and `sickle pe`.

Running sickle by itself will give print the help:

    sickle

Running sickle with either the "se" or "pe" commands will give help specific to those commands:

    sickle se
    sickle pe

### sickle se

sickle se takes an input fastq file and outputs a trimmed version of that file. 
It also has options to change the length and quality thresholds for trimming.

Examples:

    sickle se -f input_file.fastq -t illumina -o trimmed_output_file.fastq
    sickle se -f input_file.fastq -t illumina -o trimmed_output_file.fastq -q 33 -l 40

### sickle pe

sickle pe takes two paired-end files as input and outputs two trimmed paired-end files 
as well as a "singles" file.  This file contains reads that passed filter in one of the
paired-end files but not the other.  You can also change the length and quality thresholds 
for trimming.

Examples:

    sickle pe -f input_file1.fastq -r input_file2.fastq -t sanger -o trimmed_output_file1.fastq -p trimmed_output_file2.fastq -s trimmed_singles_file.fastq
    sickle pe -f input_file1.fastq -r input_file2.fastq -t sanger -o trimmed_output_file1.fastq -p trimmed_output_file2.fastq -s trimmed_singles_file.fastq -q 12 -l 15

