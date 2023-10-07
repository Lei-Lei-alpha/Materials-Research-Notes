---
layout: post
title: Machine learned potentials
author: Lei Lei
category: Lab Notes
---

## Behler and Parrinello[\[ 1\]][Ref1]

Encode the local (within a certain cutoff radius) chemical environment of each atom in a descriptor, e.g., using symmetry functions[\[ 2\]][Ref2] as input to predict atomic energies. Total potential energy is modeled as the sum of individual contributions, and forces are obtained by derivation of potential energy with respect to atomic positions.

<b>Drawback</b>: An atomic energy decomposition makes predictions size extensive and the learned model applicable to systems of different size.

## Message-passing neural networks (MPNNs) [\[ 3\]][Ref3]

Alternative to manually designed descriptors, use raw atomic numbers and Cartesian coordinates as input. Suitable atomic representations can be learned from (and adapt to) the reference data automatically. "Passing messages" between atoms to iteratively build up increasingly sophisticated descriptors.

<b>Drawback</b>: atomic numbers and Cartesian coordinates (or descriptors derived from them) do not provide an unambiguous description of chemical systems[\[ 4\]][Ref4]. Contains nuclear degrees of freedom without information on electronic structure (e.g. charge or spin state).





[Ref1]: https://doi.org/10.1103/PhysRevLett.98.146401
[Ref2]: https://doi.org/10.1063/1.3553717
[Ref3]: https://proceedings.mlr.press/v70/gilmer17a.html
[Ref4]: https://doi.org/10.1073/pnas.0408036102