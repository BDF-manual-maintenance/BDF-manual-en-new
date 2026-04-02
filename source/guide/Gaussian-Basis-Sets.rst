Gaussian Basis Sets
================================================

To solve Hartree-Fock or Kohn-Sham DFT equations, molecular orbitals are expanded as linear combinations of atomic orbitals (basis functions) as:

.. math::
    \varphi_{i}(r) = C_{1,i}\chi_{1}(r) + C_{2,i}\chi_{2}(r) + C_{3,i}\chi_{3}(r) + \dots + C_{N,i}\chi_{N}(r)

In quantum chemistry calculations, basis functions are mathematical constructs without physical meaning. More basis functions yield higher accuracy, but depend on proper selection. With infinite basis functions (complete basis set limit, CBS), molecular orbitals can be perfectly expanded. Practical calculations use finite basis sets, introducing basis set incompleteness error.

Common quantum chemistry basis functions:

#. **Gaussian Type Orbitals (GTOs)**: Used by most quantum chemistry programs due to computational efficiency for two-electron integrals.
#. **Slater Type Orbitals (STOs)**: Used in semi-empirical methods and programs like ADF. More accurate radial behavior than GTOs but harder to compute.
#. **Plane Waves**: Primarily for periodic systems, less efficient for isolated molecules.
#. **Numerical Atomic Orbitals (NAOs)**: Supported by few programs (e.g., Dmol3, Siesta), defined discretely without analytical form.

BDF initially used STOs but now primarily uses GTOs.

For angular momentum *L* > *p* (e.g., *d*, *f*), GTOs have two representations:
1. **Cartesian functions**:
   .. math::
      N x^{lx} y^{ly} z^{lz} {\rm exp}(-\alpha r^2),  \qquad L=lx+ly+lz
   With :math:`(L+1)(L+2)/2` components (e.g., *d*: xx, yy, zz, xy, xz, yz).
2. **Spherical harmonics (pure functions)**:
   .. math::
      N Y^L_m r^L {\rm exp}(-\alpha r^2)
   With :math:`2L+1` components (e.g., *d*: -2, -1, 0, +1, +2).

Cartesian functions are easy to be used in integral calculations but are redundant. Spherical harmonics correspond directly to anglular momentum quantum numbers, so integrals are usually computed in Cartesian form then transformed to Spherical form:cite:`schlegel1995`.

.. attention::
  1. Most modern basis sets are optimized for spherical harmonics.
  2. Spherical harmonics offer better accuracy and numerical stability (especially in relativistic calculations), so BDF uses them exclusively.
  3. Results differ between Cartesian and spherical basis functions. Reproducing BDF results requires matching structure, method, basis set, and spherical basis usage.

Various optimized GTO basis functions for different applications have been compiled into a dataset in many publications,
and named for use in quantum chemistry programs. These pre-optimized GTO basis function datasets are referred to as
**Gaussian Basis Sets**. For a general introduction to Gaussian Basis Sets, see:
J. G. Hill, Gaussian Basis Sets for Molecular Applications, Int. J. Quant. Chem. 113, 21-34 (2013).
https://doi.org/10.1002/qua.24355
(Free Access)

The built-in Gaussian basis sets in BDF mainly come from the following websites, and the original literature
for various basis sets can be found therein.

* Basis Set Exchange :cite:`bse2019` : all-electron basis sets and scalar ECP basis sets in BDF format
  (Note: ECP basis set requires manual adjustment of the position of ECP data). https://www.basissetexchange.org/
* Stuttgart/Cologne pseudo-potential basis set library: mainly consists of SOECP basis sets and "f-in-core" basis sets,
  as well as a few early developed scalar ECP basis sets. http://www.tc.uni-koeln.de/PP/clickpse.en.html
* Turbomole basis set library: all-electron basis sets, scalar ECP basis sets, and SOECP basis sets. https://basissets.turbomole.org/
* Dyall's relativistic basis set: all-electron relativistic basis set. https://zenodo.org/records/7574629
* Sapporo basis set library: all-electron basis sets. http://sapporo.center.ims.ac.jp/sapporo/
* Clarkson University ECP basis set library: SOECP basis set. https://people.clarkson.edu/~pchristi/reps.html (expired)
* ccECP basis set library: scalar ECP basis sets; SOECP basis sets after Kr. https://pseudopotentiallibrary.org/
* ccRepo basis set library: it contains the latest developed correlation-consistent basis sets, including all-electron non-relativistic,
  all-electron relativistic, SOECP basis sets (without displaying ECP parameters), and various other types
  such as density fitting. http://www.grant-hill.group.shef.ac.uk/ccrepo/

In addition, some built-in basis sets are taken from the original publications:

* all-electron basis sets Dirac-RPF-4Z and Dirac-aug-RPF-4Z, supporting s- and p-block elements :cite:`dasilva2014`,
  d-block elements :cite:`dasilva2014a`, and f-block elements :cite:`dasilva2017`
* all-electron DKH2 contracted basis sets (aug-)cc-p(w)CVnZ-DK for Ga-Kr are not included in Basis Set Exchange,
  taken from the original publication :cite:`peterson2007`
* Pitzer's pseudopotential basis sets Pitzer-AVDZ-PP, Pitzer-VDZ-PP, and Pitzer-VTZ-PP :cite:`pitzer2000`
* Ce - Lu :cite:`ermler1994`, Fr - Pu :cite:`ermler1991`, and Am - Og :cite:`ermler1997,ermler1999`
  in the pseudopotential basis set CRENBL (Note: The Am - Og basis sets in Basis Set Exchange are incorrect!)
* Am - Og :cite:`ermler1997,ermler1999` in the pseudopotential basis set CRENBS (Note: The Am - Og basis sets in
  Basis Set Exchange are incorrect!)
* Ac, Th, Pa :cite:`dolg2014`, and U :cite:`dolg2009` in the pseudopotential basis set Stuttgart-ECPMDFSO-QZVP

BDF users can use standard basis sets or customed basis sets.

.. _all-e-bas:

All-Electron Basis Sets
------------------------------------------------

The all-electron basis sets can be classified as non-contracted basis sets and contracted basis sets.
The former can be used for both non-relativistic and relativistic calculations, but is primarily used
for relativistic calculations, while the latter is further divided into non-relativistically
contracted basis sets and relativistically contracted basis sets.

All-electron relativistic calculations require Hamiltonians that consider relativistic effects,
such as DKH, ZORA, and X2C (see :ref:`Relativistic Effects<relativity>`). In this case,
it is necessary to use contracted basis sets specifically optimized for relativistic calculations,
such as the cc-pVnZ-DK series, SARC, ANO-RCC, and so on. In BDF, only the scalar X2C relativistic Hamiltonian
is currently retained, which can be combined with X2C relativistic basis sets, DKH3 relativistic basis sets,
or DKH2 relativistic basis sets (for atoms up to 5d). For atoms below 3d, all-electron non-relativistic basis sets can also be used.

Most relativistic contracted basis sets treat the atomic nucleus as a point charge; however, some basis sets take into account
the size effect of the nuclear charge distribution when performing contractions, which has the most significant impact on
the contraction factors of *s* and *p* basis functions. Accordingly, a finite nuclear model must also be adopted
in the calculation of molecular integrals (see: :ref:`finite nuclear models<finite-nuclear>`).

