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

| Parameter       | Description                                         | Default             |
|----------------|-----------------------------------------------------|---------------------|
| `filetype`     | Type of structure file: `pqr` or `pdb`              | `pqr`               |
| `filename`     | Path to the structure file                          | `input.pqr`         |
| `radius_file`  | Path to radius file (used only if `filetype = pdb`) | `radius.siz`        |
| `charge_file`  | Path to charge file (used only if `filetype = pdb`) | `charge.crg`        |
| `write_pqr`    | Whether to write processed `.pqr` file              | `0` (disabled)      |
| `name_pqr`     | Output `.pqr` file name                             | `output.pqr`        |

```ini
filetype = pqr
filename = path/to/structure.pqr
radius_file = radius.siz
charge_file = charge.crg
write_pqr = 0
name_pqr = output.pqr
```



## 2. Mesh Settings

This section defines how the computational grid (mesh) is built around the biomolecule.

### Mesh Shape Options  (mesh_shape)
The first step is devoted to the choice of the shape of the mesh as described in the table below:

| Option   | Description                            |
| -------- | -------------------------------------- |
|   0    |    Derefined mesh (based on perfil1/perfil2)   |
|   1    |    Uniform cubic mesh    |
|   2    |    Manual bounding box    |
|   3    |    Focused mesh (local refinement) |

**Default**: 0


### Grid parameters

| Parameter       | Description                                         | Default             |
|----------------|-----------------------------------------------------|---------------------|
| `perfil1`     | Base mesh spacing/resolution                | `0.8`               |
| `perfil2`     | Finer resolution in derefined regions       | `0.2`         |
| `scale`        | Refinement scale factor                    | `2`       |
| `rand_center`  | Randomly shift center (for mesh_shape 0 or 1) | `0` `(disabled)`        |


### Focused Mesh (if `mesh_shape = 3`)

| Parameter       | Description                                         | Default             |
|----------------|-----------------------------------------------------|---------------------|
| `cx_foc`     | X coordinate of focused region center                 | `0.0`               |
| `cy_foc`     | Y coordinate of focused region center                 | `0.0`         |
| `cz_foc`     | Z coordinate of focused region center                 | `0.0`       |
| `n_grid`  | Number of focused intervals | `10`        |
```ini
cx_foc = 0   # X-center coordinate
cy_foc = 0   # Y-center coordinate
cz_foc = 0   # Z-center coordinate
n_grid = 10  # Number of 1/scale-length intervals in the focused zone
```


### Refinement (if `mesh_shape = 1 or 2`)
Whenever `mesh_shape` is equal to 1 or 2, it is possible to set a uniform refinement level across the mesh (2^unilevel elements per side) or a minimum refinement level outside the refine box through the variables 'unilevel' and 'outlevel' respectively:

| Parameter       | Description                                         | Default             |
|----------------|-----------------------------------------------------|---------------------|
| `unilevel`     | Uniform refinement level                 | `6`               |
| `outlevel`     | Minimum refinement level outside box.    | `1`         |



#### Bounding Box Parameters
Whenever 'mesh_shape' is equal to 2, it is possible to create a bounding box by defining the physical domain directly:

```ini
x1 = -16  x2 = 16
y1 = -16  y2 = 16
z1 = -16  z2 = 16
```

#### Refinement Box 
When 'mesh_shape' is equal to 1 or 2 and 'refine_box' = 1, one may refine the mesh within a subregion:

```ini
refine_box = 0   # 1 = enable inner refinement box, 0 = disable
refine_x1 = -4.0   
refine_x2 = 4.0
refine_y1 = -4.0   
refine_y2 = 4.0
refine_z1 = -4.0   
refine_z2 = 4.0
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

```ini
calc_energy = 2        # 1 = polarization, 2 = + ionic solvation
calc_coulombic = 1     # Include Coulombic energy calculation
```

#### Output Control:

```ini
atoms_write   = 0       # Write potential at atom centers
map_type      = vtu     # Format for visualization (vtu for ParaView)
potential_map = 0       # Export potential map
surf_write    = 0       # Export surface potential
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

