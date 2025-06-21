---
title: Setting the Stage
parent: Tutorial
nav_order: 1
---

# Setting the Stage

## Install Apptainer

If you don’t have Apptainer installed, follow the instructions below for your system:

### Red Hat Enterprise Linux

```bash
sudo dnf install -y epel-release
sudo dnf install -y apptainer
```

### Ubuntu

```bash
sudo add-apt-repository -y ppa:apptainer/ppa
sudo apt update
sudo apt install -y apptainer
```

For more details, visit the official [Apptainer installation guide](https://apptainer.org/docs/admin/main/installation.html#).

## Prepare the Tutorial Directory

For this tutorial, we’ll create a working directory where you can run the solver.

```bash
mkdir ngpb_tutorial
cd ngpb_tutorial
```

Prepare also the direcotries for the exercise:

```bash
mkdir ex1
mkdir ex2
mkdir ex3
mkdir ex4
```

### Download the container

```bash
wget ...
```

### Clone the NextGenPB repository

```bash
git clone https://github.com/concept-lab/NextGenPB.git
```

This repository contains the source code (for those interested in development) as well as standard input files, which we’ll use throughout the tutorial.

---

Now you’re ready to start testing the core features of **NextGenPB** with practical exercises.

➡️ Go to the next [exercise](/nextgenpb_tutorial/docs/tutorial/ex1).