Standard basis sets are mostly optimized for the properties of valence electrons and semi-core electrons,
making them unsuitable for accurately describing the electron distribution near the atomic nucleus.
For calculations involving nuclear properties, specially optimized basis sets are required
(see: :ref:`Mössbauer spectroscopy<mossbauer>`). For example, for the Mössbauer spectrum calculation of Fe,
we modified the standard x2c-TZVPPall basis set to obtain a basis set specifically for calculating
effective contact density, x2c-TZVPPall-CD, for calculating electric field gradients and
nuclear quadrupole splittings, x2c-TZVPPall-EFG, as well as x2c-TZVPPall-CDEFG, which calculates both simultaneously.

.. table:: Standard All-Electron Basis Sets in BDF
    :widths: auto
    :class: longtable

    +------------------------+-----------------------------+----------------------------------------+------------------------+
    | Basis Set Type         | Basis Set Name              | Supported Elements                     | Remarks                |
    +========================+=============================+========================================+========================+
    | Pople                  | | STO-3G                    | 1- 54                                  |                        |
    |                        | | STO-6G                    |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | 3-21G                     | 1- 55                                  |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | 3-21++G                   | 1,  3- 20                              |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | 6-31G                     | 1- 36                                  |                        |
    |                        | | 6-31G(d,p)                |                                        |                        |
    |                        | | 6-31GP                    |                                        |                        |
    |                        | | 6-31GPP                   |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | 6-31++G                   | 1- 20                                  |                        |
    |                        | | 6-31++GP                  |                                        |                        |
    |                        | | 6-31++GPP                 |                                        |                        |
    |                        | | 6-31+G                    |                                        |                        |
    |                        | | 6-31+GP                   |                                        |                        |
    |                        | | 6-31+GPP                  |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | 6-31G(2df,p)              | 1- 18                                  |                        |
    |                        | | 6-31G(3df,3pd)            |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | 6-311++G                  | 1,  3- 20                              |                        |
    |                        | | 6-311++G(2d,2p)           |                                        |                        |
    |                        | | 6-311++GP                 |                                        |                        |
    |                        | | 6-311++GPP                |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | 6-311+G                   | 1- 20                                  |                        |
    |                        | | 6-311+G(2d,p)             |                                        |                        |
    |                        | | 6-311+GP                  |                                        |                        |
    |                        | | 6-311+GPP                 |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | 6-311G                    | 1- 20, 31- 36, 53                      |                        |
    |                        | | 6-311G(d,p)               |                                        |                        |
    |                        | | 6-311GP                   |                                        |                        |
    |                        | | 6-311GPP                  |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | 6-31++GPP-J               | 1,  6- 8                               |                        |
    |                        | | 6-31+GP-J                 |                                        |                        |
    |                        | | 6-31G-J                   |                                        |                        |
    |                        | | 6-311++GPP-J              |                                        |                        |
    |                        | | 6-311+GP-J                |                                        |                        |
    |                        | | 6-311G-J                  |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | 6-311G(2df,2pd)           | 1- 10, 19- 20                          |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | 6-311++G(3df,3pd)         | 1, 3- 18                               |                        |
    +------------------------+-----------------------------+----------------------------------------+------------------------+
    | Correlation Consistent | | aug-cc-pVDZ               | | D: 1- 18, 19- 36                     |                        |
    |                        | | aug-cc-pVTZ               | | T: 1- 18, 19- 36                     |                        |
    |                        | | aug-cc-pVQZ               | | Q: 1- 18, 19- 36                     |                        |
    |                        | | aug-cc-pV5Z               | | 5: 1- 18, 21- 36                     |                        |
    |                        | | aug-cc-pV6Z               | | 6: 1-  2,  5- 10, 13- 18             |                        |
    |                        | | aug-cc-pV7Z               | | 7: 1-  2,  5- 10, 13- 17             |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | cc-pVDZ                   | | D: 1- 18, 19- 36                     |                        |
    |                        | | cc-pVTZ                   | | T: 1- 18, 19- 36                     |                        |
    |                        | | cc-pVQZ                   | | Q: 1- 18, 19- 36                     |                        |
    |                        | | cc-pV5Z                   | | 5: 1- 18, 20- 36                     |                        |
    |                        | | cc-pV6Z                   | | 6: 1-  2,  4- 10, 13- 18             |                        |
    |                        | | cc-pV7Z                   | | 7: 1-  2,  5- 10, 13- 18             |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | aug-cc-pCVDZ              | | D: 1- 18, 31-36                      |                        |
    |                        | | aug-cc-pCVTZ              | | T: 1- 18, 31-36                      |                        |
    |                        | | aug-cc-pCVQZ              | | Q: 1- 18, 31-36                      |                        |
    |                        | | aug-cc-pCV5Z              | | 5: 3- 18, 31-36                      |                        |
    |                        | | aug-cc-pCV6Z              | | 6: 5- 10, 13-18                      |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | cc-pCVDZ                  | | D: 1- 18, 20, 31-36                  |                        |
    |                        | | cc-pCVTZ                  | | T: 1- 18, 20, 31-36                  |                        |
    |                        | | cc-pCVQZ                  | | Q: 1- 18, 20, 31-36                  |                        |
    |                        | | cc-pCV5Z                  | | 5: 3- 18, 31-36                      |                        |
    |                        | | cc-pCV6Z                  | | 6: 5- 10, 13-18                      |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | aug-cc-pV(D+d)Z           | 1- 18, 21- 36                          |                        |
    |                        | | aug-cc-pV(T+d)Z           |                                        |                        |
    |                        | | aug-cc-pV(Q+d)Z           |                                        |                        |
    |                        | | aug-cc-pV(5+d)Z           |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | cc-pV(D+d)Z               | 1- 18, 20- 36                          |                        |
    |                        | | cc-pV(T+d)Z               |                                        |                        |
    |                        | | cc-pV(Q+d)Z               |                                        |                        |
    |                        | | cc-pV(5+d)Z               |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | aug-cc-pwCVDZ             | | D: 3- 20, 31- 36                     |                        |
    |                        | | aug-cc-pwCVTZ             | | T: 3- 36                             |                        |
    |                        | | aug-cc-pwCVQZ             | | Q: 3- 36                             |                        |
    |                        | | aug-cc-pwCV5Z             | | 5: 3- 18, 21- 36                     |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | cc-pwCVDZ                 | | D: 3- 20, 31- 36                     |                        |
    |                        | | cc-pwCVTZ                 | | T: 3- 36                             |                        |
    |                        | | cc-pwCVQZ                 | | Q: 3- 36                             |                        |
    |                        | | cc-pwCV5Z                 | | 5: 3- 18, 21- 36                     |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | aug-cc-pVDZ-RIFIT         | 1-  2,  4- 10, 12- 18, 21- 36          | Auxiliary Basis Set    |
    |                        | | aug-cc-pVTZ-RIFIT         |                                        |                        |
    |                        | | aug-cc-pVQZ-RIFIT         |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | aug-cc-pV5Z-RIFIT         | | 5: 1- 10, 13- 18, 21- 36             | Auxiliary Basis Set    |
    |                        | | aug-cc-pV6Z-RIFIT         | | 6: 1-  2,  5- 10, 13- 18             |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | aug-cc-pVTZ-J             | 1,  5-  9, 13- 17, 21- 30, 34          | Auxiliary Basis Set    |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | aug-cc-pVDZ-DK            | | D: 1- 38                             | DKH2 Relativistic      |
    |                        | | aug-cc-pVTZ-DK            | | T: 1- 54, 72- 86                     |                        |
    |                        | | aug-cc-pVQZ-DK            | | Q: 1- 38, 49- 54                     |                        |
    |                        | | aug-cc-pV5Z-DK            | | 5: 1-  2,  5- 18, 21- 36             |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | aug-cc-pCVDZ-DK           | 3- 18, 31- 36                          | DKH2 Relativistic      |
    |                        | | aug-cc-pCVTZ-DK           |                                        |                        |
    |                        | | aug-cc-pCVQZ-DK           |                                        |                        |
    |                        | | aug-cc-pCV5Z-DK           |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | aug-cc-pwCVDZ-DK          | | D: 3- 20, 31- 38                     | DKH2 Relativistic      |
    |                        | | aug-cc-pwCVTZ-DK          | | T: 3- 54, 72- 86                     |                        |
    |                        | | aug-cc-pwCVQZ-DK          | | Q: 3- 38, 49- 54, 81- 86             |                        |
    |                        | | aug-cc-pwCV5Z-DK          | | 5: 3- 18, 21- 36                     |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | aug-cc-pVDZ-DK3           | | D: 55- 56, 78, 79, 87- 88            | DKH3 Relativistic      |
    |                        | | aug-cc-pVTZ-DK3           | | T: 49- 56, 72- 88                    |                        |
    |                        | | aug-cc-pVQZ-DK3           | | Q: 49- 56, 78, 79, 81- 88            |                        |
    |                        | | aug-cc-pwCVDZ-DK3         |                                        |                        |
    |                        | | aug-cc-pwCVTZ-DK3         |                                        |                        |
    |                        | | aug-cc-pwCVQZ-DK3         |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | aug-cc-pCVDZ-X2C          | 5- 10, 13- 18                          | X2C Relativistic       |
    |                        | | aug-cc-pCVTZ-X2C          |                                        |                        |
    |                        | | aug-cc-pCVQZ-X2C          |                                        |                        |
    |                        | | aug-cc-pCV5Z-X2C          |                                        |                        |
    |                        | | aug-cc-pCV6Z-X2C          |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | aug-cc-pVDZ-X2C           | | 1- 2, 5- 10, 13- 20, 31- 38, 55- 56, | X2C Relativistic       |
    |                        | | aug-cc-pVTZ-X2C           | | 87- 88                               |                        |
    |                        | | aug-cc-pVQZ-X2C           |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | aug-cc-pV5Z-X2C           | 1- 2, 5- 10, 13- 18, 31- 36            | X2C Relativistic       |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | aug-cc-pV6Z-X2C           | 1- 2, 5- 10, 13- 18                    |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | aug-cc-pwCVDZ-X2C         | 5- 10, 13- 20, 31- 38, 55- 56, 87- 88  | X2C Relativistic       |
    |                        | | aug-cc-pwCVTZ-X2C         |                                        |                        |
    |                        | | aug-cc-pwCVQZ-X2C         |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | aug-cc-pwCV5Z-X2C         | 5- 10, 13- 18, 31- 36                  | X2C Relativistic       |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | cc-pVDZ-DK                | | D: 1- 38                             | DKH2 Relativistic      |
    |                        | | cc-pVTZ-DK                | | T: 1- 54, 72- 86                     |                        |
    |                        | | cc-pVQZ-DK                | | Q: 1- 38, 49- 54                     |                        |
    |                        | | cc-pV5Z-DK                | | 5: 1- 18, 21- 36                     |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | cc-pCVDZ-DK               | 3- 18, 31-36                           | DKH2 Relativistic      |
    |                        | | cc-pCVTZ-DK               |                                        |                        |
    |                        | | cc-pCVQZ-DK               |                                        |                        |
    |                        | | cc-pCV5Z-DK               |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | cc-pwCVDZ-DK              | | D: 3- 20, 31- 38                     | DKH2 Relativistic      |
    |                        | | cc-pwCVTZ-DK              | | T: 3- 54, 72- 86                     |                        |
    |                        | | cc-pwCVQZ-DK              | | Q: 3- 38, 49- 54, 81- 86             |                        |
    |                        | | cc-pwCV5Z-DK              | | 5: 3- 18, 21- 36                     |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | cc-pVDZ-DK3               | | D: 55- 71, 78, 79, 87-103            | DKH3 Relativistic      |
    |                        | | cc-pVTZ-DK3               | | T: 49-103                            |                        |
    |                        | | cc-pVQZ-DK3               | | Q: 49- 71, 78, 79, 81-103            |                        |
    |                        | | cc-pwCVDZ-DK3             |                                        |                        |
    |                        | | cc-pwCVTZ-DK3             |                                        |                        |
    |                        | | cc-pwCVQZ-DK3             |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | cc-pCVDZ-X2C              | 5- 10, 13- 18                          | X2C Relativistic       |
    |                        | | cc-pCVTZ-X2C              |                                        |                        |
    |                        | | cc-pCVQZ-X2C              |                                        |                        |
    |                        | | cc-pCV5Z-X2C              |                                        |                        |
    |                        | | cc-pCV6Z-X2C              |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | cc-pVDZ-X2C               | | 1- 2, 5- 10, 13- 20, 31- 38, 55- 71, | X2C Relativistic       |
    |                        | | cc-pVTZ-X2C               | | 87- 103                              |                        |
    |                        | | cc-pVQZ-X2C               |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | cc-pV5Z-X2C               | 1- 2, 5- 10, 13- 18, 31- 36            | X2C Relativistic       |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | cc-pV6Z-X2C               | 1- 2, 5- 10, 13- 18                    |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | cc-pwCVDZ-X2C             | | 5- 10, 13- 20, 31- 38, 55- 71,       | X2C Relativistic       |
    |                        | | cc-pwCVTZ-X2C             | | 87- 103                              |                        |
    |                        | | cc-pwCVQZ-X2C             |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | cc-pwCV5Z-X2C             | 5- 10, 13- 18, 31- 36                  | X2C Relativistic       |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | cc-pVDZ-FW_fi             | 1-2, 5-10, 13-18, 31-36                | | NESC Relativistic    |
    |                        | | cc-pVTZ-FW_fi             |                                        | | Finite Nucleus       |
    |                        | | cc-pVQZ-FW_fi             |                                        |                        |
    |                        | | cc-pV5Z-FW_fi             |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | cc-pVDZ-FW_pt             | 1-2,  5-10, 13-18, 31-36               | NESC Relativistic      |
    |                        | | cc-pVTZ-FW_pt             |                                        |                        |
    |                        | | cc-pVQZ-FW_pt             |                                        |                        |
    |                        | | cc-pV5Z-FW_pt             |                                        |                        |
    +------------------------+-----------------------------+----------------------------------------+------------------------+
    | ANO                    | | ADZP-ANO                  | 1-103                                  |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | ANO-DK3                   | 1- 10                                  | DKH3 Relativistic      |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | ANO-R                     | | 1- 86                                | | X2C Relativistic     |
    |                        | | ANO-R0                    | | R: full; R0: MB;                     | | Finite Nucleus       |
    |                        | | ANO-R1                    | | R1: VDZP; R2: VTZP;                  | | 2021 revised version |
    |                        | | ANO-R2                    | | R3: VQZP                             | | -old for 2020 version|
    |                        | | ANO-R3                    |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | ANO-RCC                   | 1- 96                                  | DKH2 Relativistic      |
    |                        | | ANO-RCC-VDZ               |                                        |                        |
    |                        | | ANO-RCC-VDZP              |                                        |                        |
    |                        | | ANO-RCC-VTZP              |                                        |                        |
    |                        | | ANO-RCC-VQZP              |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | ANO-RCC-VTZ               | 3- 20, 31- 38                          | DKH2 Relativistic      |
    +------------------------+-----------------------------+----------------------------------------+------------------------+
    | Ahlrichs               | | Def2系列                  | All-e. NR and PP mixed, see :ref:`PP Basis Sets<ecp-bas>`       |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | jorge-DZP                 | | D: 1-103                             |                        |
    |                        | | jorge-TZP                 | | T: 1-103                             |                        |
    |                        | | jorge-QZP                 | | Q: 1- 54                             |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | jorge-DZP-DKH             | | D: 1-103                             | | DKH2 Relativistic    |
    |                        | | jorge-TZP-DKH             | | T: 1-103                             | | Finite Nucleus       |
    |                        | | jorge-QZP-DKH             | | Q: 1- 54                             |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | SARC-DKH2                 | 57- 86, 89-103                         | DKH2 Relativistic      |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | SARC2-QZV-DKH2            | 57- 71                                 | DKH2 Relativistic      |
    |                        | | SARC2-QZVP-DKH2           |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | x2c-SV(P)all              | 1- 86                                  | | X2C Relativistic     |
    |                        | | x2c-SVPall                |                                        | | Finite Nucleus       |
    |                        | | x2c-TZVPall               |                                        |                        |
    |                        | | x2c-TZVPPall              |                                        |                        |
    |                        | | x2c-QZVPall               |                                        |                        |
    |                        | | x2c-QZVPPall              |                                        |                        |
    |                        | | x2c-SV(P)all-2c           |                                        |                        |
    |                        | | x2c-SVPall-2c             |                                        |                        |
    |                        | | x2c-TZVPall-2c            |                                        |                        |
    |                        | | x2c-TZVPPall-2c           |                                        |                        |
    |                        | | x2c-QZVPall-2c            |                                        |                        |
    |                        | | x2c-QZVPPall-2c           |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | x2c-TZVPall-f             | 1- 20                                  | | X2C Relativistic     |
    |                        | | x2c-TZVPPall-f            |                                        | | Finite Nucleus       |
    +------------------------+-----------------------------+----------------------------------------+------------------------+
    | Sapporo                | | Sapporo-DZP               | 1- 54                                  | 2012 is the new version|
    |                        | | Sapporo-TZP               |                                        |                        |
    |                        | | Sapporo-QZP               |                                        |                        |
    |                        | | Sapporo-DZP-2012          |                                        |                        |
    |                        | | Sapporo-TZP-2012          |                                        |                        |
    |                        | | Sapporo-QZP-2012          |                                        |                        |
    |                        | | Sapporo-DZP-dif           |                                        |                        |
    |                        | | Sapporo-TZP-dif           |                                        |                        |
    |                        | | Sapporo-QZP-dif           |                                        |                        |
    |                        | | Sapporo-DZP-2012-dif      |                                        |                        |
    |                        | | Sapporo-TZP-2012-dif      |                                        |                        |
    |                        | | Sapporo-QZP-2012-dif      |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | Sapporo-DKH3-DZP          | 1- 54                                  | DKH3 Relativistic      |
    |                        | | Sapporo-DKH3-TZP          |                                        |                        |
    |                        | | Sapporo-DKH3-QZP          |                                        |                        |
    |                        | | Sapporo-DKH3-DZP-dif      |                                        |                        |
    |                        | | Sapporo-DKH3-TZP-dif      |                                        |                        |
    |                        | | Sapporo-DKH3-QZP-dif      |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | Sapporo-DKH3-DZP-2012     | 19- 86                                 | | DKH3 Relativistic    |
    |                        | | Sapporo-DKH3-TZP-2012     |                                        | | Finite Nucleus       |
    |                        | | Sapporo-DKH3-QZP-2012     |                                        |                        |
    |                        | | Sapporo-DKH3-DZP-2012-dif |                                        |                        |
    |                        | | Sapporo-DKH3-TZP-2012-dif |                                        |                        |
    |                        | | Sapporo-DKH3-QZP-2012-dif |                                        |                        |
    +------------------------+-----------------------------+----------------------------------------+------------------------+
    | Uncontracted           | | UGBS                      | 1- 90, 94- 95, 98-103                  | R/NR universal         |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | Dirac-RPF-4Z              | 1-118                                  | R/NR universal         |
    |                        | | Dirac-aug-RPF-4Z          |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | Dirac-Dyall.2zp           | 1-118                                  | R/NR universal         |
    |                        | | Dirac-Dyall.3zp           |                                        |                        |
    |                        | | Dirac-Dyall.4zp           |                                        |                        |
    |                        | | Dirac-Dyall.ae2z          |                                        |                        |
    |                        | | Dirac-Dyall.ae3z          |                                        |                        |
    |                        | | Dirac-Dyall.ae4z          |                                        |                        |
    |                        | | Dirac-Dyall.cv2z          |                                        |                        |
    |                        | | Dirac-Dyall.cv3z          |                                        |                        |
    |                        | | Dirac-Dyall.cv4z          |                                        |                        |
    |                        | | Dirac-Dyall.v2z           |                                        |                        |
    |                        | | Dirac-Dyall.v3z           |                                        |                        |
    |                        | | Dirac-Dyall.v4z           |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | Dirac-Dyall.aae2z         | | 1-56, 72-88, 104-118                 | R/NR universal         |
    |                        | | Dirac-Dyall.aae3z         |                                        |                        |
    |                        | | Dirac-Dyall.aae4z         |                                        |                        |
    |                        | | Dirac-Dyall.acv2z         |                                        |                        |
    |                        | | Dirac-Dyall.acv3z         |                                        |                        |
    |                        | | Dirac-Dyall.acv4z         |                                        |                        |
    |                        | | Dirac-Dyall.av2z          |                                        |                        |
    |                        | | Dirac-Dyall.av3z          |                                        |                        |
    |                        | | Dirac-Dyall.av4z          |                                        |                        |
    +------------------------+-----------------------------+----------------------------------------+------------------------+
    | Others                 | | SVP-BSEX                  | 1, 3-10                                |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | DZP                       | 1, 6-8, 16, 26, 42                     |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | DZVP                      | 1, 3-9, 11-17, 19-20, 31-35, 49-53     |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | TZVPP                     | 1, 6-7                                 |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | IGLO-II                   | 1,  5-  9, 13- 17                      |                        |
    |                        | | IGLO-III                  |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | Sadlej-pVTZ               | 1,  6- 8                               |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | Wachters+f                | 21- 29                                 |                        |
    +------------------------+-----------------------------+----------------------------------------+------------------------+


