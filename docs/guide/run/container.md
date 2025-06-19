---
title: Run with container
parent: Run
nav_order: 2
---


# Running Inside a Docker Container

Docker lets you encapsulate the solver and all dependencies inside a container, ensuring consistency and portability across different systems.

## Example 1: Binding a Specific Directory

If your input files (like `.prm`, `.pqr` or `.pdb`) are stored in a known directory, you can bind that directory into the container like this:

```bash
sudo docker run -v /your/local/files:/App -w /App <image_name>:latest mpirun -np 4 ngpb --prmfile options.prm
```

- Replace `/your/local/files` with the absolute path to your input files.
- Replace `<image_name>` with the name of your Docker image, e.g., nextgenpb.

## Example 2: Using the Current Working Directory

If you’re already in the directory where your input files are located, just run:

```bash
sudo docker run -v "$(pwd)":/App -w /App <image_name>:latest mpirun -np 4 ngpb --prmfile options.prm
```

{: .note }
Docker does not have access to your files by default—you must explicitly bind the directory using -v.


# Running with Apptainer / Singularity

For high-performance computing clusters or environments where Docker is unavailable, Apptainer (formerly known as Singularity) is the preferred alternative.

## Example 1: Binding a Specific Directory

```bash
apptainer exec --pwd /App --bind /your/local/files:/App /path/to/NextGenPB_ompi4.sif mpirun -np 4 ngpb --prmfile options.prm
```

## Example 2: Using the Current Working Directory

```bash
apptainer exec --pwd /App --bind "$(pwd)":/App /path/to/NextGenPB_ompi4.sif mpirun -np 4 ngpb --prmfile options.prm
```

- The `.sif` file is your Apptainer container image, e.g., NextGenPB_ompi4.sif.
- Replace `/path/to/NextGenPB_ompi4.sif` with the full path to your `.sif` file.

{: .note }
As with Docker, always bind the necessary directories to make your input files accessible inside the container.


# Alternative Input Method: --potfile

The --potfile flag can be used in place of --prmfile if you’ve already preprocessed your parameters:

`--potfile <file.pot>` – can be used instead of `--prmfile <file.prm>`

Example:

```bash
mpirun -np 4 ngpb --potfile options.prm --pqrfile molecule.pqr 
```

---
⬅️ Return to the [Installation Guide](/nextgenpb_tutorial/docs/guide/installation/container) if you haven’t installed the solver yet.

➡️ Go to [input files](/nextgenpb_tutorial/docs/guide/files/files) to learn everything about `.prm`, `.pot`, `.pqr`, and other input formats.