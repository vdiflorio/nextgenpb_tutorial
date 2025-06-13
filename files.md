---
layout: default
title: Input Files
nav_order: 4
---

# 📁 Input Files Guide


## ✅ Protein Files 

### PQR files
A molecular structure file with atom coordinates, radii, and partial charges. 

### PDB files
Protein Data Bank files are slightly different and contain atomic coordinates, atom names, residues and informations regarding the biomolecule's connectivity. Unfortunately they lack of crucial information when comes to electrostatic simulations, requiring a conversion to PQR files by means of tools like PDB2PQR.

---

## ✅ Configuration Files - PBE Solver Configuration Tutorial

This tutorial explains the structure and purpose of each section contained in the `options.pot` configuration file used to run electrostatics simulations by means of the Poisson–Boltzmann equation. It contains simulation parameters as well as the definition of how interactions get calculated, but you can use this as a reference to write or modify your own `.pot` files.

### Notes:
* Prefer `.pqr` format for simplicity, it includes all necessary atom data.
* If using `.pdb`, you must provide radius and charge files separately.


```text 
grid_spacing = 0.5
boundary_condition = focusing
ionic_strength = 0.15
temperature = 298
output_format = vtk
```  
# a questo punto possiamo togliere questa sezione di esempi qui sopra?


### Introduction

The `.pot` file contains the full set of instructions needed by the solver to:

* Load and interpret the molecular structure
* Generate the computational grid (mesh)
* Define the electrostatic model and boundary conditions
* Configure how the dielectric boundary surface is built
* Choose and control the numerical solver used

### 1. Input Settings 

Specify molecular input data such as the file format and associated information.

#### Key Parameters:

```ini
filetype = pqr                     # Choose between 'pqr' (recommended) and 'pdb'.
filename = path/to/structure.pqr   # Main input file containing atoms, radii, and charges.
radius_file = radius.siz           # Required only for 'pdb' input type, containing the radii for all atoms in the 'pdb' file
charge_file = charge.crg           # Required only for 'pdb' input type, containing the charges for all atoms in the 'pdb' file
write_pqr = 0                      # Set to 1 to write the processed structure to disk, 0 otherwise
name_pqr = output.pqr              # Output file name if write_pqr is set to 1.
```


### 2. Mesh Settings

Define the computational mesh that discretizes the 3D space around the molecule.

#### Mesh Shape Options 
The value assigned to the variable 'mesh_shape' controls the way the grid is generated:
```ini
0 = Derefined mesh based on perfil1/perfil2
1 = Uniform cubic mesh
2 = Manual bounding box
3 = Focused mesh (local refinement)
```

For the grid spacing control it is useful to handle the following parameters:

```ini
perfil1 = 0.8         # Base grid spacing/resolution
perfil2 = 0.5         # Finer resolution in derefined zones (if active)
scale   = 2.0         # Used in modes 0 and 3 to set grid density
rand_center = 0       # Optional: Randomly shift mesh center, typically useful for for stochastic sampling (only for mesh_shape=0,1)
```

#### Focused Mesh 
When 'mesh_shape = 3' it is possible to define the center and number of grid intervals in the focused region:

```ini
cx_foc = 0   # X-center coordinate
cy_foc = 0   # Y-center coordinate
cz_foc = 0   # Z-center coordinate
n_grid = 10  # Number of 1/scale-length intervals in the focused zone
```


#### Refinement
Whenever 'mesh_shape' is equal to 1 or 2, it is possible to set a uniform refinement level across the mesh (2^unilevel elements per side) or a minimum refinement level outside the refine box through the variables 'unilevel' and 'outlevel' respectively:

```ini
unilevel  = 6     # Uniform refinement 
outlevel  = 1     # Mininum refinement  
```


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

Define the electrostatic equation model, dielectric environment, and energy calculations.

#### Main Settings:

```ini
linearized = 1                  # 1 = Linearized PBE; 0 = Nonlinear
bc_type = 1                     # 0 = Neumann, 1 = Dirichlet, 2 = Coulombic
molecular_dielectric_constant = 2
solvent_dielectric_constant = 80
ionic_strength = 0.145          # mol/L
T = 298.15                      # Temperature in K
```

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

Define how the boundary between solute and solvent is generated using NanoShaper.

#### Surface Types:

```ini
surface_type = 0         # 0 = SES, 1 = Skin, 2 = Blobby
surface_parameter = 1.4  # Probe radius or smoothness parameter
```

#### Stern Layer:

```ini
stern_layer_surf = 0        # Include Stern layer?
stern_layer_thickness = 2.0 # In Å
```

#### Performance:

```ini
number_of_threads = 1  # Set based on your CPU
```

### 5. Solver and Algorithm 

Specify the numerical solver and its configuration.

#### Example:

```ini
linear_solver = lis
solver_options = -p\ ssor\ -ssor_omega\ 0.51\ -i\ cgs\ -tol\ 1.e-6\ -print\ 2\ -conv_cond\ 2\ -tol_w\ 0
```

#### Common LIS Solver Flags:

| Option   | Description                            |
| -------- | -------------------------------------- |
| `-i`     | Solver type (e.g., `cg`, `cgs`)        |
| `-p`     | Preconditioner (`ilu`, `ssor`, etc.)   |
| `-tol`   | Convergence tolerance                  |
| `-print` | Output level (0 = silent, 2 = verbose) |


### 6. New Developments

1. Nonlinear ionic density, going beyond the classical low-potential approximation. This linearization neglects important physical effects in systems with high surface charge, multivalent ions, or non-dilute ionic concentrations.
2. Allow more flexible formats for `.pqr` files (particularly with or without the ChainID column) or the use of `.pdb` files


### Troubleshooting

* Ensure file paths are correct relative to the `.pot` file.
* Use `.pqr` to avoid needing separate radius/charge files.
* Double-check that `mesh_shape` settings are consistent with grid parameters.

---

Feel free to copy and adapt this template into your own tutorial or documentation!