.. _ecp-bas:

Pseudopotential Basis Sets
------------------------------------------------

Effective Core Potentials (ECPs) include Pseudopotentials (PP) and Model Core Potentials (MCP). 
In quantum chemistry calculations, PPs are fundamentally similar to those in plane-wave calculations but expressed in the analytical form. 
Most quantum chemistry software, including BDF, supports PPs, while fewer support MCPs. Therefore, the terms ECP and PP can be used interchangeably when unambiguous.

Pseudopotential basis sets are used in conjunction with pseudopotentials, with basis functions describing only the atom's semi-core and valence electrons.
For systems involving heavy atoms, pseudopotential basis sets can be applied to these atoms while other atoms use standard non-relativistic all-electron basis sets. 
This approach significantly reduces computation time while effectively incorporating scalar relativistic effects. Basis sets like the Lan series,
Stuttgart series, and cc-pVnZ-PP series belong to this category. For convenience, pseudopotential basis sets for lighter elements
are essentially non-relativistic all-electron basis sets, such as the Def2 series before the 5th period.

.. _soecp-bas:

Pseudopotential basis sets are categorized into **scalar pseudopotential basis sets** and **spin-orbit coupling pseudopotential (SOECP) basis sets**
based on whether they include spin-orbit coupling terms.

.. table:: Standard Pseudopotential Basis Sets in BDF
    :widths: auto
    :class: longtable

    +------------------------+-----------------------------+----------------------------------------+------------------------+
    | Basis Set Type         | Basis Set Name              | Supported Elements                     | Remarks                |
    +========================+=============================+========================================+========================+
    | Correlation-Consistent | | aug-cc-pVDZ-PP            | 19, 20, 29- 56, 72- 88                 | SOECP                  |
    |                        | | aug-cc-pVTZ-PP            |                                        |                        |
    |                        | | aug-cc-pVQZ-PP            |                                        |                        |
    |                        | | aug-cc-pV5Z-PP            |                                        |                        |
    |                        | | aug-cc-pwCVDZ-PP          |                                        |                        |
    |                        | | aug-cc-pwCVTZ-PP          |                                        |                        |
    |                        | | aug-cc-pwCVQZ-PP          |                                        |                        |
    |                        | | aug-cc-pwCV5Z-PP          |                                        |                        |
    |                        | | cc-pV5Z-PP                |                                        |                        |
    |                        | | cc-pwCV5Z-PP              |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | cc-pVDZ-PP                | 19, 20, 29- 56, 72- 88, 90- 92         | SOECP                  |
    |                        | | cc-pVTZ-PP                |                                        |                        |
    |                        | | cc-pVQZ-PP                |                                        |                        |
    |                        | | cc-pwCVDZ-PP              |                                        |                        |
    |                        | | cc-pwCVTZ-PP              |                                        |                        |
    |                        | | cc-pwCVQZ-PP              |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | aug-cc-pCVDZ-ccECP        | | D/T/Q: 19- 30, 37- 42, 44- 51,       | | SOECP (Z > 36);      |
    |                        | | aug-cc-pCVTZ-ccECP        | | 55- 58, 63- 65, 73- 75, 77- 79, 82   | | large core for some  |
    |                        | | aug-cc-pCVQZ-ccECP        | | 5: 19- 30, 37- 42, 44- 51,           | | main group elements  |
    |                        | | aug-cc-pCV5Z-ccECP        | | 55, 56, 73- 75, 77- 79, 82           |                        |
    |                        | | cc-pCVDZ-ccECP            |                                        |                        |
    |                        | | cc-pCVTZ-ccECP            |                                        |                        |
    |                        | | cc-pCVQZ-ccECP            |                                        |                        |
    |                        | | cc-pCV5Z-ccECP            |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | aug-cc-pVDZ-ccECP         | | D/T/Q: 3- 9, 11- 17, 19- 42, 44- 53, | | SOECP (Z > 36);      |
    |                        | | aug-cc-pVTZ-ccECP         | | 55-58, 63-65, 73-75, 77-79, 82, 83   | | large core for some  |
    |                        | | aug-cc-pVQZ-ccECP         | | 5: 3- 9, 11- 17, 19- 42, 44- 53,     | | main group elements  |
    |                        | | aug-cc-pV5Z-ccECP         | | 55, 56, 73-75, 77-79, 82, 83         |                        |
    |                        | | aug-cc-pV6Z-ccECP         | | 6: 4- 9, 12- 17, 19- 20, 31- 38,     |                        |
    |                        | |                           | | 50- 53, 83                           |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | cc-pVDZ-ccECP             | | D/T/Q: 3- 42, 44- 53, 55- 58, 63-65, | | SOECP (Z > 36);      |
    |                        | | cc-pVTZ-ccECP             | | 73- 75, 77- 79, 82, 83               | | large core for some  |
    |                        | | cc-pVQZ-ccECP             | | 5: 3- 42, 44- 53, 55, 56, 73- 75,    | | main group elements  |
    |                        | | cc-pV5Z-ccECP             | | 77- 79, 82, 83                       |                        |
    |                        | | cc-pV6Z-ccECP             | | 6: 4- 10, 12- 20, 31- 38, 50- 53, 83 |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | Pitzer-AVDZ-PP            | 3- 10                                  | SOECP                  |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | Pitzer-VDZ-PP             | 3- 18                                  | SOECP                  |
    |                        | | Pitzer-VTZ-PP             |                                        |                        |
    +------------------------+-----------------------------+----------------------------------------+------------------------+
    | Clarkson               | | CRENBL                    | 1-2 (all-electron), 3-118              | SOECP, small core      |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | CRENBS                    | | 21- 36, 39- 54, 57, 72- 86,          | SOECP, large core      |
    |                        |                             | | 104-118                              |                        |
    +------------------------+-----------------------------+----------------------------------------+------------------------+
    | Ahlrichs               | | Def2-SVP-old              | 1- 36 (all-electron), 37- 57, 72- 86   | | -old for old version;|
    |                        | | Def2-SV(P)-old            |                                        | | full PP parameters   |
    |                        | | Def2-SVPD-old             |                                        | | for suffix -G16      |
    |                        | | Def2-TZVP-old             |                                        |                        |
    |                        | | Def2-TZVPD-old            |                                        |                        |
    |                        | | Def2-TZVP-F-old           |                                        |                        |
    |                        | | Def2-TZVPP-F-old          |                                        |                        |
    |                        | | Def2-TZVPP-old            |                                        |                        |
    |                        | | Def2-TZVPPD-old           |                                        |                        |
    |                        | | Def2-QZVP-old             |                                        |                        |
    |                        | | Def2-QZVPD-old            |                                        |                        |
    |                        | | Def2-QZVPP-old            |                                        |                        |
    |                        | | Def2-QZVPPD-old           |                                        |                        |
    |                        | | Def2-SV(P)-G16            |                                        |                        |
    |                        | | Def2-SVP-G16              |                                        |                        |
    |                        | | Def2-TZVP-G16             |                                        |                        |
    |                        | | Def2-TZVPP-G16            |                                        |                        |
    |                        | | Def2-QZVP-G16             |                                        |                        |
    |                        | | Def2-QZVPP-G16            |                                        |                        |
    |                        | | Def2-SVPD                 |                                        |                        |
    |                        | | Def2-TZVPD                |                                        |                        |
    |                        | | Def2-TZVPPD               |                                        |                        |
    |                        | | Def2-QZVPD                |                                        |                        |
    |                        | | Def2-QZVPPD               |                                        |                        |
    |                        | | ma-Def2-SV(P)             |                                        |                        |
    |                        | | ma-Def2-SVP               |                                        |                        |
    |                        | | ma-Def2-TZVP              |                                        |                        |
    |                        | | ma-Def2-TZVPP             |                                        |                        |
    |                        | | ma-Def2-QZVP              |                                        |                        |
    |                        | | ma-Def2-QZVPP             |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | Def2-SV(P)                | 1- 36 (all-electron), 37- 86           |                        |
    |                        | | Def2-SVP                  |                                        |                        |
    |                        | | Def2-TZVP                 |                                        |                        |
    |                        | | Def2-TZVPP                |                                        |                        |
    |                        | | Def2-TZVP-f               |                                        |                        |
    |                        | | Def2-TZVPP-f              |                                        |                        |
    |                        | | Def2-QZVP                 |                                        |                        |
    |                        | | Def2-QZVPP                |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | DHF-SV(P)                 | 37- 56, 72- 86                         | SOECP                  |
    |                        | | DHF-SVP                   |                                        |                        |
    |                        | | DHF-TZVP                  |                                        |                        |
    |                        | | DHF-TZVPP                 |                                        |                        |
    |                        | | DHF-QZVP                  |                                        |                        |
    |                        | | DHF-QZVPP                 |                                        |                        |
    +------------------------+-----------------------------+----------------------------------------+------------------------+
    | LAN                    | | LANL2DZ                   | | 1, 3-10 (all-electron)               |                        |
    |                        |                             | | 11-57, 72-83, 92-94                  |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | LANL2DZDP                 | | 1, 6-9 (all-electron)                |                        |
    |                        |                             | | 14-17, 32-35, 50-53, 82-83           |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | LANL2TZ                   | 21- 30, 39- 48, 57, 72- 80             |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | LANL08                    | 11- 57, 72- 83                         |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | LANL08(D)                 | 14- 17, 32- 35, 50- 53, 82- 83         |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | LANL2TZ+                  | 21- 30                                 |                        |
    |                        | | LANL08+                   |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | Modified-LANL2DZ          | 21- 29, 39- 47, 57, 72- 79             |                        |
    |                        | | LANL2TZ(F)                |                                        |                        |
    |                        | | LANL08(F)                 |                                        |                        |
    +------------------------+-----------------------------+----------------------------------------+------------------------+
    | SBKJC                  | | SBKJC-VDZ                 | 1-2 (all-electron), 3- 58, 72- 86      |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | SBKJC-POLAR               | | 1-2 (all-electron)                   |                        |
    |                        |                             | | 3- 20, 32- 38, 50- 56, 82- 86        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | pSBKJC                    | 6- 9, 14- 17, 32- 35, 50- 53           |                        |
    +------------------------+-----------------------------+----------------------------------------+------------------------+
    | Stuttgart              | | Stuttgart-RLC             | | 3- 20, 30- 38, 49- 56, 80- 86        | large core             |
    |                        |                             | | 89-103                               |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | Stuttgart-RSC-1997        | | 19-30, 37-48, 55-56, 58-70           | small core             |
    |                        |                             | | 72-80, 89-103, 105                   |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | Stuttgart-RSC-ANO         | 57- 71, 89-103                         | SOECP, small core      |
    |                        | | Stuttgart-RSC-SEG         |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | Stuttgart-ECP92MDFQ-DZVP  | 111-120                                | SOECP, small core      |
    |                        | | Stuttgart-ECP92MDFQ-TZVP  |                                        |                        |
    |                        | | Stuttgart-ECP92MDFQ-QZVP  |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | Stuttgart-ECPMDFSO-QZVP   | 19- 20, 37- 38, 55- 56, 87- 92         | SOECP, small core      |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | SDB-cc-pVTZ               | 31- 36, 49- 54                         | large core             |
    |                        | | SDB-cc-pVQZ               |                                        |                        |
    +                        +-----------------------------+----------------------------------------+------------------------+
    |                        | | SDB-aug-cc-pVTZ           | 31- 35, 49- 53                         | large core             |
    |                        | | SDB-aug-cc-pVQZ           |                                        |                        |
    +------------------------+-----------------------------+----------------------------------------+------------------------+

