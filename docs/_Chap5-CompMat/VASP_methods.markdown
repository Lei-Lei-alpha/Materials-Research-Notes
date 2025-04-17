---
title: VASP Methods
author: Lei Lei
category: notes
layout: post
---

## Bader charge calculation from VASP

### Using [Bader Charge Analysis](https://theory.cm.utexas.edu/henkelman/code/bader/)

1. Get desired geometry, either through geometry relaxation or experiments or any other method of your choice.

   ```
   PREC = ACC
   NSW = 500 #(don't change geometry)
   ENCUT = 520 # value equal to or larger than 400 eV.
   EDIFF = 1e-6
   EDIFFG = -0.01
   IBRION = 2
   ISIF = 3
      .
      .
      .
   ```

3. Prepare an INCAR file with the following keywords:
   
   ```
   NSW = 0 #(don't change geometry)
   PREC = ACC #(use accurate integration grids)
   LAECHG = T #(generate the AECCAR0 and AECCAR2 files)
   LCHARG = T #(generate the CHGCAR file)
   ENCUT = 600 # value equal to or larger than 400 eV.
   ```

4. Sum the output charge files AECCAR0 AECCAR2 using the the [chgsum.pl](https://chgsum.pl) script to generate the CHGCAR_sum file.
   Electron charge density = core charge density + valance charge density
        CHGCAR_sum                AECCAR0                  AECCAR2
   ```shell
   chgsum.pl AECCAR0 AECCAR2
   ```
   
4. Run the Bader charge analysis program using the following command:ZVAL

   ```shell
   bader CHGCAR -ref CHGCAR_sum
   ```

   As described on [http://theory.cm.utexas.edu/henkelman/code/bader/](http://theory.cm.utexas.edu/henkelman/code/bader/), this procedure uses all electrons (CHGCAR_sum) to draw the compartments, then integrates the valence pseudodensity (CHGCAR) over these compartments to get the valence electron populations. This will avoid any need to iterate between steps # 2 & 5 in your procedure and will avoid the need to use such a fine integration grid or to add extra spheres the procedure in your original post requires.

5. Use this equation to compute the Bader net atomic charges:
   
   $$
   \text{atomic number} - \text{num frozen core} - \text{Bader population} = \text{Bader net atomic charge}
   $$
   $$
   \text{atomic number} - \text{num frozen core} = \text{ZVAL in POTCAR}
   $$
   $$
   \text{Bader net atomic charge} = \text{ZVAL} - \text{Bader population}
   $$
   
   where:  
   num frozen core: number of frozen core electrons for that atom in the PAW potential used  
   Bader population: number printed in the `ACF.dat` file.

Note: increase NG(X, Y, Z) F until the total charge is correct!

### Use [Chargemol](https://www.researchgate.net/deref/http%3A%2F%2Fddec.sourceforge.net)

An easier approach is to use the free  to compute the DDEC6 net atomic charges and bond orders. You follow steps # 1 and 2 above as before. However, then you have the following remaining step:

3) Place the AECCAR0, AECCAR2, CHGCAR, and POTCAR files into a directory along with the job_control.txt file. (See the Chargemol program manual at the above link for details on how to set up the job_control.txt file.) Then run the Chargemol program to get the net atomic charges, atomic spin moments (for magnetic materials), atomic dipoles and quadrupoles, bond orders, overlap populations, and other atomic properties printed in easy-to-read text files.

The DDEC6 methodology is described in the following publications:

(1) [T. A. Manz and N. Gabaldon Limas, “Introducing DDEC6 atomic population analysis: part 1. Charge partitioning theory and methodology,” RSC Advances, 6 (2016) 47771-47801](http://dx.doi.org/10.1039/C6RA04656H)

(2) [N. Gabaldon Limas and T. A. Manz, “Introducing DDEC6 atomic population analysis: part 2. Computed results for a wide range of periodic and nonperiodic materials,” RSC Advances, 6 (2016) 45727-45747](http://dx.doi.org/10.1039/C6RA05507A)

(3) [T. A. Manz, "Introducing DDEC6 atomic population analysis: part 3. Comprehensive method to compute bond orders," RSC Advances, 7 (2017) 45552-45581](http://dx.doi.org/10.1039/C7RA07400J)

## Charge difference calculation
1. Get desired geometry, either through geometry relaxation or experiments or any other method of your choice.

   ```
   PREC = ACC
   NSW = 500 #(don't change geometry)
   ENCUT = 520 # value equal to or larger than 400 eV.
   EDIFF = 1e-6
   EDIFFG = -0.01
   IBRION = 2
   ISIF = 3
      .
      .
      .
   ```
2. Do single point calculations for defferent fragment A, B and AB. Generate the POSCARs of fragment A and B from AB and do not optimise the atomic positions.
   ```
   NSW = 0 #(don't change geometry)
   PREC = ACC #(use accurate integration grids)
   LCHARG = T #(generate the CHGCAR file)
   ENCUT = 600 # value equal to or larger than 400 eV.
      .
      .
      .
   ```
3. Get CHGDIFF file using VASPKIT and the CHGCARs obtained from previous calculation.
4. Plot the electron density difference with VESTA
   
## Density of States calculation
1. Get desired geometry, either through geometry relaxation or experiments or any other method of your choice.

   ```
   PREC = ACC
   NSW = 500 #(don't change geometry)
   ENCUT = 520 # value equal to or larger than 400 eV.
   EDIFF = 1e-6
   EDIFFG = -0.01
   IBRION = 2
   ISIF = 3
      .
      .
      .
   ```
2. Do single point calculation SCF and save the CHGCAR file.
   ```
   PREC = ACC #(use accurate integration grids)
   ISTART = 0
   ICHARG = 2
   NSW = 0 #(don't change geometry)
   IBRION = -1
   EDIFF = 1e-6
   LCHARG = T #(generate the CHGCAR file)
   ENCUT = 600 # value equal to or larger than 130 % in the POTCAR file.
   ISMEAR = 0
   SIGMA = 0.1
      .
      .
      .
   ```
3. Density of states calculation
   ```
   PREC = ACC #(use accurate integration grids)
   ISTART = 1
   ICHARG = 11
   NSW = 0 #(don't change geometry)
   IBRION = -1
   EDIFF = 1e-6
   ENCUT = 600 # value equal to or larger than 130 % in the POTCAR file.
   ISMEAR = -5
   SIGMA = 0.1
   LCHARG = T #(generate the CHGCAR file)
   LORBIT = 11
   NEDOS = 10000
   # EMIN = -5
   # EMAX = 15
   # NBANDS = 320 # Can find in the OUTCAR file
      .
      .
      .
   ```
