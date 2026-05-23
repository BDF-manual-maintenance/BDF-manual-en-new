Vibrational general mean field configuration interaction method - VGMFCI module
=======================================================================
Vgmfci perform vibrational general mean field calculations for diatom molecule. The non-adabatic calculate is performed and nuclear wave function is expanded by eigen-fucntion of Kratzer po
tential.

:guilabel:`Emethod` Parameter Type: String
------------------------------------------------
Specify electron structure calculation method. Supported methods are SCF, MCSCF and MRCI.
For MCSCF, molecular orbitals are not optimized in MCSCF level. However, the CAS-CI (complete active space CI) is performed as the full CI calculation.

:guilabel:`Nstate` Parameter Type: integer
-----------------------------------------------
Number of states will be printed and analyzed. 

.. code-block:: bdf

   $COMPASS 
   Title
     H2 Molecule. vgmfci example 
   Basis
    cc-pvdz
   Geometry
    H 0.000  0.000    0.70018162
    H 0.000  0.000   -0.70018162
    X 0.000  0.000    0.80018162
    X 0.000  0.000   -0.80018162
    X 0.000  0.000    0.60018162
    X 0.000  0.000   -0.60018162
   End geometry
   Check
   Unit
    Bohr
   nosymm
   Kratzer
   # maxnu maxj ngausslag re         de
    10   0   50 1.40036324  0.365148     
   $END
   
   $XUANYUAN
   #masspol
   $END
   
   $vgmfci
   emethod
    mcscf  # scf mrci
   maxiter
    1
   Nstate
    20
   $end
   
   $SCF
   RHF
   checklinear
   tollin
    1.d-6
   $END
   
   $MCSCF
   close
    0
   actele
    2
   spin
    1
   roots
    1 40 
   CASCI
   mcvgmfci
   #diagcimat
   $END

 