.. _def2-problem:

.. note:: **Notes on Def2 Basis Sets**

    1. Def2 basis sets originally were developed for Turbomole. "Def2" stands for "the second default basis set".
    2. Original Def2 series (suffix **-old**) have some deficiencies which have been fixed in Turbomole 7.3+ versions:
       Added f polarization functions (and g functions in some QZ sets) for Ba;
       Reoptimized f/g functions for I atom in Def2-QZVPD/QZVPPD;
       Added missing f functions for Mn in Def2-QZVPPD;
       Added support for lanthanides.
    3. Def2 uses truncated Stuttgart/Cologne PPs for post-Kr elements, which causes 0.1-1 mHartree energy differences.
       Gaussian 16 uses the standard PPs (denoted by **-G16** suffix).
    4. Use Def2-... or Def2-...-G16 normally; DHF-... for SOECP calculations. To match Gaussian 16 results for post-Kr elements,
       use Def2-...-G16. Avoid Def2-...-old unless reproducing early Mn/I/Ba calculations.

.. _f-in-core:

For lanthanides and actinides, "f-in-core" (FIC) basis sets put f electrons into the pseudopotential.
BDF includes these FIC scalar pseudopotential basis sets optimized for common oxidation states,
with scalar relativistic effects accounted for through the Wood-Boring approximation (MWB) in reference data.

