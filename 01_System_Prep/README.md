## PDBs
* Beinaglutide (6X18)
* Efpeglenatide (1JRJ)
* Semaglutide (7KI0)
* Taspoglutide (7KI1)
* Tirzepatide (7FIM)
* VEGFR2 (3V2A)

Albiglutide was modeled using SWISS-MODEL, and dulaglutide, lixisenatide, and efpeglenatide were modeled with AlphaFold.

## Protein Prep
In the Schrödinger Protein Preparation Wizard:
* Assign Bond orders: reassign all (CCD)
* Replace Hydrogens: Zero-order bonds to metals and disulfide bonds
* Add terminal oxygens to protein chains
* Fill in missing loops
* Generate het states (pH 7.4 +/- 2.0)

## Receptor Grid Generation for VEGFR2
1. Delete junk atoms - Chain B, Chain C, Ligand B, waters, HET ATMs
2. Use following settings in the protein prep wizard:
* Cap termini
* Fill in missing side chains
* Optimize H-bond assignments
* Minimize and delete waters
3. Create the receptor grid using the following settings:
* Center: Centroid of selected residues (360 CYS)
*** CHECK THIS OUT LATER**

## Protein Fragmenting NEED TO VERIFY SOME STUFF HERE
The GLP drugs being studied are pretty big. Because of the size limit of Schrödinger's ligand docking feature, we would not be able to dock these. For this reason, we fragmented them into three- to four-amino-acids-long fragments and later six- to eight-amino-acids-long fragments
1. Load the GLP into Schrödinger
2. The amino acids are attached together by peptide bonds, so use the SMARTS code [NX3][CX3](=[OX1])[#6] to split the bonds.
3. Split the C and N atom. The goal is to divide the drug into structures with the same number of atoms.
4. The end result should be a few structures that are 40-60 atoms, and made up of identifiable amino acids
5. These fragments were used as ligands when docking

## Small Ligand Docking
**Note:** This part of the project was done on Maestro Schrödinger for fragments containing standard residues. For the drugs with nonstandard residues, Haddock was used (see below).
**Ligand Preparation**
Prep the ligands using the LigPrep feature on Maestro with the following settings:
* Force Field: CPLS4
* Generate possible states at pH 7.30 +/- 2.000 using Epik Classic (**Note:** the pH value will differ based on what you are trying to achieve)
* Desalt and Generate Tautomer
* Retain specific chiralties
* Generate at most 3 per ligand

**Docking**
1. Select the receptor grid generator file for VEGFR2
2. Select the protein and the prepped ligands
3. Use the Ligand Docking feature with these settings:
* Output
  * Write out at most 3 poses per ligand
  * perform post docking minimization
    * Number of poses per ligand to include: 5
    * Threshold for rejecting minimized pose: 0.50 kcal/mol  
* Settings
  * Write XP descriptor information
  * Sample nitrogen inversion
  * Sample ring conformations
  * Bias sampling of torsions for: all predefined functional groups
  * Add Epik state penalities to docking score
  * Reward intramolecular hydrogen bonds
  * Enhance planarity of conjugated pi groups

## Haddock Docking
**Note:** This step requires information about the binding pose, which we already had. The ligand interaction diagram (LID) is also needed for all fragments being docked. 
1. Ensure that all PDBs are cleaned (see Protein Prep above)
2. Use the residue numbers from the LID as the active residues for VEGFR2
3. Identify the amino acids in your fragment - these are the active residues for the drug.

## Rosetta Docking
Haddock was being problematic due to the nonstandard residues again. For this reason, Rosetta was run on a Vast AI GPU. 
$conda create -n rosetta -c https://conda.rosettacommons.org -c conda-forge rosetta
$conda activate rosetta





