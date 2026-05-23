Multireference Configuration Interaction and Multireference Second-Order Perturbation Theory - XIANCI Module
============================================================
The xianci module originates from the Xi'an-CI program package, performing ucMRCI, icMRCI, XSDSCI, CB-MRPT2/3, MS-CASPT2, XMS-CASPT2, XDW-CASPT2, RMS-CASPT2, MS-NEVPT2, SS-NEVPT3, SDSPT2f, SDSPT2, SDSCIf, SDSCI, and other calculations.

**Basic Control Parameters**

:guilabel:`Roots` Parameter Type: Integer
------------------------------------------------
Specifies the number of roots (electronic states) to calculate.
* If mcscf only calculates one type of spatial and spin symmetry, the xianci module will read the number of roots calculated by the mcscf module as the default value, so this parameter does not need to be set.
* Default value: 1

:guilabel:`Istate` Parameter Type: Integer
------------------------------------------------
Specifies the number of roots to calculate and sets the indices of the roots to be calculated. If this keyword is used, the keyword ``Roots`` will be invalid.

.. attention::
 This keyword can only be used for all CASPT2, NEVPT2, SDSPT2, SDSCI and XSDSCI type methods.

Example: Line 1 contains 1 integer setting the number of states to calculate, Line 2 sets the indices of the selected electronic states (roots).

.. code-block:: bdf

     $xianci
     ...
     istate
     2
     1 3 
     $end

:guilabel:`Spin` Parameter Type: Integer
------------------------------------------------
Specifies the spin multiplicity of the electronic state to calculate, 2S+1.
* If mcscf only calculates one type of spatial and spin symmetry, the xianci module will read the number of roots calculated by the mcscf module as the default value, so this parameter does not need to be set.
* Default value: 1
:guilabel:`Symmetry` Parameter Type: Integer
------------------------------------------------
Specifies the symmetry of the electronic state to calculate.
* If mcscf only calculates one type of spatial and spin symmetry, the xianci module will read the number of roots calculated by the mcscf module as the default value, so this parameter does not need to be set.
* Default value: 1

:guilabel:`Frozen` Parameter Type: Integer Array
------------------------------------------------
Specifies the number of frozen doubly occupied (inactive) orbitals for each irreducible representation. It is recommended to freeze atomic Core orbitals. 
* Default is no frozen doubly occupied orbitals.

:guilabel:`Core` Parameter Type: Integer Array
------------------------------------------------
Specifies the number of frozen doubly occupied (inactive) orbitals for each irreducible representation. It is recommended to freeze atomic Core orbitals. 
* Default is no frozen doubly occupied orbitals.

:guilabel:`Dele` Parameter Type: Integer Array
------------------------------------------------
Specifies the number of frozen unoccupied (virtual) orbitals for each irreducible representation.
* Default is no frozen virtual orbitals.

:guilabel:`Electron` Parameter Type: Integer
------------------------------------------------
Number of electrons in the correlated orbitals.
* Default value comes from the mcscf module and may not need to be input.

:guilabel:`Inactive` Parameter Type: Integer Array
------------------------------------------------
Specifies the number of doubly occupied orbitals for each irreducible representation.
* Default value comes from the mcscf module and may not need to be input.

:guilabel:`Active` Parameter Type: Integer Array
------------------------------------------------
Specifies the number of active orbitals for each irreducible representation.
* Default value comes from the mcscf module and may not need to be input.

:guilabel:`XvrUse` Parameter Type: Boolean
------------------------------------------------
When the keyword 'Dele' is not used to set the molecular orbitals (MOs) to be deleted, the keyword 'XvrUse' is used to selectively delete virtual orbitals through the MCSCF XVR method.

.. attention::
  If both 'Dele' and 'XvrUse' are specified, the 'Dele' keyword takes precedence over 'XvrUse'.

* See example test126.inp for complete input logic

:guilabel:`Rootprt` Parameter Type: Integer
------------------------------------------------
Specifies the electronic state index required for calculating numerical gradients using the numgrad module.
* Default value: 1

:guilabel:`Orbtxt` Parameter Type: String
------------------------------------------------
Specifies the suffix name of the molecular orbital file to read.