.. table:: FIC Pseudopotential Basis Sets in BDF
    :widths: auto
    :class: longtable

    +-----------------------------+------------------------+----------------------------------------+
    | Basis Set                   | Supported Elements     | Core Electrons                         |
    +=============================+========================+========================================+
    | | MWB-FIC                   | | 57- 71               | | [Kr](4d)10(4f)n                      |
    | | MWB-FIC-I                 |                        |                                        |
    | | MWB-FIC-II                |                        |                                        |
    +-----------------------------+------------------------+----------------------------------------+
    | | MWB-FIC-AVDZ              | | 89-103               | | [Xe](4f)14(5d)10(5f)n                |
    | | MWB-FIC-AVTZ              |                        |                                        |
    | | MWB-FIC-AVQZ              |                        |                                        |
    +-----------------------------+------------------------+----------------------------------------+
    | | MWB-FICp1                 | | 57- 70               | | [Kr](4d)10(4f)n+1                    |
    +-----------------------------+------------------------+----------------------------------------+
    | | MWB-FICp1-AVDZ            | | 94-102               | | [Xe](4f)14(5d)10(5f)n+1              |
    | | MWB-FICp1-AVTZ            |                        |                                        |
    | | MWB-FICp1-AVQZ            |                        |                                        |
    +-----------------------------+------------------------+----------------------------------------+
    | | MWB-FICm1-AVDZ            | | 58- 60, 65, 66,      | | [Kr](4d)10(4f)n-1,                   |
    | | MWB-FICm1-AVTZ            | | 90- 98               | | [Xe](4f)14(5d)10(5f)n-1              |
    | | MWB-FICm1-AVQZ            |                        |                                        |
    +-----------------------------+------------------------+----------------------------------------+
    | | MWB-FICm2-AVDZ            | | 91- 95               | | [Xe](4f)14(5d)10(5f)n-2              |
    | | MWB-FICm2-AVTZ            |                        |                                        |
    | | MWB-FICm2-AVQZ            |                        |                                        |
    +-----------------------------+------------------------+----------------------------------------+
    | | MWB-FICm3-AVDZ            | | 92                   | | [Xe](4f)14(5d)10(5f)n-3              |
    | | MWB-FICm3-AVTZ            |                        |                                        |
    | | MWB-FICm3-AVQZ            |                        |                                        |
    +-----------------------------+------------------------+----------------------------------------+


