Multireference Configuration Interaction - MRCI Module
================================================
The MRCI module is used in conjunction with the DRT module to perform uncontracted MRCI calculations.

**Basic Control Parameters**

:guilabel:`Nrroots` Parameter Type: Integer
------------------------------------------------
Specifies the number of roots for MRCI calculation.

:guilabel:`PrintThresh` Parameter Type: Integer
------------------------------------------------
Default value: 0.05

Specifies the threshold for printing output CSFs.

:guilabel:`Convergence` Parameter Type: Float Array
------------------------------------------------
Default values: 1.D-8, 1.D-6, 1.D-8

Specifies the convergence thresholds for MRCI calculation. Input three floating-point numbers, controlling the energy, wave function, and residual vector convergence thresholds for MRCI iteration respectively.

:guilabel:`Maxiter` Parameter Type: Integer
------------------------------------------------
Specifies the maximum number of MRCI calculation iterations.

:guilabel:`Cipro` Parameter Type: Integer
------------------------------------------------
Specifies calculation of the one-electron reduced density matrix and related properties, such as dipole moment, etc.