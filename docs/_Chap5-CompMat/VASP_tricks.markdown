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

### NBANDS used in calculation

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