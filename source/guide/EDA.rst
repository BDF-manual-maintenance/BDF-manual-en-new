Energy Decomposition Analysis
========================================

Benefiting from BDF's automatic fragmentation engine autofrag, since June 2025, BDF supports energy decomposition analysis for covalent or non-covalent complexes based on sobEDA and sobEDAw methods :cite:`sobEDA`.

In the sobEDA method, given a system XY, the program can automatically divide XY into two fragments X and Y according to the bonding relationship, and then perform calculations on X and Y separately. Then, it combines the local orbitals of X and Y, calculates the electrostatic interaction energy Eels, exchange interaction energy Ex, DFT-related interaction energy Edftc, dispersion correction contribution Edc, solvent correction contribution Esolv, etc. Next, it performs Löwdin orthogonalization on the local orbitals of X and Y, and performs SCF iteration until convergence. The two-step energy change can also obtain the Pauli repulsion energy Erep and orbital interaction energy Eorb. Thus, the interaction energy between X and Y is decomposed into a sum of contributions from different sources. Here, X and Y can not only be two complete molecules (i.e., they have only non-covalent interactions), but also two fragments that are bonded to each other. There can even be more than one covalent/coordinate bond between the fragments, but in this case, the fragments must be open-shell, i.e., the broken bonds must be treated as free radicals rather than being saturated with buffer atoms/hydrogen atoms/PHO atoms. The program can also analyze systems consisting of more than two fragments, but regardless of whether it is Eels, Ex, Edftc, Edc, Esolv, Erep, or Eorb, only the total contribution of all fragment pairs can be obtained, and not the interaction energy of a specific pair of fragments. In the sobEDAw method, based on the sobEDA analysis, the program further allocates Edftc to Edc and Ex in a certain proportion, and the resulting value can serve as an approximation of the SAPT energy decomposition analysis result, but sobEDAw is only suitable for analyzing non-covalent interactions.

Compared to other energy decomposition analysis methods, sobEDA and sobEDAw have the following advantages:

* The computation cost is relatively low, similar to that of a single-point DFT energy calculation for the whole system, and is 1-2 orders of magnitude faster than LED, SAPT, etc.;
* The sum of various terms obtained from energy decomposition analysis is strictly equal to the interaction energy between two fragments;
* The physical meaning of the decomposition result is clear.

In addition, the sobEDA and sobEDAw methods implemented in BDF have the following extra advantages over the original sobEDA and sobEDAw methods:

* As a byproduct of the calculation, localized molecular orbitals (LMOs) for fragments and the total system can be obtained, which can be visualized or further analyzed;
* When analyzing non-covalent interactions, the program can automatically identify which molecules make up the system without manually specifying atomic numbers;
* Supports a subset of range-separated functionals (CAM-B3LYP, LC-BLYP);
* Can analyze the contribution of solvent to interaction energy.

On the other hand, the sobEDA and sobEDAw methods (or their BDF implementations) also have some limitations:

* Does not take into account the deformation energy of fragments (the energy increase brought about by the fragment changing from its equilibrium structure to its structure in the complex), as well as the contributions of zero-point energy, enthalpy correction and entropy correction. Users who need these contributions should calculate them manually according to their definitions;
* sobEDA only has meaningful results when used with theoretical levels such as B3LYP-D3 that combine a functional without dispersion correction + dispersion correction. sobEDAw does not have this limitation, but to avoid self-fitting empirical parameters as much as possible, it is still recommended to use GB3LYP-D3 unless there are special requirements;
* sobEDAw involves three empirical parameters (c, a, r) related to the theoretical level, and there are limited theoretical levels that have been fitted with these parameters in the literature; if used in conjunction with other theoretical levels, the parameters need to be fitted manually;
* BDF implements sobEDA and sobEDAw methods that do not currently support composite basis (CB) calculations;
* BDF implements sobEDA and sobEDAw methods for a limited set of functionals; see :ref:`BDF-supported exchange-correlation functionals<XCFunctional>` for details. Note that "support" here does not mean the program can automatically select the correct sobEDAw empirical parameters, so users usually still need to manually input these parameters.

