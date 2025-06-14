---
layout: default
title: Input Files
nav_order: 4
---

# 📁 Input Files Guide

This guide explains the required and optional input files for running electrostatic simulations with **NextGenPB**, particularly those governed by the Poisson–Boltzmann equation (PBE).

---

## ✅ Protein Structure Files

### PQR Files (Recommended)
`*.pqr` files contain atomic coordinates, partial charges, and radii. They are ideal for electrostatics simulations because they include all required per-atom physical data in a single file.

### PDB Files
`*.pdb` (Protein Data Bank) files contain atomic positions and residue information but **do not include charges or radii**, which are necessary for electrostatics calculations. If you use `.pdb` files, you must supply:
- A radius file (`.siz`)
- A charge file (`.crg`)

<!-- 💡 **Tip:** : Convert `.pdb` to `.pqr` using tools like **PDB2PQR** to simplify simulation setup. -->

---

## ✅ Configuration Files — `options.prm`

The `options.prm` file defines all simulation settings, including the molecular input, mesh generation, physical parameters, and solver configuration.

Use this section to help you **write or modify your `.pot` files** for specific use cases.

---

### 📌 General Notes:
- Prefer `.pqr` files when possible — simpler and self-contained.
- Ensure relative paths in `options.prm` are correct with respect to where you run the program.

---

## 1. Input Settings

These settings specify how molecular structure files are loaded and interpreted.

| Parameter      | Description                                         | Values               | Default         |
|----------------|-----------------------------------------------------|-----------------------|-----------------|
| `filetype`     | Structure file type                                 | `pqr`, `pdb`          | `pqr`           |
| `filename`     | Path to the structure file                          | string (file path)    | `input.pqr`     |
| `radius_file`  | Radius file path (used only if `filetype = pdb`)    | string (file path)    | `radius.siz`    |
| `charge_file`  | Charge file path (used only if `filetype = pdb`)    | string (file path)    | `charge.crg`    |
| `write_pqr`    | Whether to write a processed `.pqr` file            | `0`, `1`              | `0` (disabled)  |
| `name_pqr`     | Name of the output `.pqr` file                      | string                | `output.pqr`    |

**Example**: add the following section to `options.prm`:

```bash
[input]
filetype = pqr
filename = path/to/structure.pqr
radius_file = path/to/radius.siz
charge_file = path/to/charge.crg
write_pqr = 0
name_pqr = output.pqr
[../]
```

## 2. Mesh Settings

This section defines how the computational grid (mesh) is generated around the biomolecule. The mesh influences accuracy, performance, and how physical properties are computed.

### Mesh Type Selection

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



**Example**: Configuration in .prm File

```bash
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

### 3. Electrostatics Model 

This section is devoted to the physical model, e.g. the linearized Poisson-Boltzmann equation, through the definition of the boundary conditions, the dielectric environment, as well as the choice of various energy calculations.

| Option                         | Description                              |
| ------------------------------ | -----------------------------------------|
|  linearized                    |  Picked to select the linearized PBE     |
|  bc_type                       |  Selects suitable boundary conditions    |
|  molecular_dielectric_constant |  Dielectric constant inside the molecule |
|  solvent_dielectric_constant   |  Dielectric constant of the solvent      |
|  ionic_strength                |  Ionic strength measured in mol/L        |
|  T (Temperature)               |  Temperature measured in Kelvin          |

* Since only the linearized PBE is taken into account in the solver, this variable becomes set to 1
* Boundary conditions can be of several types: 0 for Neumann (zero normal derivative), 1 for Dirichlet (fixed potential), 2 for Coulombic (analytical behaviour at the boundary)
* The dielectric constant inside the molecule is typically set equal to 2
* Since the solvent considered is mainly water, its dielectric constant is equal to 80
* The ionic strength is set to 0,145 mol/L
* The temperature is set to 298,15 K


#### Energy Options:

```bash
calc_energy = 2        # 1 = polarization, 2 = + ionic solvation
calc_coulombic = 1     # Include Coulombic energy calculation
```

#### Output Control:

```bash
atoms_write   = 0       # Write potential at atom centers
map_type      = vtu     # Format for visualization (vtu for ParaView)
potential_map = 0       # Export potential map
surf_write    = 0       # Export surface potential
```

**Example**: setting the electrostatic options:

```bash
linearized = 1                          # Linearized Poisson–Boltzmann equation
bc_type = 1                             # Boundary conditions set to Dirichlet (fixed potential)

molecular_dielectric_constant = 2       # Dielectric constant inside the molecule
solvent_dielectric_constant = 80        # Dielectric constant of the solvent (e.g., water)
ionic_strength = 0.145                  # Ionic strength (mol/L)
T = 298.15                              # Temperature in Kelvin

calc_energy = 2                         # To calculate polarization and ionic solvation energy
calc_coulombic = 1                      # No calculation of Coulombic energy
```

### 4. Surface Definition 

Here one finds the definition of how the boundary between solute and solvent is generated using NanoShaper, a tool aimed at computing the molecular surface and pockets of a biomolecular system.

#### Surface Types:
Dielectric boundaries are defined by means of surface types that can be specifically selected. 

| Option | Description                     |
| ------ | ------------------------------- |
|  `0`   |  Solvent Excluded Surface (SES) |
|  `1`   |  Skin Surface                   |
|  `2`   |  Blobby Surface                 |

Moreover the surface shape can be controlled through a specific numerical parameter:
* In the case of Solvent Exluded Surfaces such parameter is the probe radius, approximately 1,4 Å for water
* For skin or blobby surfaces instead it controls smoothness/blobbyness (-1.5)

#### Stern Layer
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

### 5. Solver and Algorithm 

In this last section, one may want to personalize the solver and various options like the solver type, the preconditioner and the tolerance.

* First consider the linear solver backend: two main options can be selected, a direct solver which is stable but much more memory demanding (linear_solver = 'mumps'), and an iterative solver which is far more recommended for large problems (linear_solver = 'lis').

#### Common LIS Solver Flags:
These control the solver type, preconditioner, tolerances, and the kind of print level.

| Option       | Description                  |
| ------------ | ---------------------------- |
| `-i`         | Sets the solver type         |
| `-p`         | Chooses the preconditioner   |
| `-tol`       | Sets a convergence tolerance |
| `-print`     | Output level                 |
| `-conv_cond` | Convergence condition type   |
| `-tol_w`     | Warning tolerance

* The options for the solver type can be cg, cgs, bicgstab, etc.
* The preconditioner can be set to ilu, ssor, jacobi, etc.
* The tolerance criteria can be fixed freely
* The verbosity, through `-print`, can be selected to be 0 if silent, 1 for final and 2 for verbose
* Convergence condition type allows to select which type of norm convergence to select


#### Example
One may use the SSOR preconditioning with CGS solver and selecting a strict tolerance with the command:
```bash
solver_options = -p\ ssor\ -ssor_omega\ 0.51\ -i\ cgs\ -tol\ 1.e-6\ -print\ 2\ -conv_cond\ 2\ -tol_w\ 0
```

### 6. New Developments

1. Nonlinear ionic density, going beyond the classical low-potential approximation. This linearization neglects important physical effects in systems with high surface charge, multivalent ions, or non-dilute ionic concentrations.
2. Allow more flexible formats for `.pqr` files (particularly with or without the ChainID column) or the use of `.pdb` files


### Troubleshooting

* Ensure file paths are correct relative to the `.pot` file.
* Use `.pqr` to avoid needing separate radius/charge files.
* Double-check that `mesh_shape` settings are consistent with grid parameters.

---

Feel free to copy and adapt this template into your own tutorial or documentation!