:guilabel:`CVS` Parameter Type: Boolean
------------------------------------------------
Specifies generating a Core Valence Separation DRT during calculation, and using this DRT to calculate Core Valence excited states.
  
:guilabel:`ReadDRT` Parameter Type: Boolean
------------------------------------------------
Specifies reading the DRT information stored in $WORKDIR/$BDFTASK.cidrt from the working directory during calculation, thereby reducing the time required for DRT generation.
* Recommended for calculations on systems with large active spaces.
  
:guilabel:`Nexci` Parameter Type: Integer
------------------------------------------------
Specifies the number of electrons excited from the reference configuration.
* Default value: 2
* Optional values: 1 (single excitation only), >=3 (more than three electrons excited relative to the reference configuration's active orbitals)

:guilabel:`Readref` Parameter Type: Integer
------------------------------------------------
Automatically reads the reference configurations from $WORKDIR/BDFTASK.select_*_#, where * represents the spin multiplicity and # represents the irreducible representation.
* Default value comes from the mcscf module and may not need to be input.
* If the mcscf module does not set keywords "iCI" or "iCIPT2", and reference configurations need to be selected, then this keyword needs to be set.

:guilabel:`Node` Parameter Type: Integer
------------------------------------------------
Specifies the initial size of the array required to store nodes in the sub-DRTs that generate the CAS reference space (P space). No preset is needed for sub-DRTs generated by the state-selective method.
* Default value: 1000000

:guilabel:`Pmin` Parameter Type: Float
------------------------------------------------
Specifies that reference configurations with configuration coefficients greater than this value in $WORKDIR/BDFTASK.select_*_# are used as reference configurations for constructing excited configurations.
* Default value: Pmin=0.0, if the mcscf module includes keywords iCI or iCIPT2, the default value is Pmin=Cmin (Cmin comes from the mcscf module).
* Recommended value: Pmin=1.d-3

:guilabel:`QminDV` Parameter Type: Float
------------------------------------------------
Specifies the threshold for trimming the first-order interaction space (FOIS) value of uncontracted excited configurations in the Q subspace (\bar{D}V, double excitation operator including 3 active orbitals and 1 doubly occupied orbital).
* Default value: 0.0 
* Recommended value: 1.d-5

:guilabel:`QminVD` Parameter Type: Float
------------------------------------------------
Specifies the threshold for trimming the first-order interaction space (FOIS) value of uncontracted excited configurations in the Q subspace (\bar{V}D, double excitation operator including 3 active orbitals and 1 unoccupied orbital).
* Default value: 0.0 
* Recommended value: 1.d-5

:guilabel:`Qnex` Parameter Type: Boolean
------------------------------------------------
Specifies not selecting the DVD approximation. DVD approximation: When generating \bar{D}V and \bar{V}D excited configurations, some triple-excitation configurations involving 3 active orbitals are ignored.
* Default value: .false.

:guilabel:`Epic` Parameter Type: Float
------------------------------------------------
Specifies the threshold for storing internal contraction function coefficients in the coefficient matrix.
* Default value: QminVD=0.0 
* Recommended value: QminVD=1.d-5

:guilabel:`Seleref` Parameter Type: Integer
------------------------------------------------
Specifies the reference orbital configurations (orbital configuration, oCFG) for MRCI calculation. This parameter has nref+1 lines, where nref is the number of reference orbital configurations.
* Default value: If keyword "readref" is used to select reference configurations, this keyword is not needed.
* If the user wants to respecify oCFG, this keyword and nref selected oCFGs need to be set.

.. code-block:: bdf 

     $xianci
     ...
     seleref
     3 
     2200
     2110
     2020
     $end

First line: 1 integer, specifying the number of reference states nref.
Lines 2 to nref+1 give the reference orbital configurations.

:guilabel:`Prtcri` Parameter Type: Float
------------------------------------------------
Specifies the threshold for printing output CSFs.
* Default value: 0.05

:guilabel:`Ethres` Parameter Type: Float
------------------------------------------------
Specifies the energy (eigenvalue) convergence threshold for H0 matrix diagonalization.
* Default value: 1.D-8

:guilabel:`Conv` Parameter Type: Float Array
------------------------------------------------
Specifies the convergence thresholds for H matrix iterative diagonalization in MRCI calculation. Input three floating-point numbers, controlling the energy, wave function, and residual vector convergence thresholds for MRCI iteration respectively.
* Default values: 1.D-8, 1.D-6, 1.D-8

:guilabel:`Maxiter` Parameter Type: Integer
------------------------------------------------
Specifies the maximum number of iterations for H0 or H matrix iterative diagonalization.
* Default value: 200

:guilabel:`Maxbloch` Parameter Type: Integer
------------------------------------------------
Specifies the maximum number of iterations for solving the BLOCH equation required for iterative CASPT2, SDSPT2f, and SDSCIf calculations.
* Default value: 5

:guilabel:`InitHDav` Parameter Type: Integer
------------------------------------------------
Specifies the method for setting initial vectors during MRCI iterative diagonalization:
* Default value: 1  Uses the excited configurations most strongly coupled to the lowest-energy configuration functions (CSFs) as the initial vector.
* Optional value: 2  Selects initial vectors according to the energy level order from low to high based on CI Hamiltonian diagonal elements.
* Optional value: 3  Uses the reference wave function as the initial vector for Davidson diagonalization.

:guilabel:`InitH0Dav` Parameter Type: Integer
------------------------------------------------
Specifies the method for setting initial vectors during H0 iterative diagonalization:
* Default value: 2  Selects initial vectors according to the energy level order from low to high based on CI Hamiltonian diagonal elements.
* Optional value: 1  Uses the excited configurations most strongly coupled to the lowest-energy configuration functions (CSFs) as the initial vector.

:guilabel:`DoProp` Parameter Type: Boolean
------------------------------------------------
Specifies calculation of one-electron (transition) reduced density matrix and related properties, such as (transition) dipole moment, etc.
* Prints natural orbital occupation numbers.
* Provides the natural orbital file $BDF_WORKDIR/$BDFTASK.xianciorb.
* Provides the orbital image file $BDF_WORKDIR/$BDFTASK.xianci.molden.

:guilabel:`DCRI` Parameter Type: Float
------------------------------------------------
Specifies the orthogonalization threshold for internally contracted configuration functions.
* Default value: 1.D-12

:guilabel:`EPCC` Parameter Type: Float
------------------------------------------------
Sets the threshold for ignored contracted configuration coupling coefficients. Larger values improve icMRCI computational efficiency but reduce accuracy.
* Default value: 1.D-20

:guilabel:`Qfix` Parameter Type: Float
------------------------------------------------
Specifies the configurations that need to be optimized during iCMRCI iterative diagonalization. Only excited configurations with coefficients greater than this threshold in the first-order wave function obtained from SDSPT2(f) calculation need to be optimized. 
* Default value: 0.0

:guilabel:`Ncisave` Parameter Type: Integer
------------------------------------------------
Specifies the dimension of the H0 matrix that can be completely diagonalized. For computers with larger memory space, this value can be increased to reduce repeated calculation of matrix elements.
* Default value: 50000

:guilabel:`NoSaveact` Parameter Type: Boolean
------------------------------------------------
Specifies not storing coupling coefficients in memory during H0 iterative diagonalization calculation, avoiding the problem of insufficient memory space that may occur when calculating large active spaces.
  
:guilabel:`Setlpact` Parameter Type: Integer
------------------------------------------------
Specifies the initial size of the array used to store all coupling coefficients during H0 iterative diagonalization calculation.
Larger initial input means fewer dynamic array expansion operations, higher computational efficiency, but may cause insufficient memory space when calculating large active spaces.
* Default value: 100000000
 
:guilabel:`Setblkact` Parameter Type: Integer
------------------------------------------------
Specifies the initial size of the array used to store coupling coefficient classes during H0 iterative diagonalization calculation.
Larger initial input means fewer dynamic array expansion operations, higher computational efficiency, but may cause insufficient memory space when calculating large active spaces.
* Default value: 10000000
 
:guilabel:`Nosavelp` Parameter Type: Boolean
------------------------------------------------
Specifies that (internally contracted) coupling coefficients are not stored during icMRCI calculation, which reduces computational efficiency but saves hard disk storage space when calculating large active spaces.

:guilabel:`Setloop` Parameter Type: Integer
------------------------------------------------
Specifies the initial size of the array used to store one class of coupling coefficients during MRCI iterative diagonalization calculation.
Larger initial input means fewer dynamic array expansion operations, higher computational efficiency, but may cause insufficient memory space when calculating large active spaces.
* Default value: 10000000
 
:guilabel:`Setblk` Parameter Type: Integer
------------------------------------------------
Specifies the initial size of the array used to store coupling coefficient classes during MRCI iterative diagonalization calculation.
Larger initial input means fewer dynamic array expansion operations, higher computational efficiency, but may cause insufficient memory space when calculating large active spaces.
* Default value: 10000000

**Internally Contracted CI Method Selection Parameters**

:guilabel:`FCCI` Parameter Type: Boolean
------------------------------------------------
Specifies performing fully internally contracted MRCI (icMRCI) calculation on the excited state space (Q), but the reference state space (P) is not contracted, and perturbation calculation will contract the reference state space.
* This method is used by default.

:guilabel:`XSDSCI` Parameter Type: Boolean
------------------------------------------------
Specifies performing FCCI calculation.
* The initial guess for excited wave function coefficients comes from SDSPT2 calculation (Dyall Hamiltonian as H0). When calculating low-lying excited states, the Intruder State problem can be completely avoided.

:guilabel:`VSD` Parameter Type: Boolean
------------------------------------------------
Virtual Space Decomposition (VSD) projects large basis set virtual molecular orbitals (MOs) onto a small basis set space,
uses singular value decomposition (SVD) to screen out the strongly correlated space, thereby dividing high-dimensional virtual orbital space into subspaces with clear physical meaning.
This method, combined with XSDSCI, can significantly improve the efficiency of multi-reference calculations.
* See example test126.inp

:guilabel:`NoVDVP` Parameter Type: Boolean
------------------------------------------------
Specifies skipping the CI Hamiltonian matrix element calculation between Q subspace \bar{V}D and \bar{V}P and the zero-order wave function.

:guilabel:`SDSCI` Parameter Type: Boolean
------------------------------------------------
Specifies performing SDSCI calculation.
* The excited wave function coefficients come from SDSPT2 calculation (Dyall Hamiltonian as H0). When calculating low-lying excited states, the Intruder State problem can be completely avoided.
* Recommended for use; this is currently the MRCI method with the smallest computational cost in the xianci module.

:guilabel:`SDSCIf` Parameter Type: Boolean
------------------------------------------------
Specifies performing SDSCIf calculation.
* The excited wave function coefficients come from SDSPT2f calculation (generalized Fock operator as H0). The Intruder State problem may occur.

:guilabel:`UCCI` Parameter Type: Boolean
------------------------------------------------
Specifies performing uncontracted MRCISD (ucMRCI) calculation.

:guilabel:`NICI` Parameter Type: Boolean
------------------------------------------------
Specifies performing icMRCI calculation without contracting all internal space excitations.

:guilabel:`CWCI` Parameter Type: Boolean
------------------------------------------------
Specifies performing Celani-Werner contracted icMRCI calculation.

:guilabel:`WKCI` Parameter Type: Boolean
------------------------------------------------
Specifies performing Werner-knowles contracted icMRCI calculation.

:guilabel:`SDCI` Parameter Type: Boolean
------------------------------------------------
Specifies performing SDCI-mode icMRCI calculation. The degree of contraction and accuracy is between CWCI and WKCI.

**Multi-Reference Perturbation Calculation Related Parameters**

:guilabel:`CASPT2` Parameter Type: Boolean
------------------------------------------------
Specifies performing MS-CASPT2 (Multi-State CASPT2), constructing its own Q space for each reference state.

:guilabel:`RMSCASPT2` Parameter Type: Boolean
------------------------------------------------
Specifies performing RMS-CASPT2 (Rotated Multi-State CASPT2), constructing its own Q space for each reference state.

:guilabel:`XMSCASPT2` Parameter Type: Boolean
------------------------------------------------
Specifies performing RMS-CASPT2 (Extended Multi-State CASPT2), constructing its own Q space for each reference state.

:guilabel:`XDWCASPT2` Parameter Type: Boolean
------------------------------------------------
Specifies performing XDW-CASPT2 (Extended Dynamic Weight Multi-State CASPT2), constructing its own Q space for each reference state.

:guilabel:`XDWPara` Parameter Type: Float
------------------------------------------------
Specifies the parameter required for XDW-CASPT2 (Extended Dynamic Weight Multi-State CASPT2).
* Default value: 50
* 0: XMS-CASPT2; Infinity: RMS-CASPT2.

:guilabel:`SDSPT2f` Parameter Type: Boolean
------------------------------------------------
Specifies performing SDSPT2f calculation.
* The excited wave function coefficients use the perturbation method (generalized Fock operator as H0). The Intruder State problem may occur.

:guilabel:`Rshift` Parameter Type: Float
------------------------------------------------
Specifies the Real Level Shift parameter required to weaken the Intruder State problem in CASPT2 and other methods based on the generalized Fock operator as H0.
** Default value: 0.0
* Recommended value: 0.3

:guilabel:`Ishift` Parameter Type: Float
------------------------------------------------
Specifies the Imaginary Level Shift parameter required to weaken the Intruder State problem in CASPT2 and other methods based on the generalized Fock operator as H0.
* Default value: 0.0
* Recommended value: 0.1

:guilabel:`NEVPT2` Parameter Type: Boolean
------------------------------------------------
Specifies performing MS-NEVPT2 (Multi-State NEVPT2), constructing its own Q space for each reference state.

:guilabel:`SDSPT2` Parameter Type: Boolean
------------------------------------------------
Specifies performing SDSPT2 calculation.
* The excited wave function coefficients use the perturbation method (Dyall Hamiltonian as H0). When calculating low-lying excited states, the Intruder State problem can be completely avoided.

:guilabel:`DVRLS` Parameter Type: Float
------------------------------------------------
Specifies the Real Level Shift parameter required to weaken the Intruder State problem in Q subspace (\bar{D}V) for methods based on Dyall Hamiltonian as H0 such as NEVPT2 when calculating high-lying excited states.
* Default value: 0.0
* Recommended value: 0.3

:guilabel:`VDRLS` Parameter Type: Float
------------------------------------------------
Specifies the Real Level Shift parameter required to weaken the Intruder State problem in Q subspace (\bar{V}D) for methods based on Dyall Hamiltonian as H0 such as NEVPT2 when calculating high-lying excited states.
* Default value: 0.0
* Recommended value: 0.3

:guilabel:`DDRLS` Parameter Type: Float
------------------------------------------------
Specifies the Real Level Shift parameter required to weaken the Intruder State problem in Q subspace (\bar{D}D) for methods based on Dyall Hamiltonian as H0 such as NEVPT2 when calculating high-lying excited states.
* Default value: 0.0
* Recommended value: 0.3

:guilabel:`DVILS` Parameter Type: Float
------------------------------------------------
Specifies the Imaginary Level Shift parameter required to weaken the Intruder State problem in Q subspace (\bar{D}V) for methods based on Dyall Hamiltonian as H0 such as NEVPT2 when calculating high-lying excited states.
* Default value: 0.0
* Recommended value: 0.1
* This parameter is not recommended for use.  

:guilabel:`VDILS` Parameter Type: Float
------------------------------------------------
Specifies the Imaginary Level Shift parameter required to weaken the Intruder State problem in Q subspace (\bar{V}D) for methods based on Dyall Hamiltonian as H0 such as NEVPT2 when calculating high-lying excited states.
* Default value: 0.0
* Recommended value: 0.1
* This parameter is not recommended for use.  

:guilabel:`DDILS` Parameter Type: Float
------------------------------------------------
Specifies the Imaginary Level Shift parameter required to weaken the Intruder State problem in Q subspace (\bar{D}D) for methods based on Dyall Hamiltonian as H0 such as NEVPT2 when calculating high-lying excited states.
* Default value: 0.0
* Recommended value: 0.1
* This parameter is not recommended for use.  

:guilabel:`SAFock` Parameter Type: Boolean
------------------------------------------------
Specifies using state-averaged (SA) molecular orbital energies and integrals in NEVPT2, SDSPT2, and SDSCI calculations.
* Default value: .true.

:guilabel:`SDFock` Parameter Type: Boolean
------------------------------------------------
Specifies using state-specific (SS) molecular orbital energies and state-averaged (SA) molecular orbital integrals in NEVPT2, SDSPT2, and SDSCI calculations.
* Default value: .false.

:guilabel:`SSFock` Parameter Type: Boolean
------------------------------------------------
Specifies using state-specific (SS) molecular orbital energies and integrals in NEVPT2 calculation.
* Default value: .false.

:guilabel:`Nolan` Parameter Type: Boolean
------------------------------------------------
Specifies not calculating the Secondary states required for SDSPT2(f) and SDSCI(f).
* This keyword is used by default.

* For SDSPT2(f) and SDSCI(f) calculations with large active spaces, the keyword "Nolan" can be used to cancel the computationally expensive process of constructing Ps wave functions.
  The effective Hamiltonian matrix constructed by this method has a dimension of 2N, and in general, the calculation accuracy decreases slightly.
  However, it should be emphasized that when electronic states intersect during the calculation process (e.g., conical intersection points), the calculation accuracy may decrease to some extent.

:guilabel:`Dylan` Parameter Type: Boolean
------------------------------------------------
Specifies truncated approximation calculation of Secondary states required for SDSPT2(f) and SDSCI(f).

* For SDSPT2(f) and SDSCI(f) calculations with large active spaces, the keyword "Dylan" can be used to truncate the contribution of higher-energy Ps functions to Secondary states.
  The effective Hamiltonian matrix constructed by this method has a dimension of 3N.
  In general, calculation accuracy can be maintained, but the number of Ps functions selected varies for different molecular configurations.

:guilabel:`Dolan` Parameter Type: Boolean
------------------------------------------------
Specifies using the Lanczos method to calculate Secondary states required for SDSPT2(f) and SDSCI(f).

* For SDSPT2(f) and SDSCI(f) calculations with large active spaces, the computational cost of calculating Secondary states using the keyword "Dolan" is very large.
  The effective Hamiltonian matrix constructed by this method has a dimension of 3N.
  In general, calculation accuracy can be maintained, but the large computational cost makes this scheme not recommended.
 
:guilabel:`DEPENST` Parameter Type: Boolean
------------------------------------------------
Specifies using state-specific Fock diagonal elements in the Dyall Hamiltonian. Default: state-averaged Fock matrix diagonal elements.

:guilabel:`MR-NEVPT2` Parameter Type: Boolean
------------------------------------------------
Specifies performing Multi-reference NEVPT2 calculation.
* Constructs a globally orthogonal configuration space for all reference states.

:guilabel:`NEVPT3` Parameter Type: Boolean
------------------------------------------------
Specifies performing SS-NEVPT3 calculation.
* The Q space is independent for each state.

:guilabel:`CBMPRT2` Parameter Type: Boolean
------------------------------------------------
Specifies performing CBMRPT2 calculation.

:guilabel:`MR-CBMRPT2` Parameter Type: Boolean
------------------------------------------------
Specifies performing MR-CBMPRT2 calculation.
* Constructs a globally orthogonal configuration space for all reference states.

:guilabel:`CBMRPT3` Parameter Type: Boolean
------------------------------------------------
Specifies performing CBMRPT3 calculation.
* The Q space is independent for each state.

**Test Cases**

:guilabel:`test069.inp`
------------------------------------------------
.. attention::
   The energies of SDSPT2(f), SDSCI(f), XSDSCI, and icMRCI take the results with +Q1 (Pople Correction).
   The energy of ucMRCI takes the result with +Q3 (Davidson Correction).   

.. code-block:: bdf

     $xianci
     core 
     2 0 0 2  
     nroots
     1
     spin
     1 
     symmetry
     1
     pmin
     1.d-3
     qmindv
     1.d-5
     qminvd
     1.d-5
     epic
     1.d-5
     CASPT2 # MS-CASPT2 with generalized Fock as H0
     DBLOCH # the threshold of solving BLOCH equation
     1.d-4  # default : 1.d-4
     RLS    # Real Level Shift
     0.0    # default : 0.0
     #ILS    # Imaginary Level Shift
     #0.0    # default : 0.0
     $end

     Output :

     CASPT2 calculation is completed.

     NROOT        MC ENERGY       SS-CASPT2 ENERGY    MS-CASPT2 ENERGY    SS-CASPT3 ENERGY    MS-CASPT3 ENERGY
       1       -154.98370235       -155.47704723       -155.47704723          0.00000000          0.00000000
 
.. code-block:: bdf

     $xianci
     core
     2 0 0 2
     nroots
     1
     spin
     1
     symmetry
     1
     nevpt2 
     $end

     Output:

     NEVPT2 calculation is completed.

     NROOT        MC ENERGY       SS-NEVPT2 ENERGY    MS-NEVPT2 ENERGY    SS-NEVPT3 ENERGY    MS-NEVPT3 ENERGY
       1       -154.98370416       -155.47772092       -155.47772092          0.00000000          0.00000000

.. code-block:: bdf
 
     $xianci
     core
     2 0 0 2
     nroots
     1
     spin
     1
     symmetry
     1
     sdspt2f 
     dbloch 
     1.d-4 
     rls 
     0.0 
     $end
 
     Output:

     MRPT2 calculation is completed.

     NROOT   MC ENE      SS-CASPT2 ENE   MS-CASPT2 ENE    SDSPT2 ENE  SDSPT2+Q1 ENE  SDSPT2+Q2 ENE   SDSPT2+Q3 ENE   DAVCOEF
       1  -154.98370416  -155.47702635   -155.47702635 -155.41225671  -155.47144162  -155.47211363  -155.46852939   0.883932
   
.. code-block:: bdf
 
     $xianci
     core
     2 0 0 2
     nroots
     1
     spin
     1
     symmetry
     1
     sdspt2 
     $end

     Output:

     MRPT2 calculation is completed.

     NROOT   MC ENE     SS-NEVPT2 ENE  MS-NEVPT2 ENE  SDSPT2 ENE    SDSPT2+Q1 ENE  SDSPT2+Q2 ENE   SDSPT2+Q3 ENE   DAVCOEF
       1  -154.98370416 -155.47772092  -155.47772092  -155.41222583 -155.47205111  -155.47273880   -155.46903845   0.882941

.. code-block:: bdf

     $xianci
     core
     2 0 0 2
     nroots
     1
     spin
     1
     symmetry
     1
     sdscif 
     $end

     Output:

     MRPT2 calculation is completed.

     NROOT   MC ENE    SS-CASPT2 ENE  MS-CASPT2 ENE  SDSCI  ENE    SDSCI+Q1  ENE  SDSCI+Q2  ENE   SDSCI+Q3  ENE   DAVCOEF
       1 -154.98370416 -155.47702635  -155.47702635  -155.43865322 -155.51060490  -155.51155875   -155.50597757   0.871094
     
.. code-block:: bdf

     $xianci
     core
     2 0 0 2
     nroots
     1
     spin
     1
     symmetry
     1
     sdsci 
     $end
     
     Output:

     MRPT2 calculation is completed.

     NROOT   MC ENE     SS-NEVPT2 ENE  MS-NEVPT2 ENE  SDSCI  ENE    SDSCI+Q1  ENE   SDSCI+Q2  ENE   SDSCI+Q3  ENE   DAVCOEF
       1  -154.98370416 -155.47772092  -155.47772092  -155.43734298 -155.50941634   -155.51037685   -155.50474252   0.870644

.. code-block:: bdf
     
     $xianci
     core
     2 0 0 2
     nroots
     1
     spin
     1
     symmetry
     1
     xsdsci 
     ncisave
     10
     $end

     Output:

     Roots of Heff are calculated are listed below: 
 
                     ENE             ENE + Pople       ENE + App Pople       ENE + DAV           ENE + MEISS
     root   1   -155.44999113       -155.52660992       -155.52767146       -155.52133469       -155.51198622
    

.. code-block:: bdf

     $xianci
     core
     2 0 0 2
     nroots
     1
     spin
     1
     symmetry
     1
     $end

     Output:  
     Roots of Heff are calculated are listed below:  
                       ENE           ENE + Pople       ENE + App Pople       ENE + DAV           ENE + MEISS
     root   1    -155.45099589       -155.52816454       -155.52923990       -155.52280494       -155.51339548
 

:guilabel:`test080.inp`
------------------------------------------------

:guilabel:`test095.inp`
------------------------------------------------

:guilabel:`test126.inp`
------------------------------------------------

:guilabel:`test131.inp`
------------------------------------------------

:guilabel:`test139.inp`
------------------------------------------------

:guilabel:`test148.inp`
------------------------------------------------