# Workshop-Genomes-pangenomes

## Description

This describes a short workshop which gets users to install and use bioinformatics software in order to carry out seom prokaryotic genome analyses.

The repository has folders which contain instructions for each step.

To use this repository start by doing the following:

```
# clone to repository
git clone https://github.com/CreeveyLab/Workshop-Genomes-pangenomes.git

```

Create an environmental variable to keep track of the location where the repository is cloned

```
export WORKSHOPPATH="`realpath $PWD`/Workshop-Genomes-pangenomes"
```

## Installation of software

Follow the instructions in `software/prerequisite-software-installation.md` [Online link here](https://github.com/CreeveyLab/Workshop-Genomes-pangenomes/blob/main/software/prerequisite-software-installation.md)

## Begin Analysis

Follow the instructions in `data/analysis-instructions.md` [Online link here](https://github.com/CreeveyLab/Workshop-Genomes-pangenomes/blob/main/data/analysis-instructions.md)



```
salloc --time=02:00:00 --ntasks=1 --cpus-per-task=8 --mem=32G --gres=gpu:1 --partition=a100_full
```

