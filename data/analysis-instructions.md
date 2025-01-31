
# Anaysis:

## Step 1: Assess quality of DNA sequencing reads.

This step uses the tool fastqc to understand the quality metrics of the DNA sequencing data. A full description of what each metric represents, can be found [here](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/Help/).

> [!NOTE] 
> `$WORKSHOPPATH` below was defined in the instructions used to clone the workshop repository


```
#navigate to the data folder
cd $WORKSHOPPATH/data

# list the the sequencing files
ls *.fastq
```
You should see `genome_R1.fastq` and  `genome_R2.fastq`

DNA sequencing files usually come in pairs, the read with `R1` in its name is the forward read and `R2` the reverse read of a fragment of DNA (the red regions in the image below represent the reads).

![Paired end reads of next generation sequencing data mapped to a reference genome](https://commons.wikimedia.org/wiki/File:Mapping_Reads.png)

***fastq*** format files are an industry standard for representing DNA sequencing data which for a single read look like this:

```
@HWUSI-EAS100R:6:73:941:1973#0/1
GATTTGGGGTTCAAAGCAGTATCGATCAAATAGTAAATCCATTTGTTCAACTCACAGTTT
+
!''*((((***+))%%%++)(%%%%).1***-+*''))**55CCF>>>>>>CCCCCCC65
```
This represents:
- The first line starting with a `@` is a sequence identifier, which contains detailed information of the machine used to generate the data, the region of the flowcell and whether this is a forward or reverse read, see [here](https://en.wikipedia.org/wiki/FASTQ_format#Illumina_sequence_identifiers) for more details.

- The second line is the DNA strand belonging to the read
- The third line sometimes can have the sequence idntifier again or just a `+`
- The fourth line is an ASCII coding of the quality score of each DNA base, so there will be the same number of ASCII characters as there are DNA bases in line 2 (more information [here](https://en.wikipedia.org/wiki/FASTQ_format#Format).

To run the quality analysis type:
```
fastqc *.fastq
```

when this is finished, you can `scp` the `*.html` files to your computer and view them in a browser. You can compare them to exmaple reports for [good illumina data](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/good_sequence_short_fastqc.html) and [bad illumina data](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/bad_sequence_fastqc.html).

Often we may decide to do ***trimming*** of the DNA reads based on our interpretation of the DNA reads using tools like [Trimmomatic](https://github.com/usadellab/Trimmomatic) or [TrimGalore](https://github.com/usadellab/Trimmomatic) before proceeding to analysis. We will skip this step in this workshop.

Examples of the output are provided in the ***examples*** folder.


## Step 2: Assemble the Genome

This step will use the `megahit` tool. While in the folder that contains the two `fastq` files, run the following command:

```
megahit -1 genome_R1.fastq -2 genome_R2.fastq -o genome_assembly
```

Where: `-1` specifies the file contain the forward reads and `-2` specifes the file containing the reverse reads. `-o` specifies the directory to contain the results.

An example of the output can be found in the ***examples*** folder.

The file that contains the final assembly is called `final.contigs.fa`.







