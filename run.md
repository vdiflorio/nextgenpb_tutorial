---
layout: default
title: Running NextGenPB
nav_order: 3
---

# ⚙️ Running the NextGenPB Solver

This page provides examples and explanations for running **NextGenPB** after installation or container setup. Whether you're using a local build, Docker, or Apptainer, this guide covers it all.

---

## ⌨️ General Syntax

The basic command to run NextGenPB is:

```bash
mpirun -np <number_of_processors> ngpb --prmfile options.prm
```

If you have a .pqr file, you can also specify it using the flag:

```bash
mpirun -np <number_of_processors> ngpb --prmfile options.prm --pqrfile file.pqr
```

Where:
-	`<number_of_processors>` is the number of MPI ranks you want to use (greater or equal to 1).
- `options.prm` is your input file containing parameters for the solver.
- `file.pqr` is your PQR file
  
➡️ More details on input files can be found [here](file.md).

---
## 🧪 Example with Local Compilation

If you compiled ngpb from source and it’s available in your `PATH`:

```bash
mpirun -np 4 ngpb --prmfile options.prm
```
Or with a `.pqr` file:

```bash
mpirun -np 4 ngpb --prmfile options.prm --pqrfile 1CCM.pqr
```

## 🐳 Running with Docker

After building the Docker image:

```bash
sudo docker run -v /your/local/files:/App -w /App <image_name>:latest mpirun -np 4 ngpb --prmfile options.prm
```

Alternatively, if you’re already in the input file directory:

```bash
sudo docker run -v "$(pwd)":/App -w /App <image_name>:latest mpirun -np 4 ngpb --prmfile options.prm
```
This assumes:
- Your `.prm`, `.pqr` or `.pdb` files are located in `/your/local/files` 
- You’re using 4 MPI processes (you can increase or decrease this number depending on your machine’s capabilities)

📝 Replace `/your/local/files` with the absolute path to your input directory.
If you’re already in the directory containing the input files, you can safely use `"$(pwd)"`.


## 📦 Running with Apptainer / Singularity

Using a `.sif` container:

```bash
apptainer exec --pwd /App --bind /your/local/files:/App /path/to/NextGenPB_ompi4.sif mpirun -np 4 ngpb --prmfile options.prm
```

Alternatively, if you’re already in the input file directory:

```bash
apptainer exec --pwd /App --bind "$(pwd)":/App /path/to/NextGenPB_ompi4.sif mpirun -np 4 ngpb --prmfile options.prm
```

This assumes the same as for Docker:
- Your `.prm`, `.pqr` or `.pdb` files are located in `/your/local/files`
- You’re using 4 MPI processes (you can increase or decrease this number depending on your machine’s capabilities)

📝 Again, replace `/your/local/files` with the correct absolute path on your host system.
If you’re already in the directory containing the input files, you can safely use `"$(pwd)"`.


## 🧾 Additional Options

NextGenPB accepts several other options in the command line:
- `--potfile <file.pot>` – can be used instead of `--prmfile <file.prm>`

Example:
```bash
mpirun -np 8 ngpb --potfile options.prm --pqrfile molecule.pqr 
```


## 🧠 Tips for Effective Runs

- Always check the `.log` file generated during the run for diagnostic messages.
- You can redirect output using `>` to capture logs:

```bash
mpirun -np 4 ngpb --prmfile options.prm > run.log 2>&1
```


---
⬅️ Return to the [Installation Guide](install.md) if you haven’t installed the solver yet.

➡️ Go to [input files](file.md) to learn everything about .prm, .pot, .pqr, and other input formats.

