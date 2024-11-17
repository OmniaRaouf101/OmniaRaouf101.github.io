---
layout: slides_mila_owl
title: "Crystal-GFN"
---

name: crystal-gfn-feb24
class: title, middle

### Crystal-GFN: sampling crystals with desirable properties and constraints

Mila AI4Science: Alex Hernandez-Garcia, Alexandre Duval, Alexandra Volokhova, Yoshua Bengio, Divya Sharma, Pierre Luc Carrier, Yasmine Benabed, Michał Koziarski, Victor Schmidt, Pierre-Paul De Breuck

.turquoise[Université de Montréal, February 28th 2024]

.center[
<a href="https://mila.quebec/"><img src="../assets/images/slides/logos/mila-beige.png" alt="Mila" style="height: 4em"></a>
&nbsp&nbsp&nbsp&nbsp
<a href="https://www.umontreal.ca/"><img src="../assets/images/slides/logos/udem-white.png" alt="UdeM" style="height: 4em"></a>
]

.smaller[.footer[
Slides: [alexhernandezgarcia.github.io/slides/{{ name }}](https://alexhernandezgarcia.github.io/slides/{{ name }})
]]

---

## Crystal structure generation in the literature

Example: .highlight2[Crystal Diffusion Variational Autoencoder (CDVAE)]: a diffusion process that moves .highlight1[atomic coordinates] towards a lower energy state and updates atom types to satisfy bonding preferences between neighbors. The key idea is to learn the diffusion process from the data distribution of stable materials. .cite[(Xie et al., 2022)]

.center[![:scale 100%](../assets/images/slides/crystals/cdvae.png)]

.references[Xie et al. [Crystal diffusion variational autoencoder for periodic material generation](https://arxiv.org/abs/2110.06197). ICLR 2022] 

???

A: atom types
X: atom coordinates
L: perdiodic lattice: l1, l2, l3 (3x3)

---

count: false

## Crystal structure generation in the literature

Example: .highlight2[Physics Guided Crystal Generative Model (PGCGM)]: A GAN with the affine matrices of the symmetry operations and element properties as inputs, augmented with a distance loss between real and fake materials, and data augmentation on the atomic positions. .cite[(Zhao et al., 2023)]

.center[![:scale 70%](../assets/images/slides/crystals/pgcgm.png)]

.references[Zhao et al. [Physics guided deep learning for generative design of crystal materials with symmetry constraints](https://www.nature.com/articles/s41524-023-00987-9). npj computational materials 2023] 

---

count: false

## Crystal structure generation in the literature

Example: .highlight2[MatterGen]: An evolution of CDVAE that performs diffusion not only on atomic positions but also on the atom types and the lattice. .cite[(Zeni, Pinsler, Zügner, Fowler et al., 2023)]

.center[![:scale 90%](../assets/images/slides/crystals/mattergen.png)]

.references[Zeni, Pinsler, Zügner, Fowler et al. [MatterGen: a generative model for inorganic materials design](https://arxiv.org/abs/2312.03687). arXiv 2023] 

---

count: false

## Crystal structure generation in the literature

Example: .highlight2[UniMat]: A denoising diffusion model learns to move atoms from random locations back to their original locations. Atoms not present in the crystal are moved to the null location during the denoising process, allowing crystals with an arbitrary number of atoms to be generated.

.center[![:scale 90%](../assets/images/slides/crystals/unimat.png)]

.references[Yang et al. [Scalable Diffusion for Materials Generation](https://arxiv.org/abs/2311.09235). arXiv 2023] 

---

## Our approach
### Crystal structure parameters

.context[CDVAE and other works tackle crystal structure generation in the space of atom coordinates.]

Instead of optimising the atom positions by learning from a small data set, we draw .highlight1[inspiration from theoretical crystallography to sample crystals in a lower-dimensional space of crystal structure parameters].

.left-column[
.center[![:scale 65%](../assets/images/slides/crystals/crystal_systems_table.png)]
]
.right-column[
.center[![:scale 30%](../assets/images/slides/crystals/unit_cell.png)]
]

.conclusion[We sample materials in the space of space groups (230 groups), compositions (elements and number of atoms) and lattice parameters (6 parameters: $a, b, c, \alpha, \beta, \gamma$).]

---

## Crystal-GFlowNet
### Sequential generation

.center[![:scale 100%](../assets/images/slides/crystals/crystalgfn_init.png)]

---

count: false

## Crystal-GFlowNet
### Sequential generation

.center[![:scale 100%](../assets/images/slides/crystals/crystalgfn_sg.png)]

---

count: false

## Crystal-GFlowNet
### Sequential generation

.center[![:scale 100%](../assets/images/slides/crystals/crystalgfn_sg_output.png)]

---

count: false

## Crystal-GFlowNet
### Sequential generation

.center[![:scale 100%](../assets/images/slides/crystals/crystalgfn_comp.png)]

---

count: false

## Crystal-GFlowNet
### Sequential generation

.center[![:scale 100%](../assets/images/slides/crystals/crystalgfn_comp_output.png)]

---

count: false

## Crystal-GFlowNet
### Sequential generation

.center[![:scale 100%](../assets/images/slides/crystals/crystalgfn_lp.png)]

---

count: false

## Crystal-GFlowNet
### Sequential generation

.center[![:scale 100%](../assets/images/slides/crystals/crystalgfn_lp_output.png)]

---

count: false

## Crystal-GFlowNet
### Sequential generation

.center[![:scale 100%](../assets/images/slides/crystals/crystalgfn_all.png)]

---

count: false

## Crystal-GFlowNet
### Sequential generation

.center[![:scale 100%](../assets/images/slides/crystals/crystalgfn_all.png)]

.conclusion[Crystal-GFN binds multiple spaces representing crystallographic and material properties, setting intra- and inter-space hard constraints in the generation process.]

---

## GFlowNet approach
### Advantages

.context[We generate materials in the lower-dimensional space of crystal structure parameters.]

* Constructing materials by their crystal structure parameters allows us to introduce .highlight1[physicochemical and geometric _hard_ constraints]:
--

    * Charge neutrality of the composition.
    * Compatibility of composition and space group.
    * Hierarchical structure of the space group.
    * Compatibility of lattice parameters and lattice system.
--
* .highlight1[Searching in the lower-dimensional space] of crystal structure parameters may be more efficient than in the space of atom coordinates.

--
* Provided we have access to a predictive model of a material property, we can .highlight1[flexibly generate materials with desirable properties].

--
* We can .highlight1[flexibly sample materials with specific characteristics, such as composition or space group]. 

---

## Crystal-GFlowNet
### Properties

We can train a Crystal-GFN with any reward function, provided it is computationally tractable. Therefore, we can use it to .highlight1[generate materials with different properties]. 
--
We have tested the following properties:

- .highlight1[Formation energy per atom] [eV/atom], via a pre-trained machine learning model.
- .highlight1[Electronic band gap] [eV] (squared distance to a target value, 1.34 eV), via a pre-trained machine learning model.
- .highlight1[Unit cell density] [g/cm<sup>3</sup>], calculated _exactly_ from the GFN outputs.

--

.highlight1[Coming soon]: pre-trained machine learning model to predict the ionic conductivity [S/cm].

---

## Crystal-GFlowNet
### Proxy ML models of the formation energy and the band gap

- MLP architecture
- Inputs:
    - Space group embedding
    - Composition embedding as in PhAST (Duval et al., 2022)
    - Standardised lattice parameters
- Trained on MatBench (Materials Project)
    - Mean absolute error: 
        - Formation energy: 0.10 eV/atom $\pm$ 0.005
        - Band gap: 0.321 eV $\pm$ 0.003
- Reward: $R(x) = \exp(-\frac{MLP(x)}{T})$

---

## Crystal-GFlowNet
### Experiments

- Space groups: all 113 space groups present in the MatBench data set.
- Compositions:
    - Up to 5 unique elements from the these 22 elements: ts: H, Li, B, C, N, O, F, Na, Mg, Al, Si, P, S, Cl, K, V, Mn, Fe, Co, Ni, Cu, Se. Th (22 most common elements in the training set). 
    - Up to 16 atoms per element (80 atoms in total)
- Lattice parameters:
    - Lengths: 0.9-100 angstroms.
    - Angles: 50-150°.
    - These ranges contain the bulk of the data set (excluding outliers).

---

## Results
### Density

What do we expect Crystal-GFN to sample?

- Small length lattice parameters (small volume): 
--
4.9 Å compared to 8.3 Å in the validation dataset. &#10004;
--

- Denser packing space group: 
--
We see a shift towards spacegroups from the cubic, tetragonal, and hexagonal lattices (81, 99, 115, 195, 200, 207, 215, 221). &#10004;
--

- Higher number of atoms (larger mass): 
--
58.8 atoms on average compared to 48.1 over the validation set. &#10004;
--

- Heavier elements (larger mass): 
--
Heavier such as Se, Cu, Ni are sampled the most often. &#10004;

---

## Results
### Density

.center[![:scale 80%](../assets/images/slides/crystals/density_elements.png)]

---

## Results
### Formation energy

.context35[The formation energy is predicted by a pre-trained ML model.]

.center[![:scale 70%](../assets/images/slides/crystals/eform_distr_1.png)]

---

count: false

## Results
### Formation energy

.context35[The formation energy is predicted by a pre-trained ML model.]

.center[![:scale 70%](../assets/images/slides/crystals/eform_distr_2.png)]

---

count: false

## Results
### Formation energy

.context35[The formation energy is predicted by a pre-trained ML model.]

.center[![:scale 70%](../assets/images/slides/crystals/eform_distr_3.png)]

---

count: false

## Results
### Formation energy

.context35[The formation energy is predicted by a pre-trained ML model.]

.center[![:scale 70%](../assets/images/slides/crystals/eform_distr_4.png)]

---

count: false

## Results
### Formation energy

.context[.highlight1[After training, Crystal-GFN samples structures with even lower formation energy [eV/atom] than the validation set.]]

.center[![:scale 70%](../assets/images/slides/crystals/eform_distr_4.png)]

---

## Results
### Band gap

.context35[We aimed at sampling structures with band gap close to 1.34 eV.]

.center[![:scale 70%](../assets/images/slides/crystals/bg_distr_1.png)]

---

count: false

## Results
### Band gap

.context35[We aimed at sampling structures with band gap close to 1.34 eV.]

.center[![:scale 70%](../assets/images/slides/crystals/bg_distr_2.png)]

---

count: false

## Results
### Band gap

.context35[We aimed at sampling structures with band gap close to 1.34 eV.]

.center[![:scale 70%](../assets/images/slides/crystals/bg_distr_3.png)]

---

count: false

## Results
### Band gap

.context35[We aimed at sampling structures with band gap close to 1.34 eV.]

.center[![:scale 70%](../assets/images/slides/crystals/bg_distr_4.png)]

---

count: false

## Results
### Band gap

.context[.highlight1[After training, Crystal-GFN samples structures with band gap [eV] around the target value.]]

.center[![:scale 70%](../assets/images/slides/crystals/bg_distr_4.png)]

---

## Results
### Restricted sampling

We restrict the sampling space at sampling time:

- A: The composition is restricted to only elements Fe and O, with a maximum of 10 atoms per element.
- B: We sample in the ternary space for Li-Mn-O, keeping the element count to maximum 16 atoms.
- C: We restrict the space groups to only cubic lattices.
- D: We restrict the range of the lattice parameters to lengths between 10 and 20 angstroms and angles between 75 and 135 degrees.

---

## Results
### Restricted sampling

.center[![:scale 70%](../assets/images/slides/crystals/distributions_restricted_sampling.png)]

---

## Results
### Diversity

.context[.highlight2[Diversity] is key in materials discovery.]

Analysis of 10,000 sampled crystals and the top-100 with lowest formation energy.

--

- All 10,000 samples are unique.

--
- All crystal systems, lattice systems and point symmetries found in the 10,000 samples.
    - 4 out of 8 crystal-lattice systems in the top-100.
    - 4 out of the 5 point symmetries in the top-100.
--
- All 22 elements found in the 10,000 samples.
    - 15 out of 22 elements in the top-100.
--
- 73 out of 113 space groups (65 %) found in the 10,000 samples
    - 19 out of 113 space groups in the top-100.

--

.conclusion[Crystal-GFN samples are highly diverse.]

---

name: title
class: title, middle

## Summary and conclusions

.center[![:scale 30%](../assets/images/slides/misc/conclusion.png)]

---

## Summary and conclusions

* Instead of generating crystal structures in the space of atomic positions, we proposed to sample in a lower-dimensional space of crystallographic properties (space group, composition and lattice parameters).
* This representation allowed us to introduce .highlight1[physicochemical and structural _hard_ constraints], reducing the search space and improving validity.
    * Crystal-GFN was trained in 30 hours in a CPU-only machine.
* Our results show that we can generate .highlight1[diverse, high scoring samples with the desired constraints].
* We can flexibly select the desired material characteristics.
* The .highlight1[framework can be flexibly extended] with more constraints, crystal structure descriptors (atomic positions) and other properties. 

.references[
* Mila AI4Science et al. [Crystal-GFN: sampling crystals with desirable properties and constraints](https://arxiv.org/abs/2310.04925). AI4Mat, NeurIPS 2023 (spotlight).
]

.highlight2[Open source code]: [github.com/alexhernandezgarcia/gflownet](https://github.com/alexhernandezgarcia/gflownet)

---

## Acknowledgements
### Amazing co-authors and friends

.left-column[
* Alexandre Duval
* Alexandra Volokhova
* Pierre Luc Carrier
* Divya Sharma
* Victor Schmidt
* Pierre Paul de Breuck
* Yasmine Benabed
* Michał Koziarski
* Yoshua Bengio
]

---

name: title
class: title, middle

![:scale 50%](../assets/images/slides/materials/crystals.png)

Mila AI4Science: Alex Hernandez-Garcia, Alexandre Duval, Alexandra Volokhova, Yoshua Bengio, Divya Sharma, Pierre Luc Carrier, Yasmine Benabed, Michał Koziarski, Victor Schmidt, Pierre-Paul De Breuck

[github.com/alexhernandezgarcia/gflownet](https://github.com/alexhernandezgarcia/gflownet)

.center[
<a href="https://mila.quebec/"><img src="../assets/images/slides/logos/mila-beige.png" alt="Mila" style="height: 3em"></a>
&nbsp&nbsp&nbsp&nbsp
<a href="https://www.umontreal.ca/"><img src="../assets/images/slides/logos/udem-white.png" alt="UdeM" style="height: 3em"></a>
]

.footer[[alexhernandezgarcia.github.io](https://alexhernandezgarcia.github.io/) | [alex.hernandez-garcia@mila.quebec](mailto:alex.hernandez-garcia@mila.quebec)]<br>
.footer[[@alexhg@scholar.social](https://scholar.social/@alexhg) [![:scale 1em](../assets/images/slides/misc/mastodon.png)](https://scholar.social/@alexhg) | [@alexhdezgcia](https://twitter.com/alexhdezgcia) [![:scale 1em](../assets/images/slides/misc/twitter.png)](https://twitter.com/alexhdezgcia)]


