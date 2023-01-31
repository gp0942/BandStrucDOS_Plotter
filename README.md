# BandStrucDOS_Plotter
This workbook uses the pymatgen electronic_structure module to generate plots of the density of states and band structures from VASP output, specifically vasprun.xml files.This workbook was compiled in Jan. 23 by Gordon Peterson (Argonne National Lab, gpeterson@anl.gov) in Python 3.9.12 with Pymatgen 2022.11.7 (www.pymatgen.org). VASP5 was used for DFT calculations.

NOTE: You will need 2 distinct vasprun.xml files. 
1. 'vasprunDOS.xml' should come from a ncf calculation (ICHARG = 11) on a gamma-centered kpoint grid (KSPACING = ...) using the tetrahedron method (ISMEAR = -5).
2. 'vasprunBS.xml' should come from a ncf calculation (ICHARG = 11) on a high symmetry path of k-points (e.g. created by vaspkit or manually from Brillouin Zone and named 'KPOINTS') using ISMEAR = 0. If you try to generate a DOS from this vasprun.xml file it will not be correct!!!

Generally, the workflow to produce these files will look like this:
1. Generate a POSCAR and POTCAR corresponding to the structure of interest.
2. Copy an INCAR file from a prior calculation. Set the tags for geometry optimization using ISMEAR = 0 or -5 for metals or insulators appropriately. Set KSPACING to a reasonable value (e.g. 0.1-0.2), and check IBZKPOINTS to see if N k-points is reasonable. Run a self consistent calculation (ICHARG = 2).
3. Copy the resulting CONTCAR geometry back to the POSCAR, and run further scf calculations until good convergence. Finally, turn off the ionic steps and run a last scf calculation using ISMEAR = -5. This calculation produces the most correct Total Energy.
4. Now that you have produced a converged CHGCAR, you will run two non-self consistent calculations with ICHARG = 11. The first one will generate the DOS. Leave ISMEAR = -5 and increase the k-point density to increase the fidelity of the DOS. When the calculation is done, save the resulting vasprun.xml file as vasprunDOS.xml
5. The second ncf calculation will generate the band structure. Create a KPOINTS file containing the high symmetry points through the Brillouin Zone, either manually or by using a tool like vaspkit (example: https://www.vasp.at/wiki/index.php/KPOINTS). Change ISMEAR = 0, run the calculation, and save the resulting vasprun.xml file as vasprunBS.xml. 
6. You are now ready to run this notebook. Copy the vasprunDOS.xml, vasprunBS.xml, and KPOINTS files to the working directory for this notebook.
