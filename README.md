# ECONTACT
ECONTACT (Energetic CONTributions of Amino acid residues and its possible Cross-Talk) is a new approach to identify potential cross-talks between amino acid residues of protein pocket involved in ligand binding. This approach utilizes per-residue energy terms derived from multiple protein-ligand complexes as variables for principal component analysis (PCA) and identifies top cross-talks which can be tested in any protein-ligand complex.

**Algorithm**

![alt text](https://github.com/Prasanth-Kumar87/ECONTACT/blob/bf4200dd4a7f61c02936da4de439e77f2eb825ce/algorithm.jpg)

The ECONTACT approach transforms the per-residue energy terms derived from multiple protein-ligand complexes into orthogonal space using PCA to identify the most variable amino acid residues involved in cross-talk to promote ligand binding. Further, the cross-talks identified by ECONTACT can be examined using in silico mutagenesis data generated based on the mutations on single amino acids and double amino acids following the protocol of the double mutant cycle. Wilcoxon signed-rank test (matched pairs) is performed to assess whether the two amino acids participating in the cross-talk contribute non-additive effects in comparison with the total impact caused by double mutations (Consult publication for more information). Cross-terms with non-additive effects can be analyzed using molecular dynamics simulations to verify whether there are correlative/anti-correlative motions captured between amino acid residues of cross-talks. Finally, the jPDF metric which quantifies the strength of the ligand interactions with the two amino acids of cross-terms computes the presence of cross-talk in any ligand molecules complexed with the same protein. For more details, please refer the publication

**Publication**

**Kumar SP**, Patel CN, Rawal RM, Pandya HA (2020). Energetic contributions of amino acid residues and its cross‐talk to delineate ligand‐binding mechanism. _Proteins: Structure, Function, and Bioinformatics_, 88(9), 1207-1225. https://onlinelibrary.wiley.com/doi/abs/10.1002/prot.25894

**Example**

This example demonstrates how ECONTACT approach can be used to identify the cross-talks of beta-lactamase enzyme complexed with different inhibitors. GEMDOCK (AMBER-like) and OPLS3e force fields were employed to validate this approach. The iGEMDOCK program was used to derive per-residue energy terms for ligands bound to six beta-lactamase crystal structures (3n6i, 3n8l, 3n8s, 3nbl, 3nck and 3vff) and retained only those energy terms (columns) which scored energy value in least 80 % of the dataset (i.e. 80 % of 6 beta-lactamase complexes = 4.80, round-off to 4 complexes). This refined dataset is used to compute cross-talks (blac.data) comprising 37 per-residue energy terms.

**Data preparation for executing ECONTACT.py script**

In addition to the blac.data, three different csv (comma separated files) are also needed to be kept inside the working directory.

1. The first csv file is energy.csv which contains the specific tag “Dummy” followed by energy terms (e.g. V-M-CYS-83) in the same column leading to a single column with 38 rows (“Dummy” tag + 37 energy terms).
2. The second csv file (energy_terms_index.csv) is similar to the first csv (energy.csv) but contains the specific tag “Energy_term” followed by “Dummy” tag and energy terms resulting in 39 rows. This second file can be easily created by copying the first file contents.
3. The third csv file (pc.csv) list the indexes of the Principal Components (PCs) to be generated for the supplied blac.data (e.g. PC1, PC2,..PCn). Here, n is equal to the total number of energy terms (i.e. 37 energy terms provide 37 PCs). Specifying the PC index other than the exact n value will generate incorrect PCs.

**Instructions to run ECONTACT.py script**

1. Download the blac.data and three csv files (energy.csv, energy_terms_index.csv, pc.csv) and keep in the same directory containing the python script (econtact.py).
2. Run the econtact.py script on the terminal: python econtact.py. The econtact.py script will show the top ten per-residue energy terms and the top two potential cross-talks in the terminal screen (V-M-THR-253: V-S-ILE-117 and V-M-THR-253:H-M-THR-253) with its respective Eigen vectors/loadings computed using PCA. Note down the per-residue energy terms of the cross-talks to compute the jPDF metric for the validation test.

![alt text](https://github.com/Prasanth-Kumar87/ECONTACT/blob/bf4200dd4a7f61c02936da4de439e77f2eb825ce/jpdfpy.png)

**Data preparation for executing jPDF.py script**

jPDF.py script requires two csv files (Training_jPDF.csv and Testing_jPDF.csv) to execute the jPDF calculations. The 6 beta-lactamase-ligand complexes were regarded as the training set which contained the values of per-residue energy terms (V-M-THR-253 and V-S-ILE-117) of the first cross-term (V-M-THR-253:V-S-ILE-117) in two columns of the csv. Similarly, a validation set using five PDB entries (3n7w, 3n8r, 3nc8, 3nde and 3ndg) is composed and chosen only those corresponding per-residue energy terms of the training set (see table below).

![alt text](https://github.com/Prasanth-Kumar87/ECONTACT/blob/8c8824cbff0e86434bf8c8844c121e01f553192f/Table.PNG)

**Instructions to run jPDF.py script**

Run the jPDF.py script on the terminal: python jPDF.py. The jPDF.py script will output the calculated jPDF value of the supplied validation set of 5 blaC-ligand complexes. The jPDF metric quantifies how much strength the bound ligand is contributing to its interactions with two amino acids of cross-terms. The jPDF value close to 1 indicates the very close observation of cross-talks present in the modelling set. The jPDF results indicate that the 3nde (jPDF = 0.6; forth complex) possess 60 % probability of the presence of cross-talk in its structure. The first and fifth complexes (3n7w and 3ndg) attained jPDF value > 1 illustrating the higher strength of intermolecular interactions of its bound ligand with the two amino acid residues of cross-talk comparatively than the jPDF support set.

![alt text](https://github.com/Prasanth-Kumar87/ECONTACT/blob/6bea748d7dc3a635611ba71a04985269dc7baac6/jpdfpy_1.png)

**Customizing econtact.py and jPDF.py script for new dataset**

Edit the econtact.py script and jPDF.py according to your needs. The file name of the data (e.g. blac.data) should be specified inside the script. Accordingly, the three associate csv files viz. energy.csv, energy_terms_index.csv, pc.csv have to be modified following the formatting given in the example files. Similarly, the two input csv files (Training_jPDF.csv and Testing_jPDF.csv) of jPDF.py have to be edited according to the results of econtact.py. Users may contact us if they need assistance in the calculations by sending an email to the contributor.

**Installation (any OS)**

Python 2.7 or later with matplotlib, numpy and pandas dependencies

**Downloads**

The econtact and jPDF python scripts along with the example set can be downloaded here.

**Contributor and help in execution:**

Prasanth Kumar (prasanthbioinformatics[at]gmail.com)
