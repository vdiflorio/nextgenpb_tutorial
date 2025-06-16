---
layout: default
title: Input Files
nav_order: 4
---

# 📁 Input Files Guide

This guide explains the required and optional input files for running electrostatic simulations with **NextGenPB**, particularly those governed by the Poisson–Boltzmann equation (PBE).

---

## ✅ Molecular Structure Files

### PQR Files (Recommended)

Files with `.pqr` extension contain:
- Atomic coordinates  
- Partial charges  
- Atomic radii 

### PDB Files

Files with `.pdb` extension contain atomic coordinates and residue information **but lack charges and radii**. When using `.pdb` files, you must also provide:
- A **radius file** (`.siz`)
- A **charge file** (`.crg`)

💡 *Tip: Use tools like `PDB2PQR` to convert `.pdb` to `.pqr` for easier setup.*

---

## ✅ Configuration Files — `options.prm`

The main configuration file `options.prm` defines:
- Molecular structure input
- Mesh generation settings
- Physical parameters
- Solver and output options

### 📌 General Notes:
- Prefer `.pqr` files when possible — simpler and self-contained.
- Ensure relative paths in `options.prm` are correct with respect to where you run the program.
- An example of parameter file with all options described is [here](https://github.com/vdiflorio/NextGenPB/tree/main/data).

---



---

## 1️⃣ Molecular Input Settings

This section defines how the molecular structure is loaded and interpreted.

| Parameter      | Description                                         | Values               | Default         |
|----------------|-----------------------------------------------------|-----------------------|-----------------|
| `filetype`     | Structure file type                                 | `pqr`, `pdb`          | `pqr`           |
| `filename`     | Path to the structure file                          | string (file path)    | `input.pqr`     |
| `radius_file`  | Radius file path (used only if `filetype = pdb`)    | string (file path)    | `radius.siz`    |
| `charge_file`  | Charge file path (used only if `filetype = pdb`)    | string (file path)    | `charge.crg`    |
| `write_pqr`    | Whether to write a processed `.pqr` file            | `0`, `1`              | `0` (disabled)  |
| `name_pqr`     | Name of the output `.pqr` file                      | string                | `output.pqr`    |

**Example block** in `options.prm`:

```ini
[input]
filetype = pqr
filename = path/to/structure.pqr
radius_file = path/to/radius.siz
charge_file = path/to/charge.crg
write_pqr = 0
name_pqr = output.pqr
[../]
```

## 2️⃣ Mesh Generation Settings

Defines how the computational grid (mesh) is generated around the biomolecule. The mesh influences accuracy, performance, and how physical properties are computed.

### Mesh Type (mesh_shape)

The shape and structure of the computational grid are controlled using the `mesh_shape` parameter:

| Parameter     | Description                          | Values         | Default |
|---------------|--------------------------------------|----------------|---------|
| `mesh_shape`  | Mesh shape configuration             | `0`, `1`, `2`, `3` | `0`     |

**Available Options for `mesh_shape`:**

| Value | Description                                                                 |
|-------|-----------------------------------------------------------------------------|
| `0`   | **Derefined mesh** – uses different resolutions (`perfil1`, `perfil2`) in coarse and fine regions. |
| `1`   | **Uniform cubic mesh** – all elements have the same resolution (`perfil1`). |
| `2`   | **Manual bounding box** – define exact mesh limits via `x1`, `x2`, etc.     |
| `3`   | **Focused mesh** – refinement around a specified region center (`cx_foc`, etc.). |


### Grid Parameters

These parameters control how the mesh is built, determining both the resolution (i.e., how fine or coarse the mesh is) and whether the center of the mesh should be shifted randomly — useful in stochastic simulations where you might want to slightly vary the mesh to test robustness.


| Parameter      | Description                                                | Values          | Default         |
|----------------|------------------------------------------------------------|-----------------|-----------------|
| `perfil1`      | Sets the main grid spacing: smaller values create finer meshes | float           | `0.8`           |
| `perfil2`      | Defines a finer spacing used only in specific refined regions  | float           | `0.2`           |
| `scale`        | Refinement scale factor (used in `mesh_shape = 0 or 3`)    | float           | `2`             |
| `rand_center`  | Randomly shift the mesh center (only for shape `0` or `1`) | `0`, `1`        | `0` (disabled)  |




### Focused Mesh Parameters (for `mesh_shape = 3`)

To concentrate mesh resolution in a specific region (e.g., near a binding site), use:


| Parameter  | Description                                  | Values     | Default |
|------------|----------------------------------------------|------------|---------|
| `cx_foc`   | X coordinate of focused region center        | float      | `0.0`   |
| `cy_foc`   | Y coordinate of focused region center        | float      | `0.0`   |
| `cz_foc`   | Z coordinate of focused region center        | float      | `0.0`   |
| `n_grid`   | Number of grid intervals in the focused zone | integer    | `10`    |



### Uniform Refinement Settings (for `mesh_shape = 1` or `2`)

These parameters define mesh resolution globally (`unilevel`) or in regions outside the refine box (`outlevel`):


| Parameter    | Description                                           | Values     | Default |
|--------------|-------------------------------------------------------|------------|---------|
| `unilevel`   | Uniform refinement level (refines `2^unilevel` times) | integer    | `6`     |
| `outlevel`   | Minimum refinement level outside the refine box       | integer    | `1`     |



### Manual Bounding Box (for `mesh_shape = 2`)

If you want full control over the mesh boundaries, define them explicitly:

```bash
x1 = -16  
x2 = 16
y1 = -16  
y2 = 16
z1 = -16  
z2 = 16
```

These define the physical domain in each coordinate direction.

### Optional: Refinement Box (for mesh_shape = 1 or 2)

You can locally refine a portion of the domain by enabling refine_box. This is helpful for focusing resolution in a region of interest (e.g., protein binding site):

| Parameter           | Description                          | Values       | Default         |
|---------------------|--------------------------------------|--------------|-----------------|
| `refine_box`        | Enable box refinement                | `0`, `1`     | `0` (disabled)  |
| `outrefine_x1`      | X lower bound of refinement box      | real numbers | `-4.0`          |
| `outrefine_x2`      | X upper bounds of refinement box     | real numbers | `4.0`           |
| `outrefine_y1`      | Y lower bounds of refinement box     | real numbers | `-4.0`          |
| `outrefine_y2`      | Y upper bounds of refinement box     | real numbers | `4.0`           |
| `outrefine_z1`      | Z lower bounds of refinement box     | real numbers | `-4.0`          |
| `outrefine_z2`      | Z upper bounds of refinement box     | real numbers | `4.0`           |



**Example block** in `options.prm`:

```ini
[mesh]
# Mesh type: 0=derefined, 1=uniform, 2=manual box, 3=focused
mesh_shape = 0

# Grid spacing (perfil1 = base, perfil2 = fine zones)
perfil1 = 0.8
perfil2 = 0.5
scale   = 2.0

# Optional: randomly shift mesh center (useful for ensemble runs)
rand_center = 0

# Uniform refinement (used if mesh_shape = 1 or 2)
unilevel  = 6
outlevel  = 1

# Bounding box (only if mesh_shape = 2)
x1 = -16
x2 = 16
y1 = -16
y2 = 16
z1 = -16
z2 = 16

# Optional: enable refinement box
refine_box = 0
outrefine_x1 = -4.0
outrefine_x2 =  4.0
outrefine_y1 = -4.0
outrefine_y2 =  4.0
outrefine_z1 = -4.0
outrefine_z2 =  4.0
[../]
```

## 3️⃣ Electrostatics Model Settings

This section is devoted to the physical model, e.g. the linearized Poisson-Boltzmann equation, through the definition of the boundary conditions, the dielectric environment, as well as the choice of various energy calculations.

### 📌 Note:
Since only the linearized PBE is taken into account in the solver, parameter `linearized` can only assume the value 1. With further developments newer versions may allow to switch to the resolution of the full non-linear version of the equation.

* Boundary conditions can be adjusted depending on the specific problem the user may want to solve. This allows to consider Neumann conditions (zero normal derivative), Dirichlet (fixed potential), or even Coulombic-like conditions (regarding the analytical behaviour on the boundary), by appropriately switching values for the parameter `bc_type` as showed below:

| Values |  Boundary condition   |
|--------|---------------------- |
|   `0`  |  Neumann              |
|   `1`  |  Dirichlet            |
|   `2`  |  Coulombic            |


* The dielectric constant inside the molecule is controlled through a specific parameter `molecular_dielectric_constant` while for the solvent, which is water most of the times, it is possible to adjust `solvent_dielectric_constant`.
* The ionic strength, the measure of the concentration of ions in the solution, can be set up by means of parameter `ionic_strength` and it is measured in `mol/L`. 
* The temperature `T` is assumed to be equal to the environmental temperature 298,15 K.


### Energy Options:
It is also possible to choice which kind of energy to calculate depending on specific interests and purposes. This can be performed by suitablly setting parameter `calc_energy`:

| Values |  Description                                       |
|--------|--------------------------------------------------- |
|   `0`  | No energy calculation                              |
|   `1`  | Calculates the polaritazion energy                 |
|   `2`  | Calculates polarization and ionic solvation energy |

While through `calc_coulombic` set to 1 is is possible to neglect the calculation of the Coulombic energy.

**Example**: Output Control

```ini
atoms_write   = 0       # Write potential at atom centers
map_type      = vtu     # Format for visualization (vtu for ParaView)
potential_map = 0       # Export potential map
surf_write    = 0       # Export surface potential
```

**Example**: setting the electrostatic options:

```ini
linearized = 1                          # Linearized Poisson–Boltzmann equation
bc_type = 1                             # Boundary conditions set to Dirichlet (fixed potential)

molecular_dielectric_constant = 2       # Dielectric constant inside the molecule
solvent_dielectric_constant = 80        # Dielectric constant of the solvent (e.g., water)
ionic_strength = 0.145                  # Ionic strength (mol/L)
T = 298.15                              # Temperature in Kelvin

calc_energy = 2                         # To calculate polarization and ionic solvation energy
calc_coulombic = 1                      # No calculation of Coulombic energy
```

## 4️⃣ Surface Definition 

Here one finds the definition of how the boundary between solute and solvent is generated using NanoShaper, a tool aimed at computing the molecular surface and pockets of a biomolecular system.

### Surface Types:
Dielectric boundaries are defined by means of surface types that can be specifically selected through a parameter `surface_type` that can assume three different values as follows:

| Values |  Description                    |
| ------ | ------------------------------- |
|  `0`   |  Solvent Excluded Surface (SES) |
|  `1`   |  Skin Surface                   |
|  `2`   |  Blobby Surface                 |

Moreover the surface shape can be controlled through a specific numerical parameter (`surface_parameter`):
* In the case of Solvent Exluded Surfaces such parameter is the probe radius, approximately 1,4 Å for water
* For skin or blobby surfaces instead it controls smoothness/blobbyness (-1.5)

### Stern Layer
It is possible to choose to include a Stern layer (an electric double layer) by setting `stern_layer_surf = 1` or equal to `0` otherwise. 
It is also possible to choose its thickness (in Å) by imposing, for example, `stern_layer_thickness = 2.0`. 

**Example**: setting some surface options:

```bash
surface_type = 0              # Select the Solvent Excluded Surface (SES)
surface_parameter = 1.4   

stern_layer_surf = 0          # No Stern layer
stern_layer_thickness = 2.0   # Thickness of Stern layer (in Å)   

number_of_threads = 1         # Number of CPU threads for NanoShaper
```

## 5️⃣ Solver and Algorithm 

In this last section, one may want to personalize the solver and various options like the solver type, the preconditioner and the tolerance.

* First consider the linear solver backend: two main options can be selected, a direct solver which is stable but much more memory demanding (linear_solver = 'mumps'), and an iterative solver which is far more recommended for large problems (linear_solver = 'lis').

### Common LIS Solver Flags:
These control the solver type, preconditioner, tolerances, and the kind of print level.

| Parameter    | Description                   |  Values                  |
| ------------ | ----------------------------- | -------------------------|
| `-i`         |  Sets the solver type         | `cg`, `cgs`, `bicgstab`  |
| `-p`         |  Chooses the preconditioner   |  `ilu`, `ssor`, `jacobi` |
| `-tol`       |  Sets a convergence tolerance |  real number             |
| `-print`     |  Output level                 |  `0`,`1`,`2`             |
| `-conv_cond` |  Convergence condition type   |  `2` (default)           |
| `-tol_w`     |  Warning tolerance            |  real number             |


* The tolerance criteria can be fixed freely
* The verbosity, through `-print`, can be selected to be 0 if silent, 1 for final and 2 for verbose
* Convergence condition type allows to select which type of norm convergence to select


**Example**:
One may use the SSOR preconditioning with CGS solver and selecting a strict tolerance with the command:
```bash
solver_options = -p\ ssor\ -ssor_omega\ 0.51\ -i\ cgs\ -tol\ 1.e-6\ -print\ 2\ -conv_cond\ 2\ -tol_w\ 0
```





