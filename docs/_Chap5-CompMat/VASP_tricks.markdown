---
title: VASP practical tricks
author: Lei Lei
category: notes
layout: post
---

## Check results
### Convergence
Using `tail`

```bash
tail ./*/vasp.log
```

### Final energy

```bash
grep --text "free  energy   TOTEN" OUTCAR | tail -1
```

Get the energy of a sequence of calculations:

~~~shell
for file in $(ls vasp.log.*)
do
name=${file##*.} # keep the last content after the last .
if grep -q "reached required accuracy - stopping structural energy minimisation" $file;
then grep "free  energy   TOTEN" OUTCAR.${name} | tail -1
else echo not_converged
echo $name >> not_converged.$(printf "%(%Y-%m-%d)T")
fi
done
~~~

#### Submit calculations that not converged

~~~shell
mapfile -t names < not_converged.$(printf "%(%Y-%m-%d)T")
do
cp CONTCAR.${name} POSCAR
timeout 3h srun --distribution=block:block --hint=nomultithread vasp_std > vasp.log.${name}
mv OUTCAR OUTCAR.${name}
mv CONTCAR CONTCAR.${name}
mv OSZICAR OSZICAR.${name}
mv vasprun.xml vasprun.xml.${name}
done
~~~

### NBANDS used in calculation

```bash
grep --text "NBANDS" OUTCAR
```

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


## 