.. _alias-bas:

Aliases and Abbreviations for Standard Basis Sets
------------------------------------------------

In addition to the standard names mentioned above, some basis sets can also use their aliases and abbreviations.
The basic rules are as follows:

* Pople 6- series: Suffixes P/PP can be replaced with * (e.g., `6-311++G**` = `6-311++GPP`)
* Def2 series: Hyphens can be omitted (e.g., `def2-SVP` = `def2SVP`)
* Correlation-consistent: `cc-pVnZ` → `vnz` (e.g., `vdz` = `cc-pVDZ`); `aug-cc-pVnZ` → `avnz`
  (e.g., `avtz` = `aug-cc-pVTZ`); `aug-cc-pwCVnZ-DK` → `awcvtz-dk`.
  Use these abbreviations **only in BDF input files**, not in formal publications.

.. _SelfdefinedBasis:

Custom Basis Set Files
------------------------------------------------

BDF can use non-built-in basis sets in two ways. One method is to write the basis set data in the ``basis-block`` ... ``end basis``
section of the **COMPASS** input file, placing the basis set data in the ``inline`` ... ``end line`` data area (see the next subsection).
The other method is to save the basis set data in a text file placed in the calculation directory,
with the filename being the basis set name to be referenced in BDF.

.. warning::

    The filename of the custom basis set must be **ALL UPPERCASE**! However, when referenced in the input file,
    the case can be arbitrary.

