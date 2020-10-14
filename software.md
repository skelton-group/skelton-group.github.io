---
title: Software
---

## Software
----------

We develop and maintain several open-source package on the [group GitHub](https://github.com/skelton-group).
(Some codes are in the process of being moved across from [Jonathan's personal GitHub](https://github.com/JMSkelton).)

<br>

### Phono3py-Power-Tools
----------

A collection of tools for Phono(3)py "power users".
This package is under active development, but currently includes:

* A tool for importing phonon calculations in unsupported codes into Phonopy.
* Command-line scripts for selecting and converting supercells in Phonopy calculations.
* Command-line scripts for analysing thermal-conductivity calculations performed with Phono3py.

Link: [https://github.com/skelton-group/Phono3py-Power-Tools](https://github.com/skelton-group/Phono3py-Power-Tools)

<br>

### Phonopy-Spectroscopy
----------

Scripts for simulating experimental spectra using Phono(3)py.
Phonopy-Spectroscopy currently has command-line scripts for generating infrared (IR) and Raman spectra, and we plan to add code for inelastic X-ray and neutron scattering (IXS/INS) in the future.
Current functionality includes:

* Generate IR spectra from Phonopy `mesh.yaml` and `BORN` files.
* Set up and post-process Raman calculations, including the calculation of full Raman tensors for simulating polarised spectra.
* Assignment of modes to irreducible representations (Mulliken symbols) using Phonopy.
* Simulating spectra with the intrinsic linewidths obtained from a Phono3py calculation.

Link: [https://github.com/JMSkelton/Phonopy-Spectroscopy](https://github.com/JMSkelton/Phonopy-Spectroscopy)

<br>

### ModeMap
----------

Scripts for mapping the potential-energy surface (PES) along phonon modes, usually used to investigate imaginary modes.
The main code can currently generate 1D and 2D maps (i.e. mapping along two modes simultaneously).
The package also includes a code for solving the Schr&ouml;dinger equation for a 1D PES, which can be used to calculate effective harmonic renormalised frequencies for anharmonic modes.

Link: [https://github.com/JMSkelton/ModeMap](https://github.com/JMSkelton/ModeMap)

<br>

### Transformer
----------

A Python library for manipulating crystal structures, including the ability to enumerate symmetry-unique atomic substitutions (e.g. to build defect or alloy models) and to perform various structural analyses including pair-distrution functions (PDFs).
Transformer is currently being refactored to improve the API and to make the underlying Python objects more efficient.

Link: [https://github.com/JMSkelton/Transformer](https://github.com/JMSkelton/Transformer)

<br>
