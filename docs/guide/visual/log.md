--- 
title: Log Output
parent: Outputs
nav_order: 1
---

# Understanding the Terminal Log Output

When you run a simulation using NextGenPB with a command like:

```bash
mpirun -np 4 ngpb --potfile options.pot --pqrfile 1CCM.pqr
```

the program prints a detailed log to the terminal. This log shows what the software is doing at each step of the calculation. Understanding this output helps you verify the simulation setup, track progress, and troubleshoot if something goes wrong.

Below, we explain the main parts of the log and what each section means.

## System Information

This section summarizes the basic properties of the molecular system you’re simulating. It confirms that the input files were loaded correctly and shows key parameters like the number of atoms, net charge, and physical conditions.

### Example snippet
{: .text-delta }

```bash
Selected parameters file: options.pot
Selected pqr file:        1CCM.pqr

========== [ System Information ] ==========
  Number of atoms    : 642
  Size protein [Å]   : [27.815, 34.156, 24.478]
  Solvent epsilon    : 2
  Solvent epsilon    : 80
  Temperature        : 298.15 [K] 
  Ionic strength     : 0.145 [mol/L] 
============================================
```

##  Domain and Grid Setup

Next, the software defines the computational domain — the 3D box around your molecule where calculations happen, and how finely it divides that space (the grid resolution).

### Example snippet
{: .text-delta }

``` bash
========== [ Domain Information ] ==========
  Scale:  2
  Center of the System [Å]:  [1.1775, 0.068, 0.07]
  Perfil outer box:  0.5
  Complete Domain Box Size [Å]:
      x = [-62.8225, 65.1775]
      y = [-63.932, 64.068]
      z = [-63.93, 64.07]
  Perfil uniform grid:  0.9
  Uniform grid Size [Å]:
      x = [-17.3225, 19.6775]
      y = [-21.932, 22.068]
      z = [-16.43, 16.57]
  Number of Subdivisions in the Uniform grid:  nx = 74  ny = 88  nz = 66
============================================
```

{: .note }
Key points:
- The domain box encloses the molecule with some padding — this avoids boundary artifacts.
- The uniform grid subdivisions indicate mesh resolution; more subdivisions mean higher accuracy but longer computations.

## Surface Generation (NanoShaper)

The software builds the Surface of your molecule using [**NanoShaper**](https://gitlab.iit.it/SDecherchi/nanoshaper). This surface represents the boundary between the molecule and the solvent.

### Example log
{: .text-delta }

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

##  Grid Construction

Here, each MPI process builds its part of the octree mesh, a hierarchical grid structure that adapts mesh density where needed.

### Example log
{: .text-delta }

```bash
============ [ Building Grid ] =============
  [Rank 0] Local nodes     : 121559
  [Rank 0] Local quadrants : 123118
  [Rank 1] Local nodes     : 121559
  ...
  [Global] Total nodes     : 473299
  [Global] Total quadrants : 492472
============================================
```

This gives you an idea of the total size and resolution of the final mesh.


## Solving the Poisson–Boltzmann Equation

This is the core computational step. The solver uses the LIS library to find the electrostatic potential by iterating until convergence.

### Example log
{: .text-delta }

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


## Energy results

After solving, the program prints energy values relevant to the electrostatics of your system:

```bash
================ [ Electrostatic Energy ] =================
  Net charge [e]:                                 7.771561172376096e-16
  Flux charge [e]:                                1.864835543054415e-10
  Polarization energy [kT]:                       -316.2410979131763
  Direct ionic energy [kT]:                       -0.3089150450692983
  Coulombic energy [kT]:                          -10097.24155852402
  Sum of electrostatic energy contributions [kT]: -10413.79157148227
===========================================================
```

---