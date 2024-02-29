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
#### Tips for faster convergence

<div style="border-width: 1px; border-style:solid; border-color: #b2182b; border-radius:3px; background-color: #FFEFEF;">
    <div style="padding-left: 25px; padding-right: 10px;">
        <p>
            <span style="color: #b2182b; font-weight: 550;">Try linear mixing</span>
            <ol>
            <li>Set AMIX = 0.1 - 0.2, BMIX = 0.0001</li>
            <li>The optimal AMIX is given by the present AMIX &times; GAMMA</li>
            </ol>
        </p>
        <p>
            For the Kerker scheme (<a href="https://www.vasp.at/wiki/index.php/IMIX">IMIX</a> = 1) either **AMIX** or <a href="https://www.vasp.at/wiki/index.php/BMIX">BMIX</a> can be optimized, but we recommend to change only <a href="https://www.vasp.at/wiki/index.php/BMIX">BMIX</a> and keep **AMIX** fixed (you must decrease <a href="https://www.vasp.at/wiki/index.php/BMIX">BMIX</a> if the mean eigenvalue is larger than one, and increase <a href="https://www.vasp.at/wiki/index.php/BMIX">BMIX</a> if the mean eigenvalue Γmean<1). However, the optimal **AMIX** depends very much on the system, for metals this parameter usually has to be rather small, e.g. AMIX= 0.02.
        </p>
    </div>
</div>

### Final energy

```bash
grep --text "free  energy   TOTEN" OUTCAR | tail -1
```

Get the energy of a sequence of calculations:

~~~shell
list=not_converged.$(printf "%(%Y-%m-%d)T")
if [ -f "$list" ]; then
    > $list # Clear the content of the list.
fi
for file in $(ls vasp.log.*)
do
name=${file##*.} # keep the last content after the last ".".
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
