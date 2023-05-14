---
layout: post
title:  "Concepts and methods in computational materials science"
author: Lei Lei
date:   2023-05-12 10:44:07 +0000
category: Lab Notes
---
<hr>
> Author: Lei Lei<br>
Date: 28-03-2022

# Stability of structure
## Formation energy

## Energy above convex hull
**Convex hull** is a plot of formation energy with respect to the composition which connects phases that are lower in energy than any other phases or a linear combination of phases at that composition. Phases that lie on the convex hull are thermodynamically stable and the ones above it are either metastable or unstable. This plot can only give an idea about the stability of the structure at 0 K. Higher temperature calculation (Phonopy is one of the codes) can assure the stability of the given structure at the working temperature (mostly greater than 0 K).

The convex hull (represented by the lines at the bottom of the phase diagram) represents the set of lowest possible potential energy states you can get from either single materials (at the black points) or mixtures of those materials (along the black lines connecting the points). If a compound is above a line in the convex hull (e.g. the lighter blue dots in the below image), it will spontaneously decompose into products on the endpoints of that line. The “energy_above_hull” for a given material is the distance from its dot in the below image and the lines connecting the black dots at the bottom.

<center>
<img style = "width: 60%;" src = "../img/convex_hull.png"/>
</center>

## Preparation
- [x] Thermometer
- [x] Sand bath

## Solution preparation

- Chemical quantities

|||||
|:--:|:--:|:--:|:--:|
|CH<sub>3</sub>NH<sub>3</sub>PbI<sub>3</sub>|PbI<sub>2</sub>|CH<sub>3</sub>NH<sub>3</sub>I|GBL|
|Quantity|6.915 g|2.385 g|10 mL|
|CH<sub>3</sub>NH<sub>3</sub>PbBr<sub>3</sub>|PbBr<sub>2</sub>|CH<sub>3</sub>NH<sub>3</sub>Br|DMF|
|Quantity|4.5876 g|1.3996 g|10 mL|

The actual quantities of chemicals used are shown in [Fig.1](#fig1).



| <a id="fig1"> <img id="figure1.1" src="/gitbook/images/img/crys_cw_29032022.jpg" alt="setup" style="width:50%"> </a>|
|:--:|
| <b>Fig.1</b> The setup of crystal growth apparatus using sand bath within the glovebox. |

<!-- <figure align = "center">
<img src="/gitbook/images/img/crys_cw_29032022.jpg" alt="setup" style="width:40%">
<figcaption align = "center"><b>Fig.1</b> The setup of crystal growth apparatus using sand bath within the glovebox.</figcaption>
</figure> -->

- Temperatures
  - GBL has a maximal solubility for MAPbI<sub>3</sub> precursors at 60 &deg;C[<sup>1</sup>][Ref1], and the solution is prepared and filtered under 60 &deg;C.
  - DMF has a maximal solubility for MAPbBr<sub>3</sub> precursors at room temperature.

## Filtration
The precursor solutions were filtered with a 0.22-0.25 &mu;m PVDF syringe filter (Fig.2).

| <img src="/gitbook/images/img/crys_sf_29032022.jpg" alt="setup" style="width:40%"> |
|:--:|
| <b>Fig.2</b> Photograph of the syringe filter used. |

<!-- <figure align = "center">
<img src="/gitbook/images/img/crys_sf_29032022.jpg" alt="setup" style="width:40%">
<figcaption align = "center"><b>Fig.2</b> Photograph of the syringe filter used.</figcaption>
</figure> -->

## Crystal growth
### Apparatus
The crystal growth used the retrograde solubility of MAPbX<sub>3</sub> precursors as reported in Ref[<sup>1</sup>][Ref1]. The apparatus setup is shown in Fig.3 which used sand bath instead of oil bath.

| <img src="/gitbook/images/img/crys_setup.jpg" alt="setup" style="width:60%"> |
|:--:|
| <b>Fig.3</b> The setup of crystal growth apparatus using sand bath within the glovebox. |

<!-- <figure align = "center">
<img src="/gitbook/images/img/crys_setup.jpg" alt="setup" style="width:60%">
<figcaption align = "center"><b>Fig.3</b> The setup of crystal growth apparatus using sand bath within the glovebox.</figcaption>
</figure> -->

### Clean vials
The vials were cleaned by detergent and deionised water.

| <img src="/gitbook/images/img/crys_vc_30032022.jpg" alt="setup" style="width:50%"> |
|:--:|
| <b>Fig.4</b> The cleaned vials to be used for the growth of MAPbX<sub>3</sub> crystals. |

### MAPbBr<sub>3</sub> growth

### First trial

The temperature of the sand bath was 87.5 &deg;C as shown in Fig.5.

| <img src="/gitbook/images/img/crys_MAPB_temp_30032022.jpg" alt="setup" style="width:50%"> |
|:--:|
| <b>Fig.5</b> The cleaned vials to be used for the growth of MAPbX<sub>3</sub> crystals. |

A MAPBr seed was added to the filtered precursor solution before the growth. The temperature rose to 89 &deg;C at the beginning of crystal growth (19:40).

| <img src="/gitbook/images/img/crys_MAPB_s1_30032022.jpg" alt="setup" style="width:50%"> |
|:--:|
| <b>Fig.6</b> Photograph of vials immersed in the sand bath for the growth of MAPbBr<sub>3</sub> crystals from the precursor solution. |

The temperature was too high for growth, a lot of small crystallites form. The vials were taken out for overnight to allow the dissolve of these crystallites. A lower temperature was set for another trial.

### The second go

| <img src="/gitbook/images/img/crys_MAPB_s1f_30032022.jpg" alt="setup" style="width:50%"> |
|:--:|
| <b>Fig.7</b> Photograph of the crystallites formed due to the fast nuleation. |

The sand bath temperature was down to 75 &deg;C and the vials containing the precursor solutions were placed into the bath.

| <img src="/gitbook/images/img/crys_MAPB_s2_31032022.jpg" alt="setup" style="width:50%"> |
|:--:|
| <b>Fig.8</b> Photograph of vials immersed in the sand bath (75 &deg;C) for the growth of MAPbBr<sub>3</sub> crystals from the precursor solution. |

The temperature decreased to ~ 72 &deg;C the second morning.

For Further growth the temperature was increased to 82 &deg;C.

### MAPbI<sub>3</sub> growth

[Ref1]: https://doi.org/10.1039/C5CC06916E
