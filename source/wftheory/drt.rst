Graphical Unitary Group Approach - DRT Module
================================================
The DRT module is used in conjunction with the MRCI module to generate the Distinct Row Table (DRT) based on the Hole-particle symmetry based Graphical Unitary Group Approach (HP-GUGA) method. It is used for computing uncontracted Multireference Configuration Interaction with Single and Double excitations (MRCISD).

**Basic Control Parameters**

:guilabel:`Title` Parameter Type: String
------------------------------------------------
Input title; does not control the calculation, used only to label the calculation task.

:guilabel:`Spin` Parameter Type: Integer
------------------------------------------------
Specifies the spin multiplicity of the electronic state to be calculated, value is 2S+1.

:guilabel:`Symmetry` Parameter Type: Integer
------------------------------------------------
Specifies the symmetry of the electronic state to be calculated, i.e., the irreducible representation of the electronic state.

:guilabel:`Electron` Parameter Type: Integer
------------------------------------------------
Specifies the total number of electrons for CI calculation. Does not include electrons on orbitals frozen (Frozen) in traint.

:guilabel:`Nactel` Parameter Type: Integer
------------------------------------------------
Specifies the number of electrons in the active space for MRCI calculation.

:guilabel:`Inactive` Parameter Type: Integer Array
------------------------------------------------
Specifies the number of doubly occupied orbitals for each irreducible representation in MRCI calculation.

:guilabel:`Active` Parameter Type: Integer Array
------------------------------------------------
Specifies the number of active orbitals for each irreducible representation in MRCI calculation.

:guilabel:`Reference` Parameter Type: Integer Array
------------------------------------------------
Specifies the reference states for MRCI calculation.

:guilabel:`Ciall` Parameter Type: Integer Array
------------------------------------------------
Generates reference states using CAS method.