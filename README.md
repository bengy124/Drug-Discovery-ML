# Drug Discovery Project
This drug discovery tool is a beginner-level Quantitative Structure-Activity Relationship (QSAR) model that reveals the relationship between the structural properties of chemical compounds and biological activities. It utilizes the Chembl database which contains the bioactivity data of over 2 million compounds, comprising over 76,000 documents and 1.2 million assays with data spanning over 13,000 targets, 1,800 cells, and 33,000 indications.

### Data Collection and Pre-processing
To begin you search for a “target” being either a target protein or organism that the drug/molecule will act on and we want to see the modulatory effect the drug compound will have on the target whether it activates or inhibits the protein/organism. For this example, I used Aromatase which is an enzyme part of the cytochrome P450 which is linked with breast cancer tissue formation. After selecting a target it will search and gather data from Chembl and filter for Inhibitory Concentration at 50% (IC50), meaning the drug will elicit a 50% inhibition of the target protein, and output a dataset. The data also features the Standard Value of a molecule which quantifies the potency. The lower the value the better as it refers to the concentration of the drug a person must consume in order to elicit the IC50.

The dataset then goes through pre-processing in which labels are assigned to the molecules to show their bioactivity. Molecules with a Standard Value less than 1000 nM will be labelled active, if greater than 10,000 then labelled as inactive and between 1000 and 10,000 will be labelled as intermediate. After it creates a new dataset containing only the Molecule ID, Standard Value, Bioactivity, and Canonical Smiles which is a notation that represents the chemical structure of the molecule.

### EDA
With the pre-processed dataset it can begin the Exploratory Data Analysis (EDA) in which it will calculate Lipinski descriptors for each molecule. These descriptors were created by Christopher Lipinski who was a scientist at Pfizer that came up with rules for the assessment of drug-likeness based on the pharmacokinetic profile of absorption, distribution, metabolism, and excretion (ADME). He created a set of rules that can help predict how successful an oral drug can be based on the following: a Molecular Weight less than 500 Daltons, an Octanol-water partition coefficient aka LogP (refers to solubility) less than 5, no more than 5 hydrogen bond donors, and no more than 10 hydrogen bond acceptors. For a drug to be orally bioavailable it can not break more than one of these rules.

After calculating and assigning the Lipinski descriptors to each molecule the Standard Value/IC50 is converted to pIC50 by using a negative log transformation to achieve a more even distribution. A new dataset is created combining the bioactivity dataset with the Lipinski descriptors and changing the Standard Value to the new pIC50 values. Using this new dataset it can create a Frequency plot of active and inactive bioactivity classes, a scatter plot of Molecular Weight vs LogP and box plots comparing the bioactivity class against the Lipinski descriptors as well as pIC50. A statistical analysis using the Mann-Whitney U Test is also performed which tests for the statistical significance of the pIC50 variable and Lipinski descriptors by comparing the active and inactive classes.

For this example results from the Mann-Whitney U Test showed that the pIC50 were statistically significantly different as the active and inactive bioactivity classes showed different distributions and for the Lipinski descriptors the  MW, Hdonor and Hacceptor had statistically significant differences showing different distributions with the bioactivity classes while the LogP had no statistically significant difference as the distribution with the bioactivity classes were the same.

### Dataset Prep and Model Building
To prepare the data for running a regression model it will use PeDEL to calculate the molecular descriptors of each molecule. PaDEL removes salts and small organic acids to purify the chemical structures and outputs the changes in canonical smile notation and also assigns a PubChem fingerprint to each molecule. The PubChem fingerprint encodes molecular fragments information with 881 binary digits, this helps to describe the unique structural features of the molecules and allows the machine learning model to learn from the unique molecular properties of each fingerprint to create a model that can distinguish between compounds that are active and inactive. We want to see which fingerprints are essential for creating a potent drug with minimal side effects. Lastly, in order the create a regression model using Random Forrest Regressor it will set the PubChem fingerprints as the X input and the pIC50 values as the Y output. Then remove all the low variance values and split the data in an 80/20 ratio. Once running Random Forrest Regressor it will produce an r<sup>2</sup> value and will show a scatterplot of the experimental vs predicted pIC50 values to visualize the results.

#### *Disclaimer - This code is not my own original work, all credits go to the [Data Professor](https://www.youtube.com/@DataProfessor) YouTube channel and his video tutorials for this project. My intentions for this project were to purely get practice and begin learning Python for bioinformatics and data science applications.* 

#### Sources
- [Data Professor Bioinformatics Project from Scratch YouTube](https://www.youtube.com/watch?v=plVLRashaA8&list=PLtqF5YXg7GLlQJUv9XJ3RWdd5VYGwBHrP)
- https://pubs.rsc.org/en/content/articlelanding/2018/RA/C7RA10979B
- https://www.cancer.org/cancer/breast-cancer/risk-and-prevention/aromatase-inhibitors-for-lowering-breast-cancer-risk.html
- https://dev.drugbank.com/guides/terms/lipinski-s-rule-of-five
- https://codeocean.com/explore/capsules?query=tag:data-curation
- https://github.com/chaninlab/estrogen-receptor-alpha-qsar/blob/master/02_ER_alpha_RO5.ipynb
- https://machinelearningmastery.com/nonparametric-statistical-significance-tests-in-python/
