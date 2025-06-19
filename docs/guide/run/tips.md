---
title: Best Practices
parent: Run
nav_order: 4
---

# Tips and Best Practices

- Always check the `.log` file after each run. It contains essential diagnostic information and any warning messages from the solver.
- Redirect your solver’s output to a log file for easier review:
```bash
mpirun -np 4 ngpb --prmfile options.prm > run.log 2>&1
```
- For heavy simulations, make sure your system has enough memory and CPU resources for the number of MPI processes you’re using.

---