sobEDA example: Analyzing the interaction energy between two HC groups in acetylene
--------------------------------------------

This example comes from Table 5 of the original literature sobEDA :cite:`sobEDA`:

.. code-block:: bdf

  $autofrag
  method
  sobeda
  fragdef
  0 4 0 -4 # charge1 spinmult1 charge2 spinmult2. Negative spinmult indicates beta spins
  1,2 # fragment 1: atom 1,2 (HC)
  3,4 # fragment 2: atom 3,4 (CH)
  $end
  
  $compass
  title
    sobEDA analysis of acetylene (GB3LYP-D3/def2-TZVP)
  geometry
        H          -0.00000000      -0.00000000       1.66144913
        C          -0.00000000      -0.00000000       0.59846873
        C           0.00000000       0.00000000      -0.59846873
        H           0.00000000       0.00000000      -1.66144913
  end geometry
  basis
  def2-TZVP # for non-covalent complexes, diffuse basis sets (e.g. def2-TZVPD) are recommended
  mpec+cosx # not necessary for molecules of this size, but helpful for large systems
  $end
  
  $xuanyuan
  $end
  
  $scf
  # even if the total system is closed-shell, if some of the fragments
  # are open-shell, one should still write UKS instead of RKS here
  uks
  dft
  gb3lyp
  d3 # dispersion correction is necessary for sobEDA. "D3" defaults to B-J damping, not zero damping
  $end
  
  $localmo
  flmo # necessary even if the user does not need LMOs
  $end

Note that since HC is a shell layer, it is necessary to specify which atoms each fragment contains using the fragdef keyword in the autofrag module (otherwise, the program does not know which bond(s) to break), as well as the charge and spin multiplicity of each fragment. The atomic numbers of each fragment are separated by commas, and consecutive atomic numbers can be written in the form "start number-end number", such as "1,3,6-10,12-13,15" is equivalent to "1,3,6,7,8,9,10,12,13,15". Although the ground state of HC is a doublet, the triple bond formed between the two HC groups requires exciting HC to a quartet state, so the energy decomposition analysis is performed based on the quartet state HC. The spin multiplicity of the second HC fragment is written as -4 instead of 4 to indicate that its single-electron spin direction is opposite to that of the first fragment (with positive spin multiplicity). If analyzing non-covalent complexes, i.e., cases where the program can complete fragmentation without breaking any bonds, fragdef does not need to be specified, but users must check whether the fragments and spin multiplicities automatically identified by the program are reasonable; if not, they still need to use the fragdef keyword to specify the charge and spin multiplicity of each fragment, as well as their atomic composition.

The program first calls the autofrag module for fragmentation and then performs SCF calculations on the two fragments in sequence, with results output to *.fragment1.out and *.fragment2.out respectively; the local orbitals (LMO) of the fragments are output to *.fragment1.flmo.molden and *.fragment2.flmo.molden, which can be opened by any orbital visualization software that supports the molden format. Next, the program performs three global SCF calculations: the first does not orthogonalize pFLMO or perform SCF iterations; the second orthogonalizes pFLMO but does not perform SCF iterations; and the third both orthogonalizes pFLMO and performs SCF iterations until convergence. Finally, the program integrates the results of the five calculations and outputs energy decomposition analysis results:

.. code-block:: bdf

      *** Energy decomposition analysis result ***
  Total interaction      energy:   -275.837 kcal/mol
  Of these:
   - Electrostatic       energy:   -143.258 kcal/mol
   - Exchange-repulsion  energy:    248.908 kcal/mol
     Of these:
      > Exchange         energy:    -58.637 kcal/mol
      > Repulsion        energy:    307.545 kcal/mol
   - Orbital interaction energy:   -336.724 kcal/mol
   - Correlation         energy:    -44.763 kcal/mol
     Of these:
      > DFT correlation        :    -43.872 kcal/mol
      > Dispersion correction  :     -0.891 kcal/mol
   - Implicit solvation  energy:      0.000 kcal/mol

