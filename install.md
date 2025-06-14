---
layout: default
title: Installation
nav_order: 2
---

# 🛠️ Installation Guide for NextGenPB


Welcome to the installation guide for **NextGenPB**, a powerful tool for electrostatic simulations using advanced finite element techniques. 
This guide will walk you through the entire setup, whether you're using **macOS**, **Linux**, or **containerized environments** like Docker or Apptainer.

---

## 📦 Dependencies

NextGenPB depends on a combination of custom libraries, solvers, and visualization tools.

### 🔧 Core Libraries (Required to Compile and Run)

These libraries are mandatory to compile and execute NextGenPB:

| Library | Purpose |
|--------|---------|
| `lis` | Linear solver for sparse matrices |
| `p4est` | Adaptive mesh refinement using octrees |
| `bim++` | FEM library tailored for electrostatics |
| `NanoShaper` | Molecular surface generation |


### 🔍 Optional (But Recommended) Tools for Visualization

- **ParaView** – View simulation results in `.vtk` format.
- **GNU Octave** – Useful for analyzing `.octbin` files generated by NextGenPB.

---

## 🚀 Quick Start Options
###  🏃 Very Impatient? Use a Precompiled Apptainer Image

If you're eager to run simulations without compiling anything, use the ready-made `.sif` container image.

#### Prerequisite

