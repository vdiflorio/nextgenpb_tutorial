---
title: Outputs
nav_order: 5
---

# Output Files and Log Interpretation

After running a simulation with `NextGenPB`, the software provides detailed output in both the terminal and several output files. This page guides you through interpreting the log output and visualizing the results with ParaView.

---

## 🧾 Terminal Log Output

When launching a simulation with:

```bash
mpirun -np 4 ngpb --potfile options.pot --pqrfile 2CCH.pqr
```

you will see a detailed runtime log describing every phase of the computation. Here’s a breakdown of the most important sections:

### System Information

This block provides general properties of the input molecular system:

```bash
Selected parameters file: options.pot
Selected pqr file:        2CCH.pqr

========== [ System Information ] ==========
  Number of atoms    : 4194
  Size protein [Å]   : [63.03, 7.915, -1.275]
  Net charge         : -3.000000e+00
  Solvent epsilon    : 2
  Solvent epsilon    : 80
  Temperature        : 298.15 [K] 
  Ionic strength     : 0.145 [mol/L] 
============================================
```

###  Domain and Grid Setup

This describes the computational domain and discretization settings:

``` bash
========== [ Domain Information ] ==========
  Scale:  2
  Center of the System [Å]:  [0.569, -2.2585, -0.1815]
  Perfil outer box:  0.2
  Complete Domain Box Size [Å]:
      x = [-255.431, 256.569]
      y = [-258.259, 253.742]
      z = [-256.182, 255.819]
  Perfil uniform grid:  0.9
  Uniform grid Size [Å]:
      x = [-37.431, 38.569]
      y = [-33.2585, 28.7415]
      z = [-31.6815, 31.3185]
  Number of Subdivisions in the Uniform grid:  nx = 152  ny = 124  nz = 126
============================================
```

These values define the bounding box and resolution of the computational mesh.


### Surface Generation (NanoShaper)

The surface of the molecule is built using NanoShaper, which detects the Solvent Excluded Surface (SES):

```bash
=== [ Building Surface with NanoShaper ] ===

 <<INFO>> TBB - User selected num threads 1
 <<INFO>> Starting grid new PB initialization
  ...
 <<INFO>> Cavity detection time is 0 [s]
 <<INFO>> Assembling octrees..ok!
 <<INFO>> Unpacking rays packet 0 of size 67380
============================================
```

###  Grid Construction

Each MPI rank builds a portion of the full octree mesh:

```bash
============ [ Building Grid ] =============
  [Rank 0] Local nodes     : 651261
  [Rank 0] Local quadrants : 655496
  [Rank 1] Local nodes     : 651261
  ...
  [Global] Total nodes     : 2563587
  [Global] Total quadrants : 2621984
============================================
```
This gives you an idea of the total size and resolution of the final mesh.


### Solving the Poisson–Boltzmann Equation

The numerical solution begins using the LIS library. It uses an iterative solver (e.g., CGS) with the specified preconditioner and stopping criteria.

```bash
== [ Starting numerical solution using LIS ] ==
Selected BCs          : Null
Time to calculate rho : 12 ms
initial vector x      : all components set to 0
precision             : double
linear solver         : CGS
preconditioner        : SSOR
convergence condition : ||b-Ax||_1 <= 0.0e+00*||b||_1 + 1.0e-06 = 1.0e-06
matrix storage format : CSR
iteration:     1  relative residual = 3.067417E+05
iteration:     2  relative residual = 2.069423E+06
iteration:     3  relative residual = 9.036003E+05
iteration:     4  relative residual = 6.532491E+05
iteration:     5  relative residual = 4.131005E+05
iteration:     6  relative residual = 1.842191E+05
iteration:     7  relative residual = 1.043384E+05
iteration:     8  relative residual = 1.087400E+05
iteration:     9  relative residual = 1.199109E+05
...
linear solver status  : normal end
============================================
```


### Energy results

```bash
================ [ Electrostatic Energy ] =================
  Net charge [e]:                                 -3.000000000000087
  Flux charge [e]:                                -3.00000000003496
  Polarization energy [kT]:                       -2487.179938212694
  Direct ionic energy [kT]:                       -8.537991557366576
  Coulombic energy [kT]:                          -64216.93379848526
  Sum of electrostatic energy contributions [kT]: -66712.65172825532
===========================================================
```

---

##  Output Files

After running a simulation with NextGenPB, the program generates several output files depending on the chosen [input options](files.md).
These files contain the results of the electrostatic calculations and are useful for visualization and analysis.

### Generated Files
 - `.vtu`:  One file per MPI rank containing the dielectric map and electrostatic potential.
 - `.pvtu`: A parallel VTK file that references all `.vtu` files. Use this to load the complete data in ParaView.
 - `phi_surf.txt`: Contains coordinates of surface vertices and the electrostatic potential values at those points.
 - `phi_nodes.txt`: Contains coordinates of boundary nodes and the electrostatic potential at each node.


### Visualizing with ParaView

1. Open **ParaView**
2. Go to `File > Open`, select the `.pvtk` file
3. Click **Apply** in the **Properties** panel
4. In the top bar, choose **Color By** → `Potential`

You should now see a colored map of the electrostatic potential.


### Visualizing on VMD, PyMOL, or ChimeraX

You cannot directly use .vtu or .pvtu files in VMD, PyMOL, or ChimeraX.

To visualize the electrostatic potential on the molecular surface with these tools, follow these steps:

#### Convert to CUBE Format

Use the provided Python script [`vtu2cube.py`](https://github.com/vdiflorio/NextGenPB/tree/main/scripts) located in the scripts/ directory of the NextGenPB repository.

```bash
python path_to_ngpb_dir/scritps/vtu2cube.py <pqr_name> --scale <grid_scale>
```

- `<pqr_name>`: Base name of your .pqr input file.
-`--scale`: (Optional) Controls the resolution of the grid. Default is 2.

This will generate a CUBE file `pqr_name.cube`.

You can now load the .cube file into VMD, PyMOL, or ChimeraX to visualize the electrostatic potential mapped onto the molecular surface.

> **Example**:
>```bash
> python ~/NextGenPB/scripts/vtu2cube.py 6VYB.pqr --scale 3
>```
>This will produce `6VYB.cube`

---
➡️ [Back to Run Instructions](run.md) | [Return to Tutorial Home](index.md)