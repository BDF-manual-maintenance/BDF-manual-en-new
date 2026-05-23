Atomic Orbital to Molecular Orbital Integral Transformation - TRAINT Module
================================================
The traint module is used to transform atomic orbital integrals to molecular orbital integrals. Only symmetry-adapted integral transformation is supported. When using traint, the Compass module must not contain the Skeleton keyword. The traint module is mainly used in conjunction with post-Hartree-Fock calculations to provide molecular orbital integrals for Post-HF methods.

**Basic Control Parameters**

:guilabel:`Frozen` Parameter Type: Integer Array
---------------------------------------------------
Specifies the number of frozen doubly occupied molecular orbitals for each irreducible representation.

:guilabel:`UTDDFT` Parameter Type: Boolean
------------------------------------------------
For MO-TDDFT algorithm, specifies integral transformation based on UKS orbitals. MO-TDDFT has low computational efficiency and is only used for testing and comparison.

:guilabel:`TDDFT` Parameter Type: Boolean
---------------------------------------------------
For MO-TDDFT algorithm, specifies integral transformation based on RKS orbitals. MO-TDDFT has low computational efficiency and is only used for testing.

:guilabel:`alpha & beta` Parameter Type: Integer Array
------------------------------------------------
Specifies the number of occupied orbitals for alpha and beta orbitals of each irreducible representation for MO-UTDDFT.

:guilabel:`Occupy` Parameter Type: Integer Array
---------------------------------------------------
Specifies the number of occupied orbitals for each irreducible representation in MO-TDDFT calculation.

:guilabel:`Orbital` Parameter Type: String
------------------------------------------------
Optional values: hforb, mcorb, Orbtxt

Specifies where to read molecular orbitals. hforb reads molecular orbitals from SCF calculation; mcorb reads molecular orbitals from MCSCF calculation; Orbtxt reads molecular orbitals from text file.

:guilabel:`FCIDUMP` Parameter Type: Boolean
---------------------------------------------------
Transforms molecular orbitals and stores them in the FCIDUMP file.

:guilabel:`Symmetry` Parameter Type: Integer
------------------------------------------------
Specifies the symmetry of the molecular state.

:guilabel:`Nelectron` Parameter Type: Integer
---------------------------------------------------
Specifies the number of electrons for Full CI calculation.

:guilabel:`Spin` Parameter Type: Integer
------------------------------------------------
Specifies the spin of the electronic state, 2S+1.