Among them:
* Electrostatic energy refers to electrostatic potential energy; for this system, the electrostatic potential energy is a very large negative absolute value, because the two carbon atoms are very close, and the electron cloud of one carbon atom will penetrate into the interior of the electron cloud of another carbon atom, feeling the nuclear attraction of the latter.
* Exchange-repulsion energy refers to the exchange-repulsion energy, which can be used to characterize the contribution of steric hindrance. At first glance, acetylene as a very small molecule has no steric hindrance, but this is because only in cases where non-covalent interactions have a smaller contribution to orbital effects, the exchange-repulsion energy can dominate and manifest as steric repulsion; when studying the direct bonding of two atoms, there is indeed steric hindrance between the atoms, it's just that the orbital effect is more significant than the steric contribution, leading to many cases where the contribution of steric hindrance to covalent bonds is not discussed.
* Orbital interaction energy refers to the orbital effect energy, which has a large absolute negative value for covalent bonds and a relatively small absolute value for non-covalent interactions. This term represents the contributions of covalent effects, polarization effects, and charge transfer effects to the interaction energy. Note that the boundaries between these three contributions are blurred and may even overlap, so it is not possible to further decompose the orbital effect energy into covalent, polarizing, and charge transfer contributions within the sobEDA framework.
* Correlation energy refers to electron correlation energy, which includes contributions from the functional's own electron correlation energy (DFT correlation) and dispersion correction. For functionals such as B3LYP that cannot describe dispersion effects, the dispersion correction contribution obtained by sobEDA can be considered as the contribution of dispersion energy to interaction energy. However, note that the absolute value of the dispersion energy obtained in this way is often smaller than that obtained from some high-level energy decomposition analysis methods (such as LED, SAPT), and the difference is often even more pronounced for covalently bonded systems due to the non-uniqueness of the definition of dispersion energy at short inter-fragment distances :cite:`LED` , which does not imply that sobEDA's results are unreliable. To obtain results comparable to those from LED and SAPT, one should use sobEDAw (see below).
* Implicit solvation energy refers to the contribution of implicit solvent models to the interaction energy; for calculations without an implicit solvent model, this term is exactly 0 (even if the user adds explicit solvent with QM or MM). Note that when users consider non-electrostatic terms of solvent models or use SMD solvent models, this term includes entropy contributions from solvation and should be referred to as the contribution of dissolution free energy (rather than solvation energy) to interactions.

It can be seen that sobEDA can not only be used to analyze non-covalent compounds and fragments connected by single bonds, but also to analyze fragments connected by multiple bonds. Similarly, the bonds formed between fragments are not limited to one. However, when there are covalent/coordinate bonds between fragments, users must take into account the unpaired electrons introduced by bond breaking when defining the spin multiplicity of the fragments, and even check whether the fragments have converged to the correct wave function (e.g., through spin population analysis), simply confirming that the parity of the spin multiplicity is correct is not enough.

If the keyword ``molden`` is added to the $scf block in the above input file, the pFLMO orbitals and FLMO orbitals of the total system can also be generated, which are *.global.pflmo.molden and *.global.cflmo.molden respectively. This allows for a comparison of the LMOs with pFLMOs and FLMOs of molecular fragments. The difference in shape between LMO and pFLMO represents the effect of orbital orthogonalization, corresponding to the exchange energy in the above energy decomposition analysis; the difference in shape between pFLMO and FLMO represents the effect of inter-fragment orbital mixing, corresponding to the orbital interaction energy in the above energy decomposition analysis. For example, the following figure shows the shape change process of a p-orbital of HC in the above example, where it can be seen that orbital orthogonalization causes LMO to shrink slightly away from another fragment, while orbital mixing causes LMO to delocalize towards another fragment. The calculated FLMO is not a pure pi-orbital but contains a small amount of sigma component, which is normal. Adding the ``pipek`` keyword in the $localmo module can often help obtain pure sigma and pi orbitals by using Pipek-Mezey localization, but still cannot strictly guarantee that sigma and pi do not mix.

.. figure:: /images/HCCH-FLMO.png
   :width: 800
   :align: center


sobEDAw Example: Water Dimer Energy Decomposition Analysis
--------------------------------------------

