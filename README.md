\# AlphaFold Confidence vs Experimental Structural Deviation



\## Overview



This mini structural bioinformatics project investigates the relationship between AlphaFold residue-level confidence (pLDDT) and structural disagreement with experimentally determined protein structures.



Rather than assuming that residue numbering is directly comparable between UniProt and PDB files, the workflow first establishes residue-level correspondence through sequence alignment. Matched Cα atoms are then used for structural superposition and residue-level comparison.



Three proteins were selected as small case studies:



| Protein | UniProt | Experimental PDB | Experimental state |

|---|---|---|---|

| Human carbonic anhydrase II (CA2) | P00918 | 2CBA | Native |

| Human thioredoxin (TXN) | P10599 | 1ERT | Reduced |

| E. coli adenylate kinase (ADK) | P69441 | 4AKE | Unligated |



\## Research Question



How does AlphaFold residue-level confidence relate to residue-level structural deviation from selected experimental protein structures?



A secondary question is whether high pLDDT necessarily implies close agreement with a particular experimental conformation.



\## Workflow



The analysis consists of the following steps:



1\. Retrieve canonical sequences from UniProt, experimental structures from the PDB, and AlphaFold models.

2\. Perform quality control on sequence lengths, modeled residues, and Cα atoms.

3\. Align UniProt sequences with experimental PDB-derived sequences.

4\. Build residue-level UniProt–PDB mappings and verify amino-acid identity.

5\. Extract corresponding experimental and AlphaFold Cα coordinates.

6\. Perform Cα-based Kabsch structural superposition.

7\. Calculate global Cα RMSD and per-residue Cα deviation.

8\. Extract AlphaFold pLDDT values.

9\. Evaluate the residue-level relationship between pLDDT and Cα deviation using Spearman correlation.

10\. Examine high-confidence / high-deviation residues and domain-level patterns.



\## Key Results



\### Global structural comparison



After Kabsch superposition:



| Protein | Paired residues | Global Cα RMSD |

|---|---:|---:|

| CA2 | 258 | 0.531 Å |

| TXN | 105 | 0.384 Å |

| ADK | 214 | 7.200 Å |



CA2 and TXN showed close overall agreement with the selected experimental structures, whereas ADK showed substantially larger structural disagreement.



!\[Global RMSD summary](results/figures/global\_rmsd\_summary.png)



\### pLDDT vs residue-level structural deviation



All three proteins showed negative residue-level associations between pLDDT and Cα deviation:



| Protein | Spearman rho |

|---|---:|

| CA2 | -0.421 |

| TXN | -0.481 |

| ADK | -0.585 |



Thus, higher-confidence residues generally tended to have lower structural deviation.



However, high pLDDT did not guarantee close agreement with a particular experimental conformation.



For example, ADK residue 149 had approximately:



\- pLDDT = 97

\- Cα deviation = 18.4 Å



This indicates high AlphaFold local confidence but substantial positional disagreement with the selected 4AKE experimental structure.



!\[ADK pLDDT vs deviation](results/figures/ADK\_plddt\_vs\_deviation.png)



\### ADK domain-level structural disagreement



ADK showed a clear domain-level pattern:



| Region | Median Cα deviation |

|---|---:|

| CORE | 3.37 Å |

| NMP | 9.85 Å |

| LID | 10.45 Å |



All residues within the top 10% of the ADK deviation distribution were located in the NMP or LID regions:



\- LID: 13 residues

\- NMP: 9 residues

\- CORE: 0 residues



This suggests that the large AlphaFold–4AKE structural disagreement is concentrated in specific mobile regions rather than being uniformly distributed across the protein.



!\[ADK domain deviation](results/figures/ADK\_deviation\_by\_domain.png)



\## Interpretation



The results suggest that pLDDT provides useful residue-level confidence information: within each protein, higher pLDDT generally corresponds to lower structural deviation.



However, pLDDT should not be interpreted as a direct measure of agreement with every possible experimental conformation.



ADK is an important example. Despite generally high pLDDT values, the AlphaFold model shows large deviations from the selected 4AKE structure in specific regions. These differences are concentrated in the NMP and LID regions, suggesting a structured conformational difference rather than random residue-level disagreement.



Comparison with additional experimental conformational states would be required before determining which experimental state the AlphaFold model most closely resembles.



\## Limitations



This is a small exploratory study involving only three proteins and should not be interpreted as a general benchmark of AlphaFold accuracy.



Each AlphaFold model was compared with one selected experimental PDB structure. Experimental structures can represent specific conformational, ligand-binding, oxidation, or crystallization states, so structural disagreement does not necessarily indicate prediction error.



Residue-level observations are also not fully independent because neighboring residues are connected in sequence and three-dimensional structure. Therefore, Spearman p-values should be interpreted cautiously.



The current workflow focuses on Cα-based backbone comparison and does not evaluate side-chain geometry.



\## Repository Structure



```text

alphafold-pdb-confidence-analysis/

├── data/

│   ├── raw/

│   │   ├── fasta/

│   │   ├── experimental/

│   │   └── alphafold/

│   └── processed/

├── metadata/

├── notebooks/

│   └── 01\_data\_check.ipynb

├── results/

│   ├── figures/

│   └── tables/

├── report/

├── requirements.txt

└── README.md

## Reproducibility

### Tested environment

- Python 3.13.15
- pandas 3.0.6
- NumPy 2.5.3
- SciPy 1.18.1
- Matplotlib 3.11.2
- Biopython 1.88
- JupyterLab 4.6.4

Install dependencies with:

```bash
python -m pip install -r requirements.txt

