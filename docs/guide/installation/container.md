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

You can download the generic image using the following command:

```bash
wget https://github.com/concept-lab/NextGenPB/releases/download/NextGenPB_v1.0.0/NextGenPB.sif
```

If you know the architecture of the machine where the container will run, you can download the image built specifically for that architecture.
See the [Downloads page](https://github.com/concept-lab/NextGenPB/releases/tag/NextGenPB_v1.0.0) for available versions.


➡️ Go to [Running NextGenPB](docs/guide/run/container) for execution details using the container.


## 🚶‍♂️ Prefer Customization? Build the Docker or Apptainer Image Yourself

This option is for advanced users who want to customize compiler flags, link libraries differently, or tweak performance settings.

### Tune the settings

If you are building the image on your machine, you can tune the installation by **uncommenting** the relevant line in `recipe.def` (for Apptainer) or in the `Dockerfile`:

```ini
# export CFLAGS="-O3 -mtune=native -march=native"
```

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
sudo apptainer build NextGenPB.sif recipe.def
```
To run the Apptainer image:

```bash
 apptainer exec --pwd /App --bind /path/to/files/:/App  /path/to/sif/NextGenPB.sif mpirun -np <number_of_processors> ngpb --prmfile options.prm
```

➡️ See the [Running NextGenPB](/nextgenpb_tutorial/docs/guide/run/container) guide for execution details using the container.


### Customize compiling flags to performance

---

✅ You’re now ready to run simulations with **NextGenPB**!

➡️ Go to [running section](/nextgenpb_tutorial/docs/guide/run/) to learn out to run the solver.
