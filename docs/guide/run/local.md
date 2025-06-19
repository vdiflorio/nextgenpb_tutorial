---
title: Compiled Version
parent: Run
nav_order: 3
---

# Running Locally (Compiled Version)

If you’ve built NextGenPB from source and added the executable to your system `PATH`, you can launch it directly from your terminal.

Here are a couple of examples:

🔹 **Without a .pqr file:**
```bash
mpirun -np 4 ngpb --prmfile options.prm
```

🔹 **With a .pqr file:**
```bash
mpirun -np 4 ngpb --prmfile options.prm --pqrfile 1CCM.pqr
```

{: .note }
You can adjust the number of processors to suit your system’s capabilities. Even on a laptop, running with `-np 1` is perfectly valid.


---
⬅️ Return to the [Installation Guide](/nextgenpb_tutorial/docs/guide/installation/linux) if you haven’t installed the solver yet.

➡️ Go to [input files](/nextgenpb_tutorial/docs/guide/files/files) to learn everything about `.prm`, `.pot`, `.pqr`, and other input formats.