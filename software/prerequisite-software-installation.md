
# Software that needs to be installed

- fastqc (https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) - [backup plan using Galaxy](https://usegalaxy.eu/?tool_id=toolshed.g2.bx.psu.edu%2Frepos%2Fdevteam%2Ffastqc%2Ffastqc%2F0.74%2Bgalaxy1&version=latest)
- megahit (https://github.com/voutcn/megahit) - [backup plan using Galaxy](https://usegalaxy.eu/?tool_id=toolshed.g2.bx.psu.edu%2Frepos%2Fiuc%2Fmegahit%2Fmegahit%2F1.2.9%2Bgalaxy1&version=latest)
- prodigal (https://github.com/hyattpd/Prodigal) - [backup plan using Galaxy](https://usegalaxy.eu/?tool_id=toolshed.g2.bx.psu.edu%2Frepos%2Fiuc%2Fbakta%2Fbakta%2F1.9.4%2Bgalaxy0&version=latest)
- roary (https://github.com/sanger-pathogens/Roary) - [backup plan using Galaxy](https://github.com/sanger-pathogens/Roary)


## FIRST DO THIS

***check if you have a `bin` folder in your home directory***

```
# make sure you are in your come directory
cd

# check contents
ls

# IF you do not see a folder call bin then do the following:
mkdir bin

```


Then  naviagte to the `software` folder in the repository for example (you can copy/paste the commands below)

> [!NOTE] 
> `$WORKSHOPPATH` below was defined in the instructions used to clone the workshop repository

---

## fastQC installation

This tool is used to assess the quality of DNA sequencing reads

Download fastqc executable for linux platforms using the following command:
```
cd $WORKSHOPPATH/software

# download the executable from theproject page
wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.12.1.zip

# unzip the archive
unzip fastqc_v0.12.1.zip

# Set up a symbolic link in your bin folder to the executable
ln -s $PWD/FastQC/fastqc ~/bin/fastqc


```

---

## megahit installation

This tool is used to reconstruct genomes from DNA sequencing reads

```
cd $WORKSHOPPATH/software

# clone the megahit repository
git clone https://github.com/voutcn/megahit.git

# change directory into the new megahit folder created from the previous step
cd megahit/

# Build the executable
git submodule update --init  # pulls an submodule dependancies
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=Release
make -j4
make simple_test

# Set up a symbolic link in your bin folder to the executable
ln -s $PWD/megahit ~/bin/megahit

```

---

## prodigal installation (instructions from [here](https://github.com/hyattpd/Prodigal/blob/GoogleImport/README.md))

This tool is used to annotate genomes (find genes etc.)

```
cd $WORKSHOPPATH/software

# clone the repository 
git clone https://github.com/hyattpd/Prodigal.git

# compile the code
cd Prodigal
make install  make install INSTALLDIR=./bin/

# Set up a symbolic link in your bin folder to the executable
ln -s $PWD/bin/prodigal ~/bin/prodigal

```

---

## roary installtion (instructions from [here](https://github.com/sanger-pathogens/Roary?tab=readme-ov-file#installation))

This tool is used to assess multiple genomes from a single species to understand the variability between individuals (the pangenome)


This installation is best done by installing a conda environment ([What is a conda environment?](https://docs.anaconda.com/working-with-conda/environments/))

```

conda config --add channels r
conda config --add channels defaults
conda config --add channels conda-forge
conda config --add channels bioconda
conda install roary

```

alternatively, using mamba (if installed/activated)

```
mamba create -n roary --no-channel-priority -c bioconda roary
```

> [!NOTE]
> Installing a tool as a conda environment, means it will be put in your default conda environment folder. If you want to know where that is type `conda info --base`. 
> You will need to "activate" the environment prior to every time you use the tool. To do that simply use the command `conda activate roary`





