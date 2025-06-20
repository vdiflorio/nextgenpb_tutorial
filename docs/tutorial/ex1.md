---
title: First Run
parent: Tutorial
nav_order: 2
---

# Exercise One: First Run

In this first exercise, we will verify that **NextGenPB** is correctly set up by running a basic electrostatic potential calculation using a real protein input.

---

## Step 1 – Prepare the Inputs

First, enter the directory for the first exercise:

```bash
cd ex1
```

This is your working directory for this example:

```ini
~/ngpb_tutorial/ex1
```

Inside the `NextGenPB` repository ([cloned](/nextgenpb_tutorial/docs/tutorial/stage) in `~/ngpb_tutorial/NextGenPB`), you’ll find a `data/` folder containing sample inputs.

Copy the a `.pqr` file into your current directory:

```bash
cp ../NextGenPB/data/1CCM.pqr .
```

### Prepare the Parameter File

Create a file named `options.prm` in the current directory with the following content:

```
[input]
filetype = pqr
filename = 1CCM.pqr
[../]

[mesh]
mesh_shape = 0
perfil1 = 0.95
perfil2 = 0.2
scale   = 2.0
[../]

[model]
bc_type = 1                                
molecular_dielectric_constant = 2      # Dielectric constant inside the molecule
solvent_dielectric_constant   = 80     # Dielectric constant of the solvent (e.g., water)
ionic_strength                = 0.145  # Ionic strength (mol/L)
T                             = 298.15 # Temperature in Kelvin
calc_energy    = 2
calc_coulombic = 1
[../]
```

---

## Step 2 – Run the Solver

Now launch the simulation using Apptainer:

```bash
apptainer exec --pwd /App --bind .:/App NextGenPB.sif mpirun -np 4 ngpb --prmfile options.prm
```

## Step 3 – Output and Results

At the end of the execution, you will see a log similar to this:

```ini
...
```

This includes the timing report and the electrostatic energy of the system.
These outputs confirm that NextGenPB is functioning correctly and that your configuration is valid.

---

➡️ Go to the next [exercise](/nextgenpb_tutorial/docs/tutorial/ex2).
