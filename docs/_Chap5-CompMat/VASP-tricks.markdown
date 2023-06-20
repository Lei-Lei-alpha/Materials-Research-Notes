---
title: VASP errors and practical tricks
author: Lei Lei
category: notes
layout: post
---

## Inconsistent Bravais lattice types found for crystalline and reciprocal lattice

Suggested SOLUTIONS:
    1. Refine the lattice parameters of your structure,
    2. and/or try changing **SYMPREC**.

**SYMPREC** 

<div style="border-width: 1px; border-style:solid; border-color: #b2182b; border-radius:3px; background-color: #FFEFEF;">
    <div style="padding-left: 25px; padding-right: 10px;">
        <p>
            <span style="color: #b2182b; font-weight: 550;">SYMPREC:</span> determines to which accuracy the positions in the <b>POSCAR</b> file must be specified. Default $10^{-5}$.
        </p>
    </div>
</div>

