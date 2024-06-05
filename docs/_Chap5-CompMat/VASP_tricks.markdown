---
title: VASP practical tricks
author: Lei Lei
category: notes
layout: post
---

## Set up to run VASP from ASE
> Refer to ASE VASP calculator doc: [VASP Calculator](https://wiki.fysik.dtu.dk/ase/ase/calculators/vasp.html#id2)
### Execute command
ASE has to know the command to call and run vasp. This can be done by one of the following methods:
- Add an environment variable that contains the command to execute VASP. 
    ```shell
    export ASE_VASP_COMMAND="mpirun vasp_std"
    ```

- Specify the `command` keyword in the `Vasp` calculator. This will overwrite the environment variable.
    ```python
    calc = Vasp(command = "mpirun vasp_std")
    ```

- Alternatively, you can use the following script with the name `run_vasp.py`.
    ```python
    import os
    exitcode = os.system("vasp")
    ```
    In this case, the environment variable `VASP_SCRIPT` must point to the above file.

### Pseudopotentials
Pseudopotential directory has to be specified through the environment variable `VASP_PP_PATH`. You can set both environment variables in `.bashrc`:
```shell
export ASE_VASP_COMMAND="mpirun vasp_std"
# Alternatively, use something like this
#export VASP_SCRIPT=$HOME/vasp/run_vasp.py
export VASP_PP_PATH=$HOME/vasp/mypps
```
The following environment variable can be used to automatically copy the van der Waals kernel to the calculation directory. The kernel is needed for vdW calculations, see [VASP vdW wiki](https://www.vasp.at/wiki/index.php/Nonlocal_vdW-DF_functionals), for more details. The kernel is looked for whenever `luse_vdw=True`.

```shell
export ASE_VASP_VDW=$HOME/<path-to-vdw_kernel.bindat-folder>
```
The environment variable `ASE_VASP_VDW` should point to the folder where the `vdw_kernel.bindat` file is located.


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
            For the Kerker scheme (<a href="https://www.vasp.at/wiki/index.php/IMIX">IMIX</a> = 1) either <b>AMIX</b> or <a href="https://www.vasp.at/wiki/index.php/BMIX">BMIX</a> can be optimized, but we recommend to change only <a href="https://www.vasp.at/wiki/index.php/BMIX">BMIX</a> and keep <b>AMIX</b> fixed (you must decrease <a href="https://www.vasp.at/wiki/index.php/BMIX">BMIX</a> if the mean eigenvalue is larger than one, and increase <a href="https://www.vasp.at/wiki/index.php/BMIX">BMIX</a> if the mean eigenvalue Γmean<1). However, the optimal <b>AMIX</b> depends very much on the system, for metals this parameter usually has to be rather small, e.g. AMIX= 0.02.
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
files=$(ls vasp.log.*)
for file in $files
do
echo ${file##*.}
done
for file in $files
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
for name in ${names[@]}
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
