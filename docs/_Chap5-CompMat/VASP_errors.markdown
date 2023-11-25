---
title: VASP errors
author: Lei Lei
category: notes
layout: post
---

## Inconsistent Bravais lattice types found for crystalline and reciprocal lattice

<div style="border-width: 1px; border-style:solid; border-color: #b2182b; border-radius:3px; background-color: #FFEFEF;">
    <div style="padding-left: 25px; padding-right: 10px;">
        <p>
            <span style="color: #b2182b; font-weight: 550;">Suggested SOLUTIONS</span>
            <ol>
            <li>Refine the lattice parameters of your structure,</li>
            <li>and/or try changing <b>SYMPREC</b></li>
            </ol>
        </p>
        <p>
            <span style="color: #b2182b; font-weight: 550;">SYMPREC:</span> determines to which accuracy the positions in the <b>POSCAR</b> file must be specified. Default 10<sup style="font-size:75%">&minus;5</sup>.
        </p>
    </div>
</div>

The error was fixed by changing **SYMPREC** to $10^{-4}$.

If the system is orthorhombic, the following setting maybe helpful:

Don't use ISIF=3 but use  ISIF=4  instead! Then the cell shape only (b/a, c/a) will be relaxed at  *fixed volume* and the volume itself can be given in POSCAR on the second line as a negative value (i.e., minus volume). From that and the initial three lattice vectors below VASP calculates internally the correct a, b, c. After relaxation you will find changes in these three lattice vectors (lenghts) but such that volume will remain conserved. At the end, on CONTCAR you can then find as usual a "scaling parameter" in the second line and the three altered lattice vectors from which you can (after multiplication with the "scaling parameter") determine the (relaxed) lattice constants a, b, and c for the given volume. And of course you will also know the total energy then but even hydrostatic pressure printed in OUTCAR ("pressure = ..."). So you could not only determine an energy-volume but even a pressure-volume curve!

Only if you have no idea at all about the equilibrium lattice constants / volume you might consider a "pre-relaxation" run with ISIF=3 first before you begin to sample different volumes around the (approximate) equilibrium volume with ISIF=4 ...

Just take care that your plane-wave cutoff is high enough (in order to avoid too large Pullay stresses -- I usually recommend to approximately double the cutoff with respect to what is set by VASP for PREC=high/accurate) and that you perform enough relaxation steps to get all stress tensor components converged to at least better than order of 0.1 kBar. And also use a rather small EDIFF (use maybe values of about 1-9 to 1-6, in order to obtain well-converged wave functions which are important for numerically accurate forces and stresses). And overall try to be as accurate as possible (e.g. PREC=high or PREC=accurate is recommended, maybe also ADDGRID=.TRUE.).

This strategy works by the way for all types of (non-cubic) lattices, also monoclinic or triclinic ones (getting not only the three lattice constants but also the one or three angles). And even for tetragonal or hexagonal ones it can still be useful.


## Fatal error: bracketing interval incorrect

Error message:

```shell
ZBRENT: fatal error: bracketing interval incorrect
please rerun with smaller EDIFF, or copy CONTCAR
to POSCAR and continue
```

<div style="border-width: 1px; border-style:solid; border-color: #b2182b; border-radius:3px; background-color: #FFEFEF;">
    <div style="padding-left: 25px; padding-right: 10px;">
        <p>
            <span style="color: #b2182b; font-weight: 550;">Suggested SOLUTIONS</span>: Have a look at the <code>OUTCAR</code> file, at the forces of the last ionic step before Brent's algorithm fails
            <ul>
            <li>if the calculation is very well-converged already, please proceed using <b>IBRION=1</b>; <b>ADDGRID=.TRUE.</b> and a larger <b>ENCUT</b></li>
            <li>if this error appears at an early stage of the relaxation, pelase check if your input geometry was reasonable, eg by having a look at the interatomic distances (written in <code>OUTCAR</code>) or the forces of the first geometries. It may then help to increase the accuracy (<b>PREC = Accurate</b> and a higher <b>ENCUT</b>) and to decrease <b>POTIM</b> to avoid too large stepwidhts in the first relaxation step.</li>
            </ul>
        </p>
    </div>
</div>


## 