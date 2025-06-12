---
layout: default
title: Input Files
nav_order: 4
---

# 📁 Input Files Guide


## ✅ Protein Files 

### PQR files
A molecular structure file with atom coordinates, radii, and partial charges. [...?]

### PDB files
Protein Data Bank files are slightly different and contain atomic coordinates, atom names, residues and informations regarding the biomolecule's connectivity. Unfortunately they lack of crucial information when comes to electrostatic simulations, requiring a conversion to PQR files by means of tools like PDB2PQR.


## ✅ Configuration Files
### `options.prm`
Contains simulation parameters as well as the definition of how interactions get calculated. [...?]
Example content:

# ```text
grid_spacing = 0.5
boundary_condition = focusing
ionic_strength = 0.15
temperature = 298
output_format = vtk

### .vtk files?

### `options.pot` [...?]
1. Input Settings
2. Mesh Settings
3. Electrostatics Model Settings
4. Surface Definition (NanoShaper)
5. Solver and Algorithm Settings
6. 

