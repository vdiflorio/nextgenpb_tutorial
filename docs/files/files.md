--- 
title: PDB/PQR
parent: Input Files
nav_order: 4
---


# Molecular Structure Files

Understanding the input formats is crucial for preparing molecular systems correctly. This section introduces the supported file types and how to use them in your simulations.

---

## PQR Files (Recommended)

Files with the `.pqr` extension are the preferred input format. They **combine**:

- Atomic **coordinates**
- Atomic **partial charges**
- Atomic **radii**

This makes them ideal for simulations, as all necessary physical properties are included in a single file.

**Example:** You can find example `.pqr` files in this [GitHub repository](https://github.com/vdiflorio/NextGenPB/tree/main/data).

{: .note }
You can easily convert `.pdb` files into `.pqr` using tools like [`PDB2PQR`](https://server.poissonboltzmann.org/pdb2pqr).

---

## PDB Files

Files with the `.pdb` extension include:

- Atomic **coordinates**
- **Residue** and **chain** information

However, they **do not include**:

- Partial charges  
- Atomic radii 

### Additional Files Required

To use `.pdb` files in your simulation, you must also supply:

- A **radius file**: `.siz`  
- A **charge file**: `.crg`

These files provide the missing physical properties needed for electrostatic simulation.

**Example:** Example `.pdb`, `.siz`, and `.crg` files can be found [here](https://github.com/vdiflorio/NextGenPB/tree/main/data).

{: .note }
Using `.pdb` files gives you **more flexibility**. You can define **custom radii and charges** in your `.siz` and `.crg` files, which is especially useful if your molecule contains **non-standard residues or atoms**. You can even assign specific values to selected residues or atoms.

---

## Summary Table

| File Type | Coordinates | Charges | Radii | Ready-to-Use | Customization |
|-----------|-------------|---------|-------|---------------|----------------|
| `.pqr`    | ✅          | ✅      | ✅    | ✅            | Limited        |
| `.pdb`    | ✅          | ❌      | ❌    | ❌ (extra files needed) | ✅ (via `.siz`/`.crg`) |

---
