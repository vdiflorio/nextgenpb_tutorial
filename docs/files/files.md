--- 
title: PDB/PQR
parent: Input Files
nav_order: 1
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

>💡 **Tip:** Use tools like `PDB2PQR` to convert `.pdb` to `.pqr` for easier setup.

---
