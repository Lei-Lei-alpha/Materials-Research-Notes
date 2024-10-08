---
title: VASP Methods
author: Lei Lei
category: notes
layout: post
---

## Bader charge calculation from VASP

### Using [Bader Charge Analysis](https://theory.cm.utexas.edu/henkelman/code/bader/)

1. Get desired geometry, either through geometry relaxation or experiments or any other method of your choice.

2. Prepare an INCAR file with the following keywords: 

   ```
   NSW = 0 #(don't change geometry)
   Prec = Accurate #(use accurate integration grids)
   LAECHG = T #(generate the AECCAR0 and AECCAR2 files)
   LCHARG = T #(generate the CHGCAR file)
   ENCUT = 600 # value equal to or larger than 400 eV.
   ```

3. Sum the output charge files AECCAR0 AECCAR2 using the the [chgsum.pl](https://www.researchgate.net/deref/http%3A%2F%2Fchgsum.pl) script to generate the CHGCAR_sum file.

4. Run the Bader charge analysis program using the following command:

   ```shell
   bader CHGCAR -ref CHGCAR_sum
   ```

   As described on [http://theory.cm.utexas.edu/henkelman/code/bader/](https://www.researchgate.net/deref/http%3A%2F%2Ftheory.cm.utexas.edu%2Fhenkelman%2Fcode%2Fbader%2F), this procedure uses all electrons (CHGCAR_sum) to draw the compartments, then integrates the valence pseudodensity (CHGCAR) over these compartments to get the valence electron populations. This will avoid any need to iterate between steps # 2 & 5 in your procedure and will avoid the need to use such a fine integration grid or to add extra spheres the procedure in your original post requires.

5. Use this equation to compute the Bader net atomic charges:

   $$
   z - \text{num_frozen_core} - \text{Bader population} = \text{Bader net atomic charge}
   $$

   where:  
   $z$: atomic number  
   num_frozen_core: number of frozen core electrons for that atom in the PAW potential used  
   Bader population: number printed in the `ACF.dat` file.

### Use [Chargemol](https://www.researchgate.net/deref/http%3A%2F%2Fddec.sourceforge.net)

An easier approach is to use the free  to compute the DDEC6 net atomic charges and bond orders. You follow steps # 1 and 2 above as before. However, then you have the following remaining step:

3) Place the AECCAR0, AECCAR2, CHGCAR, and POTCAR files into a directory along with the job_control.txt file. (See the Chargemol program manual at the above link for details on how to set up the job_control.txt file.) Then run the Chargemol program to get the net atomic charges, atomic spin moments (for magnetic materials), atomic dipoles and quadrupoles, bond orders, overlap populations, and other atomic properties printed in easy-to-read text files.

The DDEC6 methodology is described in the following publications:

(1) [T. A. Manz and N. Gabaldon Limas, “Introducing DDEC6 atomic population analysis: part 1. Charge partitioning theory and methodology,” RSC Advances, 6 (2016) 47771-47801](http://dx.doi.org/10.1039/C6RA04656H)

(2) [N. Gabaldon Limas and T. A. Manz, “Introducing DDEC6 atomic population analysis: part 2. Computed results for a wide range of periodic and nonperiodic materials,” RSC Advances, 6 (2016) 45727-45747](http://dx.doi.org/10.1039/C6RA05507A)

(3) [T. A. Manz, "Introducing DDEC6 atomic population analysis: part 3. Comprehensive method to compute bond orders," RSC Advances, 7 (2017) 45552-45581](http://dx.doi.org/10.1039/C7RA07400J)
