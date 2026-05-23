Basis Set Orbital Expansion - EXPANDMO Module
================================================
The expandmo module mainly implements the expansion of small basis set molecular orbitals to large basis set, used to accelerate large basis set molecular orbital optimization. It also implements automatic generation of localized and canonical active orbitals, and other functions.

**Basic Control Parameters**

:guilabel:`Overlap` Parameter Type: Boolean
------------------------------------------------
Specifies to expand the small basis set molecular orbitals obtained from SCF to the large basis set, used to accelerate large basis set molecular orbital optimization.

:guilabel:`Overcri` Parameter Type: Float
------------------------------------------------
Specifies the orbital occupation number threshold for the "Overlap" keyword: orbitals with occupation numbers exceeding this threshold will be defined as occupied orbitals.
* Default value: 0.0

:guilabel:`Sb2lb` Parameter Type: Boolean
------------------------------------------------
Uses the molecular orbitals (MOs) from small basis set MCSCF calculation as initial guess orbitals for large basis set MCSCF calculation. See test150.inp for a specific example.

* Function description:
  Basis set expansion strategy: Using orbitals optimized at small basis set as a starting point to accelerate convergence of large basis set calculations.
  Application scenario: Suitable for computational workflow optimization during basis set upgrades, reducing iteration count and computational resource consumption.
  File guide: test150.inp demonstrates the parameter settings for orbital initialization during basis set switching.

:guilabel:`Sbolb` Parameter Type: Boolean
------------------------------------------------
Specifies calculation of orbital overlap matrix.
Computes and outputs the overlap matrix between small basis set ($BDF_WORKDIR/$BDFTASK.sbforb) and large basis set ($BDF_WORKDIR/$BDFTASK.lbforb) molecular orbitals (MOs).

* Function description:
  1. Purpose: Quantifies the overlap degree of molecular orbitals under different basis sets, assisting in analyzing the effect of basis set expansion on orbital properties.
  2. Output format: Matrix data is usually written to the specified output file, in a row-column value table format.
  3. Typical applications: Orbital continuity verification during basis set upgrades, orbital pre-screening before automatic active space selection.

:guilabel:`S12cmo` Parameter Type: Boolean
------------------------------------------------
Specifies calculation of orbital overlap matrix between two different molecular configurations, and matches the molecular orbitals of the second configuration to those of the first configuration based on the first configuration's molecular orbitals, such that the second configuration's molecular orbitals are similar to those of the first configuration. Currently, this orbital matching is only applicable to molecular systems without symmetry.

First configuration file paths:
Checkpoint file: $BDF_WORKDIR/$BDFTASK.chkfil1
Canonical molecular orbital (CMO) file: $BDF_WORKDIR/$BDFTASK.inporb1

Second configuration file paths:
Checkpoint file: $BDF_WORKDIR/$BDFTASK.chkfil2
Canonical molecular orbital (CMO) file: $BDF_WORKDIR/$BDFTASK.inporb2

* Function description:
  1. Purpose: Quantifies the molecular orbital overlap degree between different configurations (e.g., before and after geometry optimization, reaction path nodes), used to analyze the effect of configuration changes on electronic states.
  2. Output format: Matrix data is usually written to the specified output file, in a row-column value table format.
  3. Typical applications: Transition state analysis, configuration correlation studies, orbital continuity tracking.

.. code-block:: bdf

      $compass
      title
      c2h4 molecule test run
      basis
      cc-pvdz
      geometry
      c 0.00000000 0.00000000 1.25306000
      c 0.00000000 0.00000000 -1.25306000
      h 1.74646000 0.00000000 2.37500000
      h -1.74646000 0.00000000 2.37500000
      h 1.74646000 0.00000000 -2.37500000
      h -1.74646000 0.00000000 -2.37500000
      end geometry
      nosym
      unit
      bohr
      skeleton
      RI-C
      cc-pvdz
      $end
      
      %cp $BDF_WORKDIR/$BDFTASK.chkfil $BDF_WORKDIR/$BDFTASK.chkfil1
      
      $xuanyuan
      $end
      
      $scf
      RHF
      $end
      
      %cp $BDF_WORKDIR/$BDFTASK.scforb $BDF_WORKDIR/$BDFTASK.inporb
      
      $expandmo
      vcmo
      minbas
      4
      1C|2P-1  
      1C|2P0   
      2C|2P-1   
      2C|2P0   
      $end
      
      %cp $BDF_WORKDIR/$BDFTASK.exporb $BDF_WORKDIR/$BDFTASK.inporb
      
      $mcscf
      automc
      spin
      1
      roots
      2 2 1
      symmetry
      1
      guess
      read
      molden
      quasi
      $end
      
      %cp $BDF_WORKDIR/$BDFTASK.casorb $BDF_WORKDIR/$BDFTASK.inporb1
      
      $compass
      title
      c2h4 molecule test run
      basis
      cc-pvdz
      geometry
      c 0.00000000 0.00000000 1.35306000
      c 0.00000000 0.00000000 -1.35306000
      h 1.74646000 0.00000000 2.37500000
      h -1.74646000 0.00000000 2.37500000
      h 1.74646000 0.00000000 -2.37500000
      h -1.74646000 0.00000000 -2.37500000
      end geometry
      #nosym
      unit
      bohr
      skeleton
      RI-C
      cc-pvdz
      $end
      
      %cp $BDF_WORKDIR/$BDFTASK.chkfil $BDF_WORKDIR/$BDFTASK.chkfil2
      
      $xuanyuan
      $end
      
      $scf
      RHF
      $end
      
      %cp $BDF_WORKDIR/$BDFTASK.scforb $BDF_WORKDIR/$BDFTASK.inporb
      
      $expandmo
      vcmo
      minbas
      4
      1C|2P-1  
      1C|2P0   
      2C|2P-1   
      2C|2P0   
      $end
      
      %cp $BDF_WORKDIR/$BDFTASK.exporb $BDF_WORKDIR/$BDFTASK.inporb
      
      $mcscf
      automc
      #close
      #6
      #active
      #4
      #actele
      #4
      spin
      1
      roots
      2 2 1
      symmetry
      1
      guess
      read
      molden
      quasi
      $end
      
      %cp $BDF_WORKDIR/$BDFTASK.casorb $BDF_WORKDIR/$BDFTASK.inporb2
      
      $expandmo
      s12cmo
      $end
      

