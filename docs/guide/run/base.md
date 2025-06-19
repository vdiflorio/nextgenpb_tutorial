---
title: Basic Command Syntax
parent: Run
nav_order: 1
---

# Basic Command Syntax

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

{: .note }
Details about the structure and purpose of `.prm`, `.pqr`, and `.pdb` files are provided in the [Input Files guide](/nextgenpb_tutorial/docs/guide/files).

---