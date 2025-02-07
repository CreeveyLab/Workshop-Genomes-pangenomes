# Workshop-Genomes-pangenomes

## Description

This describes a short workshop which gets users to install and use bioinformatics software in order to carry out seom prokaryotic genome analyses.

The repository has folders which contain instructions for each step.

> [!NOTE]
> If you are doing this workshop on a HPC, then you would be advised to start by setting up an interactive session and do everything in that.
> If you are using the QUB Kelvin2 HPC the command to do that is `srun --pty /bin/bash`

To use this repository start by doing the following:

```
# clone to repository
git clone https://github.com/CreeveyLab/Workshop-Genomes-pangenomes.git

```

Create an environmental variable to keep track of the location where the repository is cloned

```
export WORKSHOPPATH="`realpath $PWD`/Workshop-Genomes-pangenomes"
```

## Installation of software (This may not be necessary depending on your system)

Follow the instructions in `software/prerequisite-software-installation.md` [Online link here](https://github.com/CreeveyLab/Workshop-Genomes-pangenomes/blob/main/software/prerequisite-software-installation.md)

## Begin Analysis

Follow the instructions in `data/analysis-instructions.md` [Online link here](https://github.com/CreeveyLab/Workshop-Genomes-pangenomes/blob/main/data/analysis-instructions.md)

Example outputs for all the analyses in the workshop can be found in the `example_results` folder.