For example, create a text file named MYBAS-1 in the calculation directory (note: if you create a text file on a Windows operating system,
the system may hide the extension *.txt*, so the actual name is MYBAS-1.txt) with the content as follows:

.. code-block::

   # This is my basis set No. 1.               # Any blank lines and comment lines starting with #
   # Supported elements: He and Al

   ****                                        # 4 asterisks at the beginning of the line, followed by a basis set.
   He      2    1                              # Element symbol, nuclear charge, highest angular momentum of GTOs: 1=p, 2=d...
   S      4    2                               # S-type GTO functions, 2 contracted functions from 4 primitive functions
                  3.836000E+01                 # 4 exponents of S-type primitive functions
                  5.770000E+00
                  1.240000E+00
                  2.976000E-01
         2.380900E-02           0.000000E+00   # Two column coefficients, corresponding to two contracted S-type functions.
         1.548910E-01           0.000000E+00
         4.699870E-01           0.000000E+00
         5.130270E-01           1.000000E+00
   P      2    2                               # P-type GTO functions, 2 contracted functions from 2 primitive functions
                  1.275000E+00
                  4.000000E-01
         1.0000000E+00           0.000000E+00
         0.0000000E+00           1.000000E+00
   ****                       # 4 asterisks end He basis set, which can be followed by another basis set, or end.
   Al     13    2
   (omitted)

In the above basis set, the P function is acturally not contracted, and can also be written in the following form:

.. code-block::

  (S-function, omitted)
  P      2    0              # 0 indicates non-contraction, and no contraction confficents required
                  1.275000E+00
                  4.000000E-01
   ****
   (omitted)


For pseudopotential basis sets, ECP data also needs to be provided after the valence basis function. For example

.. code-block::

   ****                                              # valence function part, see above
   Al     13    2
   S       4    3
              14.68000000
               0.86780000
               0.19280000
               0.06716000
       -0.0022368000     0.0000000000     0.0000000000
       -0.2615913000     0.0000000000     0.0000000000
        0.6106597000     0.0000000000     1.0000000000
        0.5651997000     1.0000000000     0.0000000000
   P       4    2
               6.00100000
               1.99200000
               0.19480000
               0.05655000
       -0.0034030000     0.0000000000
       -0.0192089000     0.0000000000
        0.4925534000    -0.2130858000
        0.6144261000     1.0000000000
   D       1    1
               0.19330000
        1.0000000000
   ECP                     # valence functions are immediately followed by the keyword ECP, indicating ECP data followed
   Al    10    2    2      # Same element symbol, number of core electron, ECP and optional SOECP highest angular momenta
   D potential  4                                    # The number of PP functions of the highest angular momentum (D)
      2      1.22110000000000     -0.53798100000000  # Power of R, exponent, and coeffients (the same below)
      2      3.36810000000000     -5.45975600000000
      2      9.75000000000000    -16.65534300000000
      1     29.26930000000000     -6.47521500000000
   S potential  5                                    # The number of S projections
      2      1.56310000000000    -56.20521300000000
      2      1.77120000000000    149.68995500000000
      2      2.06230000000000    -91.45439399999999
      1      3.35830000000000      3.72894900000000
      0      2.13000000000000      3.03799400000000
   P potential  5                                    # The number of P projections
      2      1.82310000000000     93.67560600000000
      2      2.12490000000000   -189.88896800000001
      2      2.57050000000000    110.24810400000000
      1      1.75750000000000      4.19959600000000
      0      6.76930000000000      5.00335600000000
   P so-potential  5                                 # The number of P-SO projections, no this part for scalar ECP
      2      1.82310000000000      1.51243200000000  # no this part for scalar ECP
      2      2.12490000000000     -2.94701800000000  # no this part for scalar ECP
      2      2.57050000000000      1.64525200000000  # no this part for scalar ECP
      1      1.75750000000000     -0.08862800000000  # no this part for scalar ECP
      0      6.76930000000000      0.00681600000000  # no this part for scalar ECP
   D so-potential  4                                 # The number of D-SO projections, no this part for scalar ECP
      2      1.22110000000000     -0.00138900000000  # no this part for scalar ECP
      2      3.36810000000000      0.00213300000000  # no this part for scalar ECP
      2      9.75000000000000      0.00397700000000  # no this part for scalar ECP
      1     29.26930000000000      0.03253000000000  # no this part for scalar ECP
   ****


For scalar ECP, the highest angular momentum of SOECP is 0 (can be omitted and not written),
and the data of the SO projection part is not required.

Once the above data is saved, the ``MYBAS-1`` basis set can be called in the BDF input file,
which needs to be implemented by the following mixed input modes:

.. code-block:: bdf

    #!bdfbasis.sh
    HF/genbas 

    Geometry
     .....
    End geometry

    $Compass
    Basis
       mybas-1         # the file name of basis set in the current directory, which is not case-sensitive
    $End


Custom basis sets must be entered with BDF's mixed-input mode or adavanced input moded. In the second line,
enter the basis set to **genbas**, and the custom basis set file name needs to use the keyword '''Basis''
in the **COMPASS** module, and the value is ''mybas-1''', which means that the basis set file named ''MYBAS-1'' is called.

Assignment of the basis set 
------------------------------------------------
**Use the same BDF built-in basis set for all atoms**

The easy input mode is specified in either Method/Functional/Basis set or Method/Basis set.
Here ''Basis Set'' is the built-in basis set name of the BDF listed in the previous sections,
and the input character is not case sensitive, as follows:

.. code-block:: bdf

   #! basisexample.sh
   TDDFT/PBE0/3-21g

   Geometry
   H   0.000   0.000    0.000
   Cl  0.000   0.000    1.400
   End geometry


.. code-block:: bdf

   #! basisexample.sh
   HF/lanl2dz 

   Geometry
   H   0.000   0.000    0.000
   Cl  0.000   0.000    1.400
   End geometry

In the case of advanced input mode, the basis set used for the calculation is specified in the  **COMPASS**  module using the keyword ''basis'', for example

.. code-block:: bdf

  $compass
  Basis
   lanl2dz
  Geometry
    H   0.000   0.000    0.000
    Cl  0.000   0.000    1.400
  End geometry
  $end

where ``lanl2dz`` calls the built-in LanL2DZ basis set (registered in the  ``basisname``  file), which is not case-sensitive.

**Specify different basis sets for different elements**

Easy input does not support custom or mixed basis sets, you must use mixed input mode, that is, set the basis set to  ``genbas``  in the
``method/functional/basis set``, and add the **COMPASS** module input, using the ``basis-block`` ... ``end basis`` keyword to specify the basis set.

If you specify a basis set with different names for different elements, you need to put it in the  ``basis-block`` ... ``end basis`` block of the **COMPASS module,
The first line is the default basis set, and the following lines specify other basis sets for different elements, in the format
*element=basissetname* or *element1, element2, ..., elementn=basissetnam*.

For example, in mixed input mode, the following is an example of using different basis sets for different atoms:

