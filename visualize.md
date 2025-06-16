---
layout: default
title: Ouputs
nav_order: 5
---

## Log output

```ini
mpirun -np 4 /Users/vincenzo/Documents/PhD/PBE/LPBe/NextGenPB/src/ngpb --potfile options.pot --pqrfile 2CCH.pqr
Selected parameters file: options.pot
Selected pqr file:        2CCH.pqr

========== [ System Information ] ==========
  Number of atoms    : 4194
  Size protein [Å]   : [63.03, 7.915, -1.275]
  Net charge         : -3.000000e+00
  Solvent epsilon    : 2
  Solvent epsilon    : 80
  Temperature        : 298.15 [K] 
  Ionic strength     : 0.145 [mol/L] 
============================================

========== [ Domain Information ] ==========
  Scale:  2
  Center of the System [Å]:  [0.569, -2.2585, -0.1815]
  Perfil outer box:  0.2
  Complete Domain Box Size [Å]:
      x = [-255.431, 256.569]
      y = [-258.259, 253.742]
      z = [-256.182, 255.819]
  Perfil uniform grid:  0.9
  Uniform grid Size [Å]:
      x = [-37.431, 38.569]
      y = [-33.2585, 28.7415]
      z = [-31.6815, 31.3185]
  Number of Subdivisions in the Uniform grid:  nx = 152  ny = 124  nz = 126
============================================

=== [ Building Surface with NanoShaper ] ===

 <<INFO>> TBB - User selected num threads 1
 <<INFO>> Starting grid new PB initialization
 <<INFO>> Input nx = 152
 <<INFO>> Input ny = 124
 <<INFO>> Input nz = 126
 <<INFO>> Adjusted geometric baricenter ->  0.569 4.7415 6.3185
 <<INFO>> Grid is 153
 <<INFO>> MAX 38.569 42.7415 44.3185
 <<INFO>> MIN -37.431 -33.2585 -31.6815
 <<INFO>> Allocating memory...ok!
 <<INFO>> Initialization completed
 <<INFO>> Adjusting self intersection grid 
 <<INFO>> Self intersection grid is (before) 40
 <<INFO>> Self intersection grid is 32
 <<INFO>> Allocating self intersection grid....
 <<INFO>> Computing alpha shape complex....
 <<INFO>> Checking 681 probes for self intersections...ok!
 <<INFO>> Surface build-up time.. 0 [s]
 <<INFO>> Probe Radius value 1.4
 <<INFO>> Number of ses cells -> 9980
 <<INFO>> Number of del_point cells -> 1576
 <<INFO>> Number of regular del_edge cells -> 4895
 <<INFO>> Number of singular del_edge cells -> 7
 <<INFO>> Number of regular del_facet cells -> 3170
 <<INFO>> Number of singular del_facet cells -> 166
 <<INFO>> Use load balancing: 1
 <<INFO>> User selected num threads 1
 <<INFO>> Inside id value is 5
 <<INFO>> Ray-tracing panel 0...ok!
 <<INFO>> Ray-tracing panel 1...ok!
 <<INFO>> Ray-tracing panel 2...ok!
 <<INFO>> Ray-tracing computation time.. 1 [s]
 <<INFO>> Approximated 0 rays (0.00000 %)
 <<INFO>> Cavity detection time is 0 [s]
 <<INFO>> Assembling octrees..ok!
 <<INFO>> Unpacking rays packet 0 of size 67380
============================================
Building Surface with NanoShaper
Elapsed time : 636.642ms

============ [ Building Grid ] =============
  [Rank 0] Local nodes     : 651261
  [Rank 0] Local quadrants : 655496
  [Rank 1] Local nodes     : 651261
  [Rank 1] Local quadrants : 655496
  [Rank 2] Local nodes     : 651261
  [Rank 2] Local quadrants : 655496
  [Rank 3] Local nodes     : 651261
  [Rank 3] Local quadrants : 655496
  [Global] Total nodes     : 2563587
  [Global] Total quadrants : 2621984
============================================
Building Grid
Elapsed time : 1733.76ms

========= [ Building Epsilon Map ] =========
============================================
create element markers
Elapsed time : 2838.4ms

== [ Starting numerical solution using LIS ] ==
Selected BCs          : Null
Time to calculate rho : 12 ms
initial vector x      : all components set to 0
precision             : double
linear solver         : CGS
preconditioner        : SSOR
convergence condition : ||b-Ax||_1 <= 0.0e+00*||b||_1 + 1.0e-06 = 1.0e-06
matrix storage format : CSR
iteration:     1  relative residual = 3.067417E+05
iteration:     2  relative residual = 2.069423E+06
iteration:     3  relative residual = 9.036003E+05
iteration:     4  relative residual = 6.532491E+05
iteration:     5  relative residual = 4.131005E+05
iteration:     6  relative residual = 1.842191E+05
iteration:     7  relative residual = 1.043384E+05
iteration:     8  relative residual = 1.087400E+05
iteration:     9  relative residual = 1.199109E+05
iteration:    10  relative residual = 1.377359E+05
iteration:    11  relative residual = 1.366632E+05
iteration:    12  relative residual = 7.541739E+04
iteration:    13  relative residual = 3.261625E+04
iteration:    14  relative residual = 1.591401E+04
iteration:    15  relative residual = 9.041179E+03
iteration:    16  relative residual = 3.870120E+03
iteration:    17  relative residual = 1.474331E+03
iteration:    18  relative residual = 7.455318E+02
iteration:    19  relative residual = 4.959013E+02
iteration:    20  relative residual = 4.122821E+02
iteration:    21  relative residual = 3.448654E+02
iteration:    22  relative residual = 3.381472E+02
iteration:    23  relative residual = 2.927664E+02
iteration:    24  relative residual = 2.589968E+02
iteration:    25  relative residual = 3.105592E+02
iteration:    26  relative residual = 4.358623E+02
iteration:    27  relative residual = 6.184245E+02
iteration:    28  relative residual = 9.918619E+02
iteration:    29  relative residual = 2.123093E+03
iteration:    30  relative residual = 1.047364E+04
iteration:    31  relative residual = 6.671690E+04
iteration:    32  relative residual = 2.251344E+03
iteration:    33  relative residual = 3.310813E+02
iteration:    34  relative residual = 3.341111E+01
iteration:    35  relative residual = 2.811797E+02
iteration:    36  relative residual = 9.794163E+02
iteration:    37  relative residual = 1.568218E+03
iteration:    38  relative residual = 2.063978E+03
iteration:    39  relative residual = 3.137692E+03
iteration:    40  relative residual = 4.229767E+03
iteration:    41  relative residual = 5.523246E+03
iteration:    42  relative residual = 1.058535E+04
iteration:    43  relative residual = 5.472240E+04
iteration:    44  relative residual = 3.348667E+05
iteration:    45  relative residual = 1.336482E+04
iteration:    46  relative residual = 3.046107E+03
iteration:    47  relative residual = 1.262680E+03
iteration:    48  relative residual = 5.411764E+02
iteration:    49  relative residual = 2.315703E+02
iteration:    50  relative residual = 9.572131E+01
iteration:    51  relative residual = 3.563032E+01
iteration:    52  relative residual = 1.692430E+01
iteration:    53  relative residual = 1.079657E+01
iteration:    54  relative residual = 8.274985E+00
iteration:    55  relative residual = 6.346421E+00
iteration:    56  relative residual = 4.651552E+00
iteration:    57  relative residual = 4.154404E+00
iteration:    58  relative residual = 4.087012E+00
iteration:    59  relative residual = 4.341545E+00
iteration:    60  relative residual = 5.400192E+00
iteration:    61  relative residual = 6.573620E+00
iteration:    62  relative residual = 8.515465E+00
iteration:    63  relative residual = 2.103415E+01
iteration:    64  relative residual = 1.552371E+02
iteration:    65  relative residual = 2.180622E+05
iteration:    66  relative residual = 9.640164E+02
iteration:    67  relative residual = 2.647565E+04
iteration:    68  relative residual = 6.421371E+03
iteration:    69  relative residual = 4.978679E+02
iteration:    70  relative residual = 7.591661E+01
iteration:    71  relative residual = 3.228506E+01
iteration:    72  relative residual = 2.740193E+01
iteration:    73  relative residual = 1.981403E+01
iteration:    74  relative residual = 7.455439E+00
iteration:    75  relative residual = 2.343820E+00
iteration:    76  relative residual = 1.180555E+00
iteration:    77  relative residual = 1.239617E+00
iteration:    78  relative residual = 1.371968E+00
iteration:    79  relative residual = 9.045784E-01
iteration:    80  relative residual = 4.841410E-01
iteration:    81  relative residual = 3.970473E-01
iteration:    82  relative residual = 6.935133E-01
iteration:    83  relative residual = 2.415226E+00
iteration:    84  relative residual = 5.921300E+03
iteration:    85  relative residual = 1.206259E+00
iteration:    86  relative residual = 2.549196E-01
iteration:    87  relative residual = 1.201367E-01
iteration:    88  relative residual = 7.340935E-02
iteration:    89  relative residual = 5.018916E-02
iteration:    90  relative residual = 3.974402E-02
iteration:    91  relative residual = 3.486382E-02
iteration:    92  relative residual = 3.227947E-02
iteration:    93  relative residual = 3.121068E-02
iteration:    94  relative residual = 3.212094E-02
iteration:    95  relative residual = 3.851212E-02
iteration:    96  relative residual = 5.879442E-02
iteration:    97  relative residual = 1.151296E-01
iteration:    98  relative residual = 4.028686E-01
iteration:    99  relative residual = 1.284568E+01
iteration:   100  relative residual = 8.660895E-02
iteration:   101  relative residual = 1.429496E-02
iteration:   102  relative residual = 9.596026E-03
iteration:   103  relative residual = 1.276424E-02
iteration:   104  relative residual = 2.408514E-02
iteration:   105  relative residual = 7.927748E-02
iteration:   106  relative residual = 4.473241E+00
iteration:   107  relative residual = 2.479977E-02
iteration:   108  relative residual = 3.198689E-03
iteration:   109  relative residual = 1.096542E-03
iteration:   110  relative residual = 4.456833E-03
iteration:   111  relative residual = 7.930473E-03
iteration:   112  relative residual = 7.324415E-03
iteration:   113  relative residual = 4.483855E-03
iteration:   114  relative residual = 2.226977E-03
iteration:   115  relative residual = 1.011970E-03
iteration:   116  relative residual = 5.044715E-04
iteration:   117  relative residual = 8.236251E-04
iteration:   118  relative residual = 2.560694E-03
iteration:   119  relative residual = 5.315082E-03
iteration:   120  relative residual = 6.291384E-03
iteration:   121  relative residual = 3.707369E-03
iteration:   122  relative residual = 1.952204E-03
iteration:   123  relative residual = 1.822057E-03
iteration:   124  relative residual = 2.331663E-03
iteration:   125  relative residual = 1.950454E-03
iteration:   126  relative residual = 4.327983E-04
iteration:   127  relative residual = 5.514606E-05
iteration:   128  relative residual = 2.212388E-04
iteration:   129  relative residual = 1.613612E-02
iteration:   130  relative residual = 1.244619E-04
iteration:   131  relative residual = 4.470160E-04
iteration:   132  relative residual = 1.429313E-04
iteration:   133  relative residual = 2.557756E-05
iteration:   134  relative residual = 2.103223E-05
iteration:   135  relative residual = 5.647089E-05
iteration:   136  relative residual = 2.813162E-04
iteration:   137  relative residual = 2.527313E-02
iteration:   138  relative residual = 1.182483E-04
iteration:   139  relative residual = 2.618856E-05
iteration:   140  relative residual = 1.443040E-05
iteration:   141  relative residual = 1.221453E-05
iteration:   142  relative residual = 8.404410E-06
iteration:   143  relative residual = 3.633940E-06
iteration:   144  relative residual = 1.155894E-06
iteration:   145  relative residual = 5.552888E-07
linear solver status  : normal end


Time to solve linear problem:  4828 ms
============================================
Compute Potential
Elapsed time : 10541.5ms

================ [ Electrostatic Energy ] =================
  Net charge [e]:                                 -3.000000000000087
  Flux charge [e]:                                -3.00000000003496
  Polarization energy [kT]:                       -2487.179938212694
  Direct ionic energy [kT]:                       -8.537991557366576
  Coulombic energy [kT]:                          -64216.93379848526
  Sum of electrostatic energy contributions [kT]: -66712.65172825532
===========================================================
compute energy
Elapsed time : 634.727ms

Timing Report:
Event: Building Grid, total hits: 1, total time: 1.733758 s. (10.58133316797947%)
Event: Building Surface with NanoShaper, total hits: 1, total time: 0.636642 s. (3.885502538836899%)
Event: Compute Potential, total hits: 1, total time: 10.541535 s. (64.33625335076546%)
Event: compute energy, total hits: 1, total time: 0.634727 s. (3.873815063989382%)
Event: create element markers, total hits: 1, total time: 2.8384 s. (17.32309587842878%)
```

## 📊 Visualizing Results with ParaView

Once the `.vtk` file is generated, you can view it using **ParaView**.


### 🧭 Steps

1. Open **ParaView**
2. Go to `File > Open`, select the `.vtk` file
3. Click **Apply** in the Properties panel
4. Use "Color By" → `Potential`
5. Add filters such as:
   - `Slice` to inspect cross sections
   - `Contour` for isopotential surfaces
   - `Volume` to explore in 3D

---

### 🖼️ Example Visualization

ParaView allows overlaying the molecule structure (e.g., from `.pdb`) with the potential field for detailed inspection.

Go back to [Run Instructions](run.md) or return to [Tutorial Home](index.md).
