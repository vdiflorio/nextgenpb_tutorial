---
title: Linux
parent: Installation
nav_order: 4
---

# Installing on Rocky Linux or Ubuntu

Use the provided Dockerfiles for a streamlined install:
- Dockerfile – for Rocky Linux
- Dockerfile_ubuntu – for Ubuntu

## Manual Build on Linux

To manually compile the code on Linux:

```bash
export CXXFLAGS="-O3 -mtune=native -march=native"
export CFLAGS="-O3 -mtune=native -march=native"
export FCFLAGS="-O3 -mtune=native -march=native"
```

Make sure all dependencies (`mumps`, `lis`, `p4est`, `bim++`) are compiled with the same compiler and flags to avoid incompatibility or runtime errors.

Then follow the same steps described in the [**macOS build**](mac) section.

---

✅ You’re now ready to run simulations with **NextGenPB**!

Go to [running section](/nextgenpb_tutorial/docs/guide/run/) to learn out to run the solver.