For non-covalent complexes that are not affected by the limitations described at the beginning of this chapter, it is recommended to use sobEDAw instead of sobEDA for energy decomposition analysis. For example, the following input file decomposes the interaction energy between two water molecules in a water dimer in liquid water environment, taking into account the contribution of surrounding water molecules using an implicit solvent model:

.. code-block:: bdf

  $autofrag
  method
  sobedaw
  # parameters for GB3LYP/6-31+G(d,p)
  # JPCA, 2023, 127, 7023
  # Note that this is a rather poor level of theory for sobEDAw;
  # for accurate results, diffuse TZ basis sets are recommended
  sobedaw_c
  0.638
  sobedaw_a
  0.124
  sobedaw_r
  3.523
  $end
  
  $compass
  geometry
     O         -1.65542061     -0.12330038      0.00000000
     O          1.24621244      0.10268870      0.00000000
     H         -0.70409026      0.03193167      0.00000000
     H         -2.03867273      0.75372294      0.00000000
     H          1.57598558     -0.38252146     -0.75856129
     H          1.57598558     -0.38252146      0.75856129
  end geometry
  basis
  6-31+G(d,p)
  $end
  
  $xuanyuan
  $end
  
  $scf
  rks
  dft
  gb3lyp
  d3
  solvent
  water
  $end
  
  $localmo
  flmo
  $end

It can be seen that the parameters c, a, and r required by sobEDAw need to be looked up from the original literature of sobEDA or fitted manually. The analysis result is

.. code-block:: bdf

  sobEDAw parameters:
  c = 0.63800000
  a = 0.12400000
  r = 3.52300000
  dEdc/dEels = 0.05722559
  w = 1.00000000
  
      *** Energy decomposition analysis result ***
  Total interaction      energy:     -4.775 kcal/mol
  Of these:
   - Electrostatic       energy:    -11.166 kcal/mol
   - Exchange-repulsion  energy:      9.531 kcal/mol
   - Orbital interaction energy:     -2.801 kcal/mol
   - Dispersion          energy:     -2.964 kcal/mol
   - Implicit solvation  energy:      2.625 kcal/mol

It can be seen that sobEDAw does not separately print the exchange energy and repulsion energy, nor does it print DFT-related energies. This is because sobEDAw distributes the DFT-related energies in a certain proportion to the exchange-repulsion energy and dispersion energy. From the results of energy analysis, it can be found that electrostatic interaction is the main reason why water molecules can form dimers, but most of the contribution from electrostatic attraction is offset by steric repulsion; orbital interactions (i.e., the covalent component of hydrogen bonds) and dispersion effects also make a significant contribution to the binding energy between water molecules. Due to the high dielectric constant of water, the liquid water environment has a shielding effect on the electrostatic effects between water molecules, weakening their mutual interaction, so the solventization energy contributes to the interaction energy in the form of repulsion.

When the system consists of three or more molecules, each energy item printed by the program is the sum of all pairwise interactions between molecules. For example, for a system ABC consisting of three molecules A, B, and C, the program can print the total electrostatic energy and total exchange-repulsion energy between all molecules in the system, but cannot print the electrostatic energy or exchange-repulsion energy between molecules A and B specifically. However, due to the characteristics of sobEDA and sobEDAw, when there is no implicit solvent, the total intermolecular electrostatic energy of ABC equals the sum of the electrostatic energies calculated separately for AB, BC, and CA. Therefore, in this case, performing energy decomposition on the AB complex without C can yield the electrostatic energy between A and B. However, for items other than electrostatic energy and exchange energy (note that this does not include exchange-repulsion energy), the sum of contributions calculated separately for AB, BC, and CA is not equal to the total contribution of ABC. In some cases, users may want to decompose the interaction energy between AC as a whole and B; in such cases, use the fragdef keyword to define AC as one fragment and B as another.

Similar to sobEDA, in the sobEDAw method analysis, it is also possible to plot the fragments LMO, pFLMO, FLMO of the system, visually showing the shape change of local orbitals during interaction, which will not be repeated here.
