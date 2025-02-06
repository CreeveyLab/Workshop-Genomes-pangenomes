
# Anaysis:

> [!NOTE] 
> `$WORKSHOPPATH` below was defined in the instructions used to clone the workshop repository

```
#navigate to the data folder
cd $WORKSHOPPATH/data

# list the the sequencing files
ls 
```
You should see folders belonging to each step of the analysis that we will do today:
 ```
fastQC-read-quality
megahit-assembly
prodigal-genome-annotation
roary-pangenome
 ```

---

## Step 1: Assess quality of DNA sequencing reads (fastQC).



This step uses the tool fastqc to understand the quality metrics of the DNA sequencing data. A full description of what each metric represents, can be found [here](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/Help/).


Enter the folder `fastQC-read-quality` using the command: `cd fastQC-read-quality` 


DNA sequencing files usually come in pairs, the read with `R1` in its name is the forward read and `R2` the reverse read of a fragment of DNA (the red regions in the image below represent the reads).

![Paired end reads of next generation sequencing data mapped to a reference genome](https://upload.wikimedia.org/wikipedia/commons/2/2e/Mapping_Reads.png)

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

Use the command `gzcat Illumina-reads-100kbasicUniform_R1.fastq.gz | head` to look at the first 10 lines of one of the files. **Note** that as the file is compressed with *gzip*, we need to use `gzcat` to uncompress it on the fly before sending it to the `head` command.


### To run the quality analysis type:
```
fastqc *.fastq.gz
```

when this is finished, you can `scp` the `*.html` files to your computer and view them in a browser. You can compare them to exmaple reports for [good illumina data](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/good_sequence_short_fastqc.html) and [bad illumina data](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/bad_sequence_fastqc.html).

Often we may decide to do ***trimming*** of the DNA reads based on our interpretation of the DNA reads using tools like [Trimmomatic](https://github.com/usadellab/Trimmomatic) or [TrimGalore](https://github.com/usadellab/Trimmomatic) before proceeding to analysis. We will skip this step in this workshop.

Examples of the output are provided in the ***examples/fastQC*** folder.


---

## Step 2: Assemble the Genome (megahit)

Enter the folder that contains the assembly data:

```
# change to the assembly folder
cd $WORKSHOPPATH/data/megahit-assembly

# view the files for analysis
ls
```

Provided are two example datasets for assembly called **noisy** and **perfect**, you will reconstruct the genomes of each.

```
toy-genome-noisy_R1.fastq.gz   toy-genome-noisy_R2.fastq.gz   

toy-genome-perfect_R1.fastq.gz toy-genome-perfect_R2.fastq.gz
```


This step will use the `megahit` tool. We need to provide the forward (R1) and reverse (R2) files for the assembly of each genome. 

The commands to do this are:


```
# assemble the noisy data
megahit-assembly]$ megahit -1 toy-genome-noisy_R1.fastq.gz -2 toy-genome-noisy_R2.fastq.gz -o assembly-noisy


# assemble the perfect data
megahit -1 toy-genome-perfect_R1.fastq.gz -2 toy-genome-perfect_R2.fastq.gz -o assembly-perfect
```

Where: `-1` specifies the file contain the forward reads and `-2` specifes the file containing the reverse reads. `-o` specifies the directory to contain the results.

These datasets are greatly reduced in size for the  purpose of the workshop, usually these analyses could take in the region of hours to complete, however yours should finish within about 1 minute.

An example of the output for both of these commands can be found in the ***example_results/megahit*** folder.

The file that contains the final assembly in the output folders `assembly-perfect` and `assembly-noisy` is called `final.contigs.fa`.

---

## Step 3: Annotate the genomes.


> [!NOTE] 
> Annotation is the tern used to detect genes and other functional units in genomes.
> More information can be found here: https://en.wikipedia.org/wiki/DNA_annotation


Enter the folder that contains the genomes for annotation:

```
# change to the assembly folder
cd $WORKSHOPPATH/data/prodigal-genome-annotation

# view the files for analysis
ls

```

Provided are 10 real *Escherichia coli* genoems that we will annotate"

```
E-coli-bacterial_genome.aaa.fas E-coli-bacterial_genome.aad.fas E-coli-bacterial_genome.aag.fas E-coli-bacterial_genome.aaj.fas
E-coli-bacterial_genome.aab.fas E-coli-bacterial_genome.aae.fas E-coli-bacterial_genome.aah.fas
E-coli-bacterial_genome.aac.fas E-coli-bacterial_genome.aaf.fas E-coli-bacterial_genome.aai.fas
```

You can use a `for` loop to run prodigal on all of the genomes sequentially:

```
for i in E-coli-bacterial_genome.*.fas; do 
	prodigal -i $i -f gff -o $i.gff; 
	done

```

Where in each loop `$i` will contain the name of one of the genome files. The annotation will have the extension `.gff` at the end.


Have a look at an example of this output file:

```
# look at the top 10 lines of one of the gff output files
head bacterial_genome.aaa.fas.gff

```


This will look like this:

```
# Sequence Data: seqnum=1;seqlen=4641867;seqhdr="accn|CP123277   Escherichia coli strain 11A.B25 chromosome.   [Escherichia coli 11A.B25 | 562.126128]"
# Model Data: version=Prodigal.v2.6.3;run_type=Single;model="Ab initio";gc_cont=50.79;transl_table=11;uses_sd=1
accn|CP123277	Prodigal_v2.6.3	CDS	3	98	1.2	+	0	ID=1_1;partial=10;start_type=Edge;rbs_motif=None;rbs_spacer=None;gc_cont=0.427;conf=56.60;score=1.16;cscore=-1.56;sscore=2.72;rscore=0.00;uscore=0.00;tscore=3.22;
accn|CP123277	Prodigal_v2.6.3	CDS	336	2798	336.5	+	0	ID=1_2;partial=00;start_type=ATG;rbs_motif=GGAG/GAGG;rbs_spacer=5-10bp;gc_cont=0.533;conf=99.99;score=336.49;cscore=321.09;sscore=15.40;rscore=11.20;uscore=0.93;tscore=3.93;
accn|CP123277	Prodigal_v2.6.3	CDS	2800	3732	109.5	+	0	ID=1_3;partial=00;start_type=ATG;rbs_motif=AGGAG;rbs_spacer=5-10bp;gc_cont=0.558;conf=100.00;score=109.48;cscore=89.26;sscore=20.23;rscore=14.92;uscore=0.12;tscore=3.93;
accn|CP123277	Prodigal_v2.6.3	CDS	3733	5019	201.7	+	0	ID=1_4;partial=00;start_type=ATG;rbs_motif=GGA/GAG/AGG;rbs_spacer=5-10bp;gc_cont=0.534;conf=99.99;score=201.69;cscore=197.46;sscore=4.23;rscore=3.01;uscore=-3.98;tscore=3.93;
accn|CP123277	Prodigal_v2.6.3	CDS	5233	5529	6.5	+	0	ID=1_5;partial=00;start_type=GTG;rbs_motif=AGGAG;rbs_spacer=5-10bp;gc_cont=0.539;conf=81.67;score=6.50;cscore=-0.66;sscore=7.16;rscore=14.92;uscore=-0.08;tscore=-6.53;
accn|CP123277	Prodigal_v2.6.3	CDS	5682	6458	119.5	-	0	ID=1_6;partial=00;start_type=ATG;rbs_motif=AGGA;rbs_spacer=5-10bp;gc_cont=0.490;conf=100.00;score=119.46;cscore=102.62;sscore=16.85;rscore=10.99;uscore=1.93;tscore=3.93;
accn|CP123277	Prodigal_v2.6.3	CDS	6528	7958	135.7	-	0	ID=1_7;partial=00;start_type=ATG;rbs_motif=GGAG/GAGG;rbs_spacer=5-10bp;gc_cont=0.531;conf=100.00;score=135.75;cscore=122.80;sscore=12.95;rscore=11.20;uscore=-1.52;tscore=3.93;


```

A description of the GFF format is available here: https://en.wikipedia.org/wiki/General_feature_format


An example of the output for this command can be found in the ***example_results/prodigal*** folder.

---

## Step 4: Carry out a Pangenome analysis (roary)

> [!NOTE] 
> A pangenome analysis allows an understanding of the distribution of gene families across individuals of a species.
> More information can be found here: https://en.wikipedia.org/wiki/Pan-genome

Enter the folder that contains the annotations for generating the pangenome analysis:

```
# change to the assembly folder
cd $WORKSHOPPATH/data/roary-pangenome

# view the files for analysis
ls

```

Provided are 10 annotations using *PROKKA* in GFF format:

```
bacterial_genome.aaa.fas.gff bacterial_genome.aad.fas.gff bacterial_genome.aag.fas.gff bacterial_genome.aaj.fas.gff
bacterial_genome.aab.fas.gff bacterial_genome.aae.fas.gff bacterial_genome.aah.fas.gff
bacterial_genome.aac.fas.gff bacterial_genome.aaf.fas.gff bacterial_genome.aai.fas.gff
```

To run roary and the pangenome analysis do the following:

```
roary *.gff -p 8
```
This will take the infomration from all the GFF files and use 8 processors to do the analysis.


The roary website has links to tools that can allow you to generate visualisations of your results: https://sanger-pathogens.github.io/Roary/


When finished, look insides the result file `summary_statistics.txt`

```
cat summary_statistics.txt
```
to see the high-level overview of the pangenome, for instnace:

```
Core genes	(99% <= strains <= 100%)	483
Soft core genes	(95% <= strains < 99%)	0
Shell genes	(15% <= strains < 95%)	5307
Cloud genes	(0% <= strains < 15%)	8956
Total genes	(0% <= strains <= 100%)	14746
```
Investigate the other files to learn more about the specific details.

An example of the output for this command can be found in the ***example_results/roary*** folder.




