# Sustain bioinformatics workshop

## Before workshop:
1. Install software\
check that tools actually work i.e. prodigal --help

2. Download Data \
You may use the instructions below or follow Chris' intructions on GitHub (link below)

## Step 1: Install software

##### Option 1:Docker (needed for James Gillespie's practical)
###### NOTE --> works best on computers NOT HPC systems (convert to singularity if working on a HPC that does not support Docker --> see singularity instructions)
Install Docker(if you don't have already)
Mac https://docs.docker.com/desktop/setup/install/mac-install/
```
# also if you have homebrew
brew install docker
```
Windows:
https://docs.docker.com/desktop/setup/install/windows-install/

Linux:
https://docs.docker.com/engine/install/

######  Pull the container
```
docker pull ldillon26/bioinformatics-tools:latest

# test docker container prior to workshop!! For example:
docker run -it ldillon26/bioinformatics-tools:latest bash
prodigal --help
(ctrl D to exit)
```
if you cannot access files try:
```
docker run -it \
  -v $(pwd):/data \
  ldillon26/bioinformatics-tools:latest bash
```

##### Option 2: Using singularity on the HPC
(see instructions at bottom of the page)

##### Option 3: follow Chris' instructions to install software:
https://github.com/CreeveyLab/Workshop-Genomes-pangenomes/tree/main

##### Option 4: use Galaxy online
https://usegalaxy.org/


## Step 2: Git clone Chris' repo
```
git clone https://github.com/CreeveyLab/Workshop-Genomes-pangenomes.git
```

## Step 3: move into correct directory
```
cd Workshop-Genomes-pangenomes/

# you can check folders by using ls 
ls
```
# Using Docker on your local machine:
### Step 1: Start Docker
```
docker run -it ldillon26/bioinformatics-tools:latest bash
```
Note: Docker can only see the files and subdirectories in the directory that it is started running in (i.e. is cannot see what is in the previous directory)
(if using chris' installation please follow the intructions on the GitHub for the following steps https://github.com/CreeveyLab/Workshop-Genomes-pangenomes/tree/main)


### Step 2: FastQC
```
cd fastQC-read-quality
fastqc *.fastq.gz
```

### Step 3: Megahit

```
# change to the assembly folder
cd ../megahit-assembly

# view the files for analysis
ls

# assemble the noisy data
megahit -1 megahit-assembly/toy-genome-noisy_R1.fastq.gz -2 megahit-assembly/toy-genome-noisy_R2.fastq.gz -o assembly-noisy


# assemble the perfect data
megahit -1 toy-genome-perfect_R1.fastq.gz -2 toy-genome-perfect_R2.fastq.gz -o assembly-perfect
```

### Step 4: Annotate the genomes
```
# change to the assembly folder
cd ../prodigal-genome-annotation

# view the files for analysis
ls

# run prodigal
for i in E-coli-bacterial_genome.*.fas; do 
	prodigal -i $i -f gff -o $i.gff; 
	done
# look at the top 10 lines of one of the gff output files
head E-coli-bacterial_genome.aaa.fas.gff

```

### Step 5: Carry out a Pangenome analysis (roary)
```
# change to the assembly folder
cd ../roary-pangenome

# view the files for analysis
ls

roary *.gff -p 8
```

# Using singularity on the HPC
Docker does not typically work on a HPC so you can use singularity on the same container

### Load singularity module (this will be specific to your hpc)
```
# load singularity on kelvin2 
module load apps/singularity/3.10.0     
```

### Git clone workshop repo 
```
# move to where you want data to be
# clone to repository
git clone https://github.com/CreeveyLab/Workshop-Genomes-pangenomes.git
```

### move into data directory
```
cd Workshop-Genomes-pangenomes/data
```

### Pull container from Docker
```
singularity pull docker://ldillon26/bioinformatics-tools:latest
```

### Now run fastqc on reads
This next command assumes that you pulled the singularity container in the data dir of the project folder.
```
singularity exec \
  --bind /mnt/scratch2/users/40309916/Workshop-Genomes-pangenomes/data \
  bioinformatics-tools_latest.sif fastqc fastQC-read-quality/*.fastq.gz
```

### Assemble reads into contigs
```
singularity exec \
  --bind /mnt/scratch2/users/40309916/Workshop-Genomes-pangenomes/data \
  bioinformatics-tools_latest.sif megahit -1 megahit-assembly/toy-genome-noisy_R1.fastq.gz -2 megahit-assembly/toy-genome-noisy_R2.fastq.gz -o assembly-noisy  
```

### Annotate the genomes
```
singularity exec \
  --bind /mnt/scratch2/users/40309916/Workshop-Genomes-pangenomes/data \
  bioinformatics-tools_latest.sif \
  bash -c 'for i in prodigal-genome-annotation/E-coli-bacterial_genome.*.fas; do prodigal -i $i -f gff -o $i.gff; done'
```
### Perform a pagenome analysis
```
singularity exec \
  --bind /mnt/scratch2/users/40309916/Workshop-Genomes-pangenomes/data \
  bioinformatics-tools_latest.sif \
  roary roary-pangenome/*.gff -p 1
 ```


# Using conda environments:
If you are using conda environments please use Chris' instructions:
https://github.com/CreeveyLab/Workshop-Genomes-pangenomes/blob/main/data/analysis-instructions.md

# Galaxy
If you are unable to install anything on your computer or HPC please use galaxy online web tool.\
https://usegalaxy.org/