- Install **Apptainer** (formerly Singularity):  
  [https://apptainer.org/docs/](https://apptainer.org/docs/)


#### ⬇️ Download the image

```bash
wget ....
```

➡️ Go to [Running NextGenPB](run.md) for execution details using the container.


### 🚶‍♂️ Prefer Customization? Build the Docker or Apptainer Image Yourself

This option is for advanced users who want to customize flags, link libraries differently, or tweak performance.

#### Build the Docker image (if root access available)

```bash
sudo docker build -f Dockerfile -t <name_of_image>:latest .
```
To run the Docker image:
```bash
sudo docker run -v "$(pwd)":/App -w /App <name_of_image>:latest mpirun -np <number_of_processors> ngpb --prmfile options.prm
```

#### Build the Apptainer image (if root access available)

```bash
sudo apptainer build NextGenPB_ompi4.sif recipe.def
```
To run the Apptainer image:
```bash
 apptainer exec --pwd /App --bind /path/to/files/:/App  /path/to/sif/NextGenPB_ompi4.sif mpirun -np <number_of_processors> ngpb --prmfile options.prm
```
➡️ See the [Running NextGenPB](run.md) guide for execution details using the container.


#### Customize compiling flags to performance

---

## 🍏 Installation on macOS (via MacPorts)

This section will guide you through installing and compiling NextGenPB from source using MacPorts, a package manager for macOS.

💡 **Tip:** Homebrew can be used too.

### ✅ Step 1: Install Core Dependencies

We’ll begin by installing necessary packages via MacPorts. This may take a while.

```bash
sudo port install openmpi cmake onetbb boost cgal5 nlohmann-json jansson octave
sudo port install mumps +openmpi -mpich
sudo port install lis +openmpi -mpich
sudo port install p4est +openmpi -mpich
```

-	`+openmpi` ensures compatibility with the MPI implementation NextGenPB expects.
-	`-mpich` avoids conflicts from having multiple MPI stacks.

### ✅ Step 2: Install External Libraries

These libraries are not managed by MacPorts and must be installed manually.


#### NanoShaper: Generate molecular surfaces

NanoShaper is required to compute molecular surfaces.

```bash
git clone https://gitlab.iit.it/SDecherchi/nanoshaper.git
cd nanoshaper
cp CMakeLists_so.txt CMakeLists.txt
cd build_lib
cmake .. -DCGAL_DIR=/opt/local/ -DCMAKE_BUILD_TYPE=Release
make
```

- CGAL is required for NanoShaper’s surface triangulation.
- The library will be used later by the main NextGenPB build.

#### octave_file_io: Required for .octbin handling

Needed to interface with .octbin files generated by GNU Octave.

```bash
git clone https://github.com/carlodefalco/octave_file_io.git
cd octave_file_io
./autogen.sh
mkdir build
cd build
../configure CXX=mpicxx --prefix=/opt/octave_file_io/1.0.91 --with-octave-home=/opt/local/bin 'LDFLAGS=-Wl,-rpath -Wl,/opt/local/lib/libgcc -Wl,-rpath -Wl,/opt/local/lib/gcc13 -ld_classic'
make
sudo make install
```

#### bim++ (NextGenPB branch)

Download and configure the custom FEM library:

```bash
wget https://github.com/carlodefalco/bimpp/archive/refs/tags/NextGenPB-v0.0.01.tar.gz
tar -xvzf NextGenPB-v0.0.01.tar.gz
cd bimpp-NextGenPB-v0.0.01
./autogen.sh
mkdir build && cd build
```

Configure build options carefully:

```bash
../configure --prefix=/opt/bimpp \
  LDFLAGS="-L/opt/local/lib -Wl,-rpath,/opt/local/lib/libgcc -Wl,-rpath,/opt/local/lib/gcc14" \
  CPPFLAGS="-I/opt/local/include/ -I/opt/local/include/gcc14 -DOMPI_SKIP_MPICXX -DHAVE_OCTAVE_44 -DBIM_TIMING" \
  --with-blas-lapack="-lopenblas" \
  --with-octave_file_io-home=/opt/octave_file_io/1.0.91 \
  --with-octave-home=/opt/local/bin \
  --with-p4est-home=/opt/local \
  --with-lis-home=/opt/local \
  --with-mumps-home=/opt/local \
  --with-mumps-extra-libs="-L/opt/local/lib -lptscotch -lscotch -lmpi -Wl,-flat_namespace -Wl,-commons,use_dylibs \
  -L/opt/local/lib/openmpi-mp -lmpi_usempif08 -lmpi_usempi_ignore_tkr -lmpi_mpifh -lopenblas -L/opt/local/lib/gcc14 -lgfortran" \
  F77=mpif90 CXX=mpicxx MPICC=mpicc CC=mpicc \
  CXXFLAGS="-std=c++17 -O3 -mtune=native -march=native"
```

Then compile and install:

```bash
make
sudo make install
```

### ✅ Step 3: Build NextGenPB

####  Prepare Build Settings

Copy the appropriate local_settings.mk file from the `local_setting/` directory:

```bash
cp local_setting/local_settings_mac.mk src/local_settings.mk
```

Edit this file as needed for macOS-specific paths and compiler flags.

####  Compile the executable

From the `src` directory:

```bash
make clean all
```
This will create the binary `ngpb` inside the `src` directory.

➡️ Once compiled, see [Running NextGenPB](run.md) for examples on how to run ngpb on your system or within a container.


### ✅  Step 4: (Optional) Add ngpb to Your Path

To call ngpb from any directory:

```bash
echo 'export PATH=/path/to/ngpb/src:$PATH' >> ~/.zshrc
source ~/.zshrc
```

---

## 🐧 Installing on Rocky Linux or Ubuntu

Use the provided Dockerfiles for a streamlined install:
- Dockerfile – for Rocky Linux
- Dockerfile_ubuntu – for Ubuntu

### Manual Build on Linux

To manually compile the code on Linux:

```bash
export CXXFLAGS="-O3 -mtune=native -march=native"
export CFLAGS="-O3 -mtune=native -march=native"
export FCFLAGS="-O3 -mtune=native -march=native"
```

Make sure all dependencies (`mumps`, `lis`, `p4est`, `bim++`) are compiled with the same compiler and flags to avoid incompatibility or runtime errors.

Then follow the same steps described in the **macOS build** section.

---

✅ You’re now ready to run simulations with **NextGenPB**!

➡️ Go to [running section](run.md) to learn out to run the solver.