:guilabel:`Core` Parameter Type: Integer Array
------------------------------------------------
Specifies the number of frozen doubly occupied (inactive) orbitals for each irreducible representation required for the calculation.

:guilabel:`Close` Parameter Type: Integer Array
------------------------------------------------
Specifies the number of unfrozen doubly occupied (inactive) orbitals for each irreducible representation required for the calculation.

:guilabel:`Active` Parameter Type: Integer Array
------------------------------------------------
Specifies the number of active orbitals for each irreducible representation required for the calculation.

:guilabel:`Acte` Parameter Type: Integer
------------------------------------------------
Specifies the number of active electrons required for the calculation.

:guilabel:`Phosp` Parameter Type: Integer
------------------------------------------------
Sets Projected Hybrid Orbitals (PHO) as active atomic orbitals, supporting sp²/sp³/sp hybrid system modeling.

.. code-block:: bdf

   PHOSP
   2  ! First line: total number of atoms to be hybridized
   2 1 2 3 4 0  ! Second line: sp² parameter structure: (n=2) (center atom 1) (coordination atoms 2,3,4) (0: one position has no adjacent atom)
   ! Parameter explanation:
   ! 2 → principal quantum number n=2 (operates on 2s/2p orbitals)
   ! 1 → center atom number 1
   ! 2 3 4 → three coordination atom numbers
   ! 0 → mark sp² hybridization (non-zero value triggers sp³)
   2 2 1 5 6 7  ! Third line: sp³ parameter structure: (principal quantum number n=2) (center atom 2) (coordination atoms 1,5,6,7)
   3 4 1 5 0 0  ! Fourth line: sp parameter structure: (principal quantum number n=3) (center atom 4) (coordination atoms 1,5) (0,0: two positions have no adjacent atoms)

.. attention::

   If the user wants to select specific hybrid orbitals, such as sp³ hybrid orbitals, but the number of adjacent atoms directly bonded to the selected atom is insufficient, atoms at the next-nearest positions in approximately the same direction can be used as substitutes.
   This keyword is used only to obtain approximate hybrid atomic orbitals to generate initial guesses for molecular orbitals with specific bond types. The final molecular orbitals are generated by MCSCF calculation.

:guilabel:`Minbas` Parameter Type: String
------------------------------------------------
Specifies the selected active (hybrid) atomic orbitals. If the keyword "Phosp" is used, this indicates selection of hybrid atomic orbitals.
The first line specifies the number of selected orbitals.
Starting from the second line, the selected atomic orbitals are set line by line.
* The COMPASS program output atomic orbital symbol format must be strictly followed (case-insensitive).
  Standard basis sets use the "element|orbital" format (e.g., 1Co|3D0).
  The numeric prefix "1" represents the atomic number, "Co3" in "Co3" represents the basis set number, and the orbital symbol must be exactly consistent with the program's internal definition.
* Do not modify the naming convention of orbital symbols.

.. attention::
   When PHOsp is enabled, the BDF program's atomic orbital ordering rule is adopted by default:

   1. If the selected atom and adjacent atoms use sp³ hybridization.
   s0  : hybrid atomic orbital bonded to the 1st adjacent atom.
   p-1 : hybrid atomic orbital bonded to the 2nd adjacent atom.
   p1  : hybrid atomic orbital bonded to the 3rd adjacent atom.
   p0  : hybrid atomic orbital bonded to the 4th adjacent atom.

   2. If the selected atom and adjacent atoms use sp² hybridization.
   s0  : hybrid atomic orbital bonded to the 1st adjacent atom.
   p-1 : hybrid atomic orbital bonded to the 2nd adjacent atom.
   p1  : hybrid atomic orbital bonded to the 3rd adjacent atom.
   p0  : (approximate) hybrid atomic orbital perpendicular to s0, p-1, and p1.

   3. If the selected atom and adjacent atoms use sp hybridization.
   s0  : hybrid atomic orbital bonded to the 1st adjacent atom.
   p-1 : hybrid atomic orbital bonded to the 2nd adjacent atom.
   p1  : lone pair hybrid atomic orbital.
   p0  : another lone pair hybrid atomic orbital.

