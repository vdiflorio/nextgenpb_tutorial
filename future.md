---
layout: default
title: Future Projects
nav_order: 3
nav_enabled: true
---

# Future Projects

This page outlines several research and development directions planned for the NextGenPB software. These projects aim to extend its physical accuracy, usability, and applicability.

---
## NextGenPB Web Server

**Goal:** Develop a **web-based interface** for submitting jobs and visualizing results directly in the browser.

- Designed for users without local installation or HPC access.
- Simplifies workflows for non-expert users in biology, chemistry, and nanotechnology.
- Inspired by the [NanoShaper Web Server](https://nanoshaperweb.iit.it/) for surface generation, which will be integrated as a backend service for SES computation.

---

## MM-PBSA Integration

**Goal:** Compute the **full solvation free energy**, including both electrostatic and non-polar components.

- Enables direct integration with molecular dynamics pipelines (e.g., GROMACS or AMBER).
- Facilitates binding free energy estimation for biomolecular complexes.

---

## Electrostatic Force Calculations

**Goal:** Derive **electrostatic forces** acting on atoms directly from the NGPB solution.

- Useful for system relaxation and force-based refinement.
- Opens the path for hybrid MD–continuum simulations.

---

## Zeta Potential Estimation

**Goal:** Estimate **zeta potential** from surface electrostatics in ionic environments.

- Important for colloidal stability analysis.
- Relevant for nanoparticle functionalization and biosensing applications.

---

## Nonlinear Poisson–Boltzmann Solver

**Goal:** Accurately solve the **nonlinear PBE**, going beyond the classical low-potential approximation.

- Linearization neglects essential physical effects:
  - High surface charge density
  - Multivalent ions
  - Moderately concentrated electrolytes
- Nonlinear models allow:
  - More realistic ionic density distributions
  - Better agreement with experiments in strong coupling regimes

---