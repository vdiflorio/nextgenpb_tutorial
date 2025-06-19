---
title: Containers
parent: Installation
nav_order: 2
nav_enabled: true
---

This is the easiest way to get **NextGenPB** running on your computer.
{: .fs-6 .fw-300 }


# Quick Start Options

##  🏃 Very Impatient? Use a Precompiled Apptainer Image

If you're eager to run simulations without compiling anything, use the ready-made `.sif` container image.

### Prerequisite

- Install **Apptainer** (formerly Singularity):  
  [https://apptainer.org/docs/](https://apptainer.org/docs/)


### Download the image

```bash
wget ....
```

➡️ Go to [Running NextGenPB](docs/run/index.md) for execution details using the container.


## 🚶‍♂️ Prefer Customization? Build the Docker or Apptainer Image Yourself

This option is for advanced users who want to customize compiler flags, link libraries differently, or tweak performance settings.

### Build the Docker image (if root access available)

```bash
sudo docker build -f Dockerfile -t <name_of_image>:latest .
```
To run the Docker image:
```bash
sudo docker run -v "$(pwd)":/App -w /App <name_of_image>:latest mpirun -np <number_of_processors> ngpb --prmfile options.prm
```

### Build the Apptainer image (if root access available)

```bash
sudo apptainer build NextGenPB_ompi4.sif recipe.def
```
To run the Apptainer image:
```bash
 apptainer exec --pwd /App --bind /path/to/files/:/App  /path/to/sif/NextGenPB_ompi4.sif mpirun -np <number_of_processors> ngpb --prmfile options.prm
```
➡️ See the [Running NextGenPB](/nextgenpb_tutorial/docs/run/container) guide for execution details using the container.


### Customize compiling flags to performance

---

✅ You’re now ready to run simulations with **NextGenPB**!

➡️ Go to [running section](/nextgenpb_tutorial/docs/run/) to learn out to run the solver.