:guilabel:`Avas` Parameter Type: Boolean
------------------------------------------------
Specifies using the Atomic Valence Active Space (AVAS) method to generate quasi-canonical molecular orbitals including the active molecular orbitals obtained from the atomic orbitals selected by the keyword "Minbas". Auto-generated doubly occupied orbitals, active orbitals, and virtual orbitals are sorted by energy from low to high.

:guilabel:`Vcmo` Parameter Type: Boolean
------------------------------------------------
Specifies using the Imposed CAS (iCAS) method to generate quasi-canonical molecular orbitals including the active molecular orbitals obtained from the atomic orbitals selected by the keyword "Minbas". Auto-generated doubly occupied orbitals, active orbitals, and virtual orbitals are sorted by energy from low to high.

:guilabel:`Localmo` Parameter Type: Boolean
------------------------------------------------
Specifies localizing the quasi-canonical molecular orbitals generated by the keyword "Vcmo". Classified as doubly occupied orbitals, active orbitals, and virtual orbitals, then generating localized molecular orbitals.
* Pipek-Mezey type localized orbitals are generated by default.

:guilabel:`Vlmo` Parameter Type: Boolean
------------------------------------------------
Contracts the Fock matrix to active atomic orbitals, diagonalizes the Fock matrix and localizes valence canonical molecular orbitals (VCMOs), generating valence pre-localized molecular orbitals (pre-LMO).
Automatically selects active localized molecular orbitals (LMOs) or fragment localized molecular orbitals (FLMOs).

.. attention::
   This function only supports systems without symmetry. pre-LMOs are currently only generated from pre-CMOs via localization, and do not support external orbital input.
   The default localization method is Pipek-Mezey; other localization methods can be used by entering keywords such as "Boys".

:guilabel:`Nolmocls` Parameter Type: Boolean
------------------------------------------------
Specifies not localizing the doubly occupied orbitals generated by keywords "Vcmo" or "Vlmo".

:guilabel:`Nolmoact` Parameter Type: Boolean
------------------------------------------------
Specifies not localizing the active orbitals generated by keywords "Vcmo" or "Vlmo".

:guilabel:`Nolmovir` Parameter Type: Boolean
------------------------------------------------
Specifies not localizing the virtual orbitals generated by keywords "Vcmo" or "Vlmo".

:guilabel:`Pipek` Parameter Type: Boolean
------------------------------------------------
Specifies localizing the generated quasi-canonical molecular orbitals into Pipek-Mezey type localized molecular orbitals.
* Mulliken charges are used by default. If the user wants to use Lowdin charges, enter the keyword "Lowdin".
* The first-order method Jacobi sweep iteration is used by default to generate localized molecular orbitals. If the user wants to use the second-order method trust-region, enter the keyword "Trust".

:guilabel:`Boys` Parameter Type: Boolean
------------------------------------------------
Specifies localizing the generated quasi-canonical molecular orbitals into Boys type localized molecular orbitals.
* Not supported for molecular systems with symmetry.

:guilabel:`mBoys` Parameter Type: Integer
------------------------------------------------
Specifies localizing the generated quasi-canonical molecular orbitals into mBoys type localized molecular orbitals.
* Not supported for molecular systems with symmetry.

.. code-block:: bdf
    
   mBoys
   2  ! Specifies the powern value

:guilabel:`Cdloc` Parameter Type: Boolean
------------------------------------------------
Specifies localizing the generated quasi-canonical molecular orbitals into Cholesky type localized molecular orbitals.

:guilabel:`Maxcycle` Parameter Type: Integer
------------------------------------------------
Specifies the maximum number of iterations for localization calculation.
* Default value: 3000

:guilabel:`Thresh` Parameter Type: Float
------------------------------------------------
Specifies the two convergence thresholds for localization iteration.
* Default value: 1.d-6 1.d-6

:guilabel:`Highsym` Parameter Type: Boolean
------------------------------------------------
Specifies high-order point group atomic orbitals.

:guilabel:`VSD` Parameter Type: Boolean
------------------------------------------------
Through singular value decomposition (SVD) screening criteria, splits the large basis set virtual molecular orbitals (Virtual MOs) into strongly correlated space and weakly correlated space.

* See example test126.inp for complete input logic

**Test Cases**

:guilabel:`test071.inp`
------------------------------------------------

:guilabel:`test080.inp`
------------------------------------------------

:guilabel:`test086.inp`
------------------------------------------------

:guilabel:`test100.inp`
------------------------------------------------

:guilabel:`test114.inp`
------------------------------------------------

:guilabel:`test126.inp`
------------------------------------------------

:guilabel:`test131.inp`
------------------------------------------------

:guilabel:`test148.inp`
------------------------------------------------

:guilabel:`test150.inp`
------------------------------------------------