.. code-block:: bdf

  #! multibasis.sh
  HF/genbas 

  Geometry
  H   0.000   0.000    0.000
  Cl  0.000   0.000    1.400
  End geometry

  $compass
  Basis-block
   lanl2dz
   H = 3-21g
  End Basis
  $end

In the example above, H uses the 3-21G basis set, while Cl without additional definition uses the default LanL2DZ basis set.

If it's an advanced input, look like this:

.. code-block:: bdf

  $compass
  Basis-block
   lanl2dz
   H = 3-21g
  End Basis
  Geometry
    H   0.000   0.000    0.000
    Cl  0.000   0.000    1.400
  End geometry
  $end

.. attention::

    The ANO-RCC series basis sets (including ANO-RCC-VDZ, ANO-RCC-VDZP, etc.) cannot be specified directly in ``basis-block``
    due to different r/w mechanisms, and otherwise the program will use the highest level ANO-RCC basis set.
    You can copy the properly contracted ANO-RCC basis data to a  :ref:`custom basis set file<SelfdefinedBasis>`
    or the  ``inline`` section (see below).


**Provide basis set data in the input file explicitly** 

If you are using a custom non-standard basis set, in addition to editing a basis set file (see the previous section),
you can also write the basis set data for each element or atom type (i.e., the part between the two lines of **** in the basis set file)
in the data area between ``inline`` ... ``end line`` . For example:

.. code-block:: bdf

  $compass
  Basis-block
   sto-3g
   inline
   # Pitzer-cc-pVDZ-PP for F
     F       9    2
     S       4    3
                52.19000000
                 9.33900000
                 1.18100000
                 0.36250000
         -0.0097379000     0.0000000000     0.0000000000
         -0.1335636000     0.0000000000     0.0000000000
          0.6014362000     0.0000000000     1.0000000000
          0.5072134000     1.0000000000     0.0000000000
     P       4    2
                22.73000000
                 4.98500000
                 1.34700000
                 0.34710000
          0.0448419000     0.0000000000
          0.2356122000     0.0000000000
          0.5089430000     0.0000000000
          0.4578928000     1.0000000000
     D       1    1
                 1.69100000
          1.0000000000
     ECP
     F      2    1    1
     P potential  3
        2     44.51660000000000     -6.72302400000000
        2     12.94870000000000     -0.92964900000000
        1    132.49670000000000     -1.52673400000000
     S potential  4
        2      2.88350000000000     12.68530600000000
        2      3.10770000000000    -19.30258900000000
        1      5.61220000000000      1.00217900000000
        0      2.81460000000000      2.24534900000000
     P so-potential  3
        2     44.51660000000000     -0.01349600000000
        2     12.94870000000000      0.02610200000000
        1    132.49670000000000      0.10999800000000
   end line
   inline
   # 3-21G for Li
     Li      3   1
     S      6    3
                    0.3683820000E+02
                    0.5481720000E+01
                    0.1113270000E+01
                    0.5402050000E+00
                    0.1022550000E+00
                    0.2856450000E-01
           0.6966866381E-01       0.00000000             0.00000000
           0.3813463493E+00       0.00000000             0.00000000
           0.6817026244E+00       0.00000000             0.00000000
           0.00000000            -0.2631264058E+00       0.00000000
           0.00000000             0.1143387418E+01       0.00000000
           0.00000000             0.00000000             0.1000000000E+01
     P      3    2
                    0.5402050000E+00
                    0.1022550000E+00
                    0.2856450000E-01
           0.1615459708E+00       0.00000000
           0.9156628347E+00       0.00000000
           0.00000000             0.1000000000E+01
   end line
  End Basis
  Geometry
    Li  0.000   0.000    0.000
    F   0.000   0.000    1.400
  End geometry
  $end

In the definition of the LiF molecular system above, Li and F use the all-electron 3-21G basis set and
the Pitzer-cc-pVDZ-PP pseudopotential basis set, respectively, but they are not read from the standard basis set library;
instead, the basis set data is provided directly in the input file. The default basis set STO-3G is also defined in the example,
which is only to meet the formatting requirements of ``basis-block`` ... ``end basis`` and is not used in the actual calculations.


**Specify different basis sets for different atoms of the same element**

BDF can also specify different named basis sets for different atoms in the same element,
which need to be distinguished by an arbitrary number after the element symbol. For example


.. code-block:: bdf

  #! CH4.sh
  RKS/B3lyp/genbas

  Geometry
    C       0.000   -0.000    0.000
    H1     -0.000   -1.009   -0.357
    H2     -0.874    0.504   -0.457
    H1      0.874    0.504   -0.357
    H2      0.000    0.000    1.200
  End geometry

  $compass
  Basis-block
   6-31g
   H1= cc-pvdz
   H2= 3-21g
  End basis
  $end

In the example above, the cc-pVDZ is used for the two hydrogen atoms of type H1, the 3-21G is used for the two hydrogen atoms of type H2,
and the 6-31G is used for the carbon atom.

.. attention::

    1. If hydrogen atoms of types H1 and H2 are defined in the coordinates, then a basis set must be explicitly specified for each of them,
       as the default basis set does not contain hydrogen atoms named H1 and H2.
    2. symmetric equivalent atoms must use the same basis set, which will be checked by the program.
       If symmetric equivalents must use different basis sets, you can set a lower point group symmetry by  ``Group`` , or turn off symmetry with  ``Nosymm`` .


Auxiliary basis set
------------------------------------------------
The method of using the density fitting approximation (RI) requires a secondary basis set.
The Ahlrichs family of basis sets and Dunning-correlation consistent sets as well as other individual basis sets
have specially optimized auxiliary sets. In the BDF, it is possible to specify the auxiliary basis set in  **COMPASS** 
by the keywords ``RI-J``,  ``RI-K`` , and ``RI-C`` , where ``RI-J`` is used to specify the coulomb-fitting basis set,
``RI-K`` is used to specify the coulomb-exchange fitting basis set, and  ``RI-C``  is used to specify the
coulomb-correlation fitting basis set.
The auxiliary basis sets supported by BDF are stored in the corresponding folder in the ``$BDFHOME/basis_library`` directory.

High-level density fitting basis sets can be used on low-level basis sets, e.g.  ``cc-pVTZ/C``  can be used to make RI-J on  ``cc-pVTZ``.
For pople series basis sets that do not have a standard auxiliary basis set, such as ``6-31G**`` ,  ``cc-pVTZ/J`` can also be used for RI-J or RIJCOSX.
On the other hand, the combination of high-level orbital basis sets and low-level auxiliary basis sets will bring more obvious errors.

.. code-block:: bdf

  $Compass
  Basis
    DEF2-SVP
  RI-J
    DEF2-SVP
  Geometry
    C          1.08411       -0.01146        0.05286
    H          2.17631       -0.01146        0.05286
    H          0.72005       -0.93609        0.50609
    H          0.72004        0.05834       -0.97451
    H          0.72004        0.84336        0.62699
  End Geometry
  $End

In the example above, the :math:`\ce{CH4}`  methane molecule is computed using the ``def2-SVP`` basis set and
accelerated by using the def2-SVP standard coulomb-fitting basis set.

.. hint::
  The RI calculation function of BDF is used to accelerate the calculation of wave function methods, such as **MCSCF** and **MP2**,
  and it is not recommended for use in the calculations of **SCF** and **TDDFT**. For the latter methods, users may use
  the multipole expansion Coulomb potential (MPEC) method, which does not rely on auxiliary basis sets, and the calculation speed
  as well as accuracy are comparable to those of the RI method.

