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

Defines the physical model used to compute the electrostatic potential, including the Poisson–Boltzmann equation configuration, boundary conditions, dielectric constants, ionic strength, and energy calculation options.

### Poisson–Boltzmann Equation (linearized)

Currently, **only the linearized Poisson–Boltzmann equation** is implemented. Set the following parameter accordingly:

| Parameter           | Description                          | Values       | Default         |
|---------------------|--------------------------------------|--------------|-----------------|
| `linearized`        | Enable linearized PBE (mandatory for now) | `1`     | `1`   |

⚠️ **Note**: The solver only supports the linearized version. Future versions may add support for the full nonlinear PBE.

### Boundary Conditions (bc_type)

You can choose among different boundary conditions to better represent the physical environment of your problem. This affects how the potential behaves at the domain boundaries.

| Parameter           | Description                          | Values       | Default         |
|---------------------|--------------------------------------|--------------|-----------------|
| `bc_type`        | Type of boundary condition | `0`, `1`, `2`     | `1`   |

#### Available Options for `bc_type`:

| Values |  Boundary condition   |
|--------|---------------------- |
|   `0`  |  Neumann – zero normal derivative              |
|   `1`  |  Dirichlet  - zero potential           |
|   `2`  |  Coulombic – analytical Debye-Hückel decay at boundary       |


### Dielectric Environment

Set the dielectric constants for both the solute (molecule) and solvent (usually water). These values affect how the electric field propagates through different regions.

| Parameter           | Description                          | Values       | Default         |
|---------------------|--------------------------------------|--------------|-----------------|
| `molecular_dielectric_constant`      | Dielectric constant inside the molecule     | real numbers | `2`          |
| `solvent_dielectric_constant`      | Dielectric constant of the solvent      | real numbers | `80`           |

### Dielectric Environment

Set the dielectric constants for both the solute (molecule) and solvent (usually water). These values affect how the electric field propagates through different regions.

| Parameter           | Description                          | Units       | Default         |
|---------------------|--------------------------------------|--------------|-----------------|
| `ionic_strength`      | Concentration of ions in the solvent     | mol/L | `0.145`          |
| `T`      | Temperature of the system      | Kelvin | `298.15`           |



### Energy Calculation Options

Choose what type of energy the solver should compute based on your goals (e.g. estimating solvation effects, computing binding energies, etc.).

| Parameter           | Description                          | Values       | Default         |
|---------------------|--------------------------------------|--------------|-----------------|
| `calc_energy`      | Energy to calculate     | `0`, `1`, `2`     | `2`   |
| `calc_coulombic`      | Whether to compute Coulombic energy (1 = yes)      | `0`, `1`     | `0`   |

#### Available Options for `calc_energy`:

| Values |  Description                                       |
|--------|--------------------------------------------------- |
|   `0`  | No energy calculation                              |
|   `1`  | Calculates the polaritazion energy                 |
|   `2`  | Calculates polarization and ionic solvation energy |






### Output Options (What gets saved after the simulation)

These parameters allow you to customize which data is written to disk and how it is formatted.

#### Available Parameters

| Parameter       | Description                                                                 | Values                 | Default |
|-----------------|-----------------------------------------------------------------------------|------------------------|---------|
| `atoms_write`   | Write electrostatic potential at atomic positions (e.g., atom centers)      | `0` = no<br>`1` = yes  | `0`     |
| `map_type`      | File format for exporting potential maps or fields                          | `vtu`, `vtk`, etc.     | `vtu`   |
| `potential_map` | Export a 3D potential map over the entire computational grid                | `0` = no<br>`1` = yes  | `0`     |
| `surf_write`    | Export potential on the molecular surface (e.g., SES or SAS)                | `0` = no<br>`1` = yes  | `0`     |

---

#### 📘 Detailed Descriptions

##### `atoms_write = 1`
Writes the electrostatic potential at each atomic center (e.g., where the atoms' charges are located).  
Useful for:
- Analyzing energy contributions at specific atoms
- Comparing potentials across charged and polar residues
- Visual inspection of potential at reactive sites

##### `map_type = vtu`
Sets the output format for the volumetric and surface data:
- `vtu`: XML-based VTK format (recommended, readable by ParaView and VTK)
- `vtk`: Legacy format, still compatible with many tools

##### `potential_map = 1`
Exports a volumetric scalar field of the electrostatic potential across the mesh.  
Useful for:
- Visualizing potential gradients in space
- Detecting electrostatic "hot spots"
- Quantitative field analysis in 3D

##### `surf_write = 1`
Exports potential sampled on the molecular surface (e.g., solvent-excluded surface, SES).  
Useful for:
- Identifying surface charge patches
- Guiding docking or drug design
- Comparing surface potential across different conformations

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

atoms_write   = 1       # Write potential at atom centers
map_type      = vtu     # Format: VTU (compatible with ParaView)
potential_map = 1       # Export full volumetric potential map
surf_write    = 1       # Export potential on the molecular surface
```

## 4️⃣ Surface Definition 

This section defines **how the boundary between solute and solvent** is generated using [**NanoShaper**](https://gitlab.iit.it/SDecherchi/nanoshaper/), a tool for computing molecular surfaces and identifying cavities or pockets.

The molecular surface plays a critical role in determining how the dielectric constant changes across space and where electrostatic discontinuities occur, which directly impacts the accuracy of the Poisson–Boltzmann solver.

---

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





