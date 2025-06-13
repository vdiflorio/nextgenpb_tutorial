---
layout: default
title: Running NextGenPB
nav_order: 3
---

# ⚙️ Running the NextGenPB Solver

Once you’ve installed or containerized **NextGenPB**, you're ready to begin running simulations.  
This guide walks you through different ways to launch the solver—whether you've compiled it locally, or you're using it within a Docker or Apptainer (Singularity) container.

We'll also explain what the basic commands mean, provide examples, and share useful best practices so you can get the most out of your simulations.

---

## ⌨️ Basic Command Syntax

The most common way to run **NextGenPB** is through the `mpirun` command, which enables parallel execution using MPI (Message Passing Interface).  
Even though the solver is designed for parallelism, **you can use it with any number of processors, starting from 1**:

```bash
mpirun -np <number_of_processors> ngpb --prmfile options.prm
```

Let’s break this down:
- `mpirun` is the standard MPI launcher command.
-	`-np <number_of_processors>`  tells MPI how many parallel processes (or “ranks”) to run. This number must be greater than or equal to 1.
-	`ngpb` is the executable name for the NextGenPB solver.
- `--prmfile options.prm`  is a required argument that specifies the simulation parameters.
  
If your simulation also involves a specific molecular structure, you can provide a .pqr file that contains atomic coordinates and charges:

```bash
mpirun -np <number_of_processors> ngpb --prmfile options.prm --pqrfile molecule.pqr
```

📘 **Note**: Details about the structure and purpose of `.prm`, `.pqr`, and `.pdb` files are provided in the [Input Files guide](files.md).

---
## 🧪 Running Locally (Compiled Version)

If you’ve built NextGenPB from source and added the executable to your system `PATH`, you can launch it directly from your terminal.

Here are a couple of examples:

🔹 **Without a .pqr file:**
```bash
mpirun -np 4 ngpb --prmfile options.prm
```

🔹 **With a .pqr file:**
```bash
mpirun -np 4 ngpb --prmfile options.prm --pqrfile 1CCM.pqr
```

💡 You can adjust the number of processors to suit your system’s capabilities. Even on a laptop, running with `-np 1` is perfectly valid.


## 🐳 Running Inside a Docker Container

Docker lets you encapsulate the solver and all dependencies inside a container, ensuring consistency and portability across different systems.

### Example 1: Binding a Specific Directory

If your input files (like `.prm`, `.pqr` or `.pdb`) are stored in a known directory, you can bind that directory into the container like this:

```bash
sudo docker run -v /your/local/files:/App -w /App <image_name>:latest mpirun -np 4 ngpb --prmfile options.prm
```

- Replace `/your/local/files` with the absolute path to your input files.
- Replace `<image_name>` with the name of your Docker image, e.g., nextgenpb.

### Example 2: Using the Current Working Directory

If you’re already in the directory where your input files are located, just run:

```bash
sudo docker run -v "$(pwd)":/App -w /App <image_name>:latest mpirun -np 4 ngpb --prmfile options.prm
```

📝 **Notes**: Docker does not have access to your files by default—you must explicitly bind the directory using -v.


## 📦 Running with Apptainer / Singularity

For high-performance computing clusters or environments where Docker is unavailable, Apptainer (formerly known as Singularity) is the preferred alternative.

### Example 1: Binding a Specific Directory

```bash
apptainer exec --pwd /App --bind /your/local/files:/App /path/to/NextGenPB_ompi4.sif mpirun -np 4 ngpb --prmfile options.prm
```

### Example 2: Using the Current Working Directory

```bash
apptainer exec --pwd /App --bind "$(pwd)":/App /path/to/NextGenPB_ompi4.sif mpirun -np 4 ngpb --prmfile options.prm
```

- The `.sif` file is your Apptainer container image, e.g., NextGenPB_ompi4.sif.
- Replace `/path/to/NextGenPB_ompi4.sif` with the full path to your `.sif` file.

📝 **Notes**: As with Docker, always bind the necessary directories to make your input files accessible inside the container.


## 🧾 Alternative Input Method: --potfile

The --potfile flag can be used in place of --prmfile if you’ve already preprocessed your parameters:

`--potfile <file.pot>` – can be used instead of `--prmfile <file.prm>`

Example:

```bash
mpirun -np 4 ngpb --potfile options.prm --pqrfile molecule.pqr 
```


## 🧠 Tips and Best Practices

- Always check the `.log` file after each run. It contains essential diagnostic information and any warning messages from the solver.
- Redirect your solver’s output to a log file for easier review:
```bash
mpirun -np 4 ngpb --prmfile options.prm > run.log 2>&1
```
- For heavy simulations, make sure your system has enough memory and CPU resources for the number of MPI processes you’re using.

---
⬅️ Return to the [Installation Guide](install.md) if you haven’t installed the solver yet.

➡️ Go to [input files](files.md) to learn everything about `.prm`, `.pot`, `.pqr`, and other input formats.

