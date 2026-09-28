\# AlphaFold Confidence and Experimental Structural Deviation:

\## A Residue-Level Structural Comparison of Three Proteins



\### Mini Structural Bioinformatics Project



\---



\## Abstract



AlphaFold provides both predicted protein structures and residue-level confidence estimates through the predicted Local Distance Difference Test (pLDDT). However, high prediction confidence does not necessarily imply that a predicted structure will closely match every experimentally determined protein conformation.



This exploratory structural bioinformatics project examined the relationship between AlphaFold pLDDT and residue-level structural disagreement with selected experimental structures. Three proteins were analyzed: human carbonic anhydrase II (CA2), human thioredoxin (TXN), and Escherichia coli adenylate kinase (ADK).



Canonical UniProt sequences were aligned with experimental PDB-derived sequences to establish residue-level correspondence. Matched Cα atoms were then used for Kabsch structural superposition, global Cα RMSD calculation, and per-residue structural deviation analysis. AlphaFold pLDDT values were compared with residue-level Cα deviations using Spearman correlation.



CA2 and TXN showed close global agreement with their selected experimental structures, with Cα RMSDs of 0.531 Å and 0.384 Å, respectively. ADK showed substantially greater disagreement, with a global Cα RMSD of 7.20 Å. All three proteins showed negative residue-level associations between pLDDT and Cα deviation, with Spearman correlations ranging from −0.421 to −0.585.



ADK nevertheless contained multiple residues with high pLDDT but large structural deviations from the selected experimental structure. These deviations were strongly concentrated in the NMP and LID regions rather than the CORE region. The results illustrate that pLDDT provides useful local confidence information but should not be interpreted as a direct measure of agreement with a particular experimental conformational state.



\---



\## 1. Introduction



AlphaFold has made high-quality predicted protein structures widely available and provides a residue-level confidence score, pLDDT, for evaluating local prediction confidence. A high pLDDT value generally indicates that AlphaFold has high confidence in the local structural environment of a residue.



However, experimentally determined protein structures are not necessarily static representations of a single universal conformation. Proteins may adopt different structures depending on ligand binding, biochemical state, crystallization conditions, or intrinsic conformational dynamics. Therefore, disagreement between an AlphaFold prediction and one experimental PDB structure should not automatically be interpreted as prediction failure.



The main research question of this project was:



\*\*How does AlphaFold residue-level confidence relate to residue-level structural deviation from selected experimental protein structures?\*\*



A secondary question was whether residues with high pLDDT necessarily show close structural agreement with a particular experimental conformation.



Three proteins were selected as small case studies:



| Protein | UniProt | Experimental PDB | Selected state |

|---|---|---|---|

| Human carbonic anhydrase II (CA2) | P00918 | 2CBA | Native |

| Human thioredoxin (TXN) | P10599 | 1ERT | Reduced |

| E. coli adenylate kinase (ADK) | P69441 | 4AKE | Unligated |



The goal was not to construct a general benchmark of AlphaFold accuracy, but to develop and validate a reproducible residue-level structural comparison workflow and examine how confidence and experimental structural disagreement relate in different proteins.



\---



\## 2. Methods



\### 2.1 Data collection



Canonical protein sequences were obtained from UniProt. Experimental structures were obtained from the Protein Data Bank (PDB), and corresponding AlphaFold models were obtained from the AlphaFold Protein Structure Database.



The three proteins had canonical sequence lengths of:



\- CA2: 260 residues

\- TXN: 105 residues

\- ADK: 214 residues



Raw FASTA files, experimental PDB structures, and AlphaFold PDB structures were stored separately to preserve the original data.



\### 2.2 Residue mapping



Directly assuming that a UniProt sequence position is identical to a PDB residue number can produce incorrect structural comparisons because PDB structures may contain missing residues, alternative residue numbering, insertion codes, or sequence differences.



Therefore, the UniProt canonical sequence was aligned with the amino-acid sequence extracted from each experimental PDB chain.



Residue-level mapping tables were then generated between UniProt sequence positions and experimental PDB residues.



For CA2, the UniProt sequence contained 260 residues whereas the selected experimental structure contained 258 modeled residues. Sequence alignment showed that UniProt positions 1 and 2 lacked corresponding modeled residues in 2CBA.



All mapped residues in the three proteins showed matching amino-acid identities:



| Protein | Canonical length | Mapped residues | Amino-acid mismatches |

|---|---:|---:|---:|

| CA2 | 260 | 258 | 0 |

| TXN | 105 | 105 | 0 |

| ADK | 214 | 214 | 0 |



This mapping step ensured that corresponding Cα atoms represented the same biological residues before structural comparison.



\### 2.3 Cα coordinate extraction and structural superposition



Cα coordinates were extracted from both the experimental and AlphaFold PDB structures.



For each mapped residue, the analysis table contained:



\- UniProt residue position

\- experimental PDB residue number

\- experimental Cα coordinates

\- AlphaFold Cα coordinates

\- AlphaFold pLDDT



The AlphaFold structure was then superimposed onto the experimental structure using the Kabsch algorithm.



The Kabsch procedure removes rigid-body translation and rotation while preserving internal structural differences. This step is necessary because two identical structures may have very different raw coordinates if they are located or oriented differently in three-dimensional space.



\### 2.4 Global and residue-level structural deviation



After superposition, global Cα RMSD was calculated as:



RMSD = sqrt(mean of squared distances between corresponding Cα atoms)



Per-residue Cα deviation was also calculated for every mapped residue.



Global RMSD describes overall structural agreement, whereas per-residue deviation identifies local regions of structural disagreement.



\### 2.5 pLDDT and correlation analysis



AlphaFold pLDDT values were extracted from the B-factor field of the AlphaFold PDB files.



Spearman rank correlation was used to examine the residue-level relationship between pLDDT and Cα deviation.



Spearman correlation was selected because the analysis focused on monotonic association and did not assume a linear relationship between pLDDT and structural deviation.



\### 2.6 Exploratory high-deviation analysis



For each protein, the 90th percentile of the Cα deviation distribution was used as an exploratory protein-specific threshold for identifying relatively high-deviation residues.



This threshold was used only as an analytical method for selecting the highest-deviation residues within each protein. It was not treated as a biological definition of structural error.



For ADK, residues were additionally divided into CORE, NMP, and LID regions to examine whether structural disagreement showed a domain-level pattern.



\---



\## 3. Results



\### 3.1 Global structural comparison



After Kabsch superposition, CA2 and TXN showed close overall agreement with their selected experimental structures.



| Protein | Paired Cα atoms | Global Cα RMSD |

|---|---:|---:|

| CA2 | 258 | 0.531 Å |

| TXN | 105 | 0.384 Å |

| ADK | 214 | 7.200 Å |



CA2 and TXN both had global RMSDs below 1 Å. In contrast, ADK showed a substantially larger global RMSD of 7.20 Å.



!\[Global Cα RMSD](../results/figures/global\_rmsd\_summary.png)



The high RMSD of ADK was not caused only by one or two extreme residues. Its mean residue-level Cα deviation was approximately 5.83 Å, the median was approximately 4.37 Å, and the 75th percentile was approximately 7.12 Å.



These values indicate that substantial structural disagreement was distributed across many ADK residues.



\### 3.2 Relationship between pLDDT and structural deviation



All three proteins showed negative residue-level associations between AlphaFold pLDDT and Cα deviation.



| Protein | Spearman rho | Nominal p-value |

|---|---:|---:|

| CA2 | −0.421 | 1.58 × 10^-12 |

| TXN | −0.481 | 2.06 × 10^-7 |

| ADK | −0.585 | 4.47 × 10^-21 |



Within each protein, residues with higher pLDDT therefore generally tended to show smaller deviations from the selected experimental structure.



However, this relationship was not absolute.



!\[ADK pLDDT vs Cα deviation](../results/figures/ADK\_plddt\_vs\_deviation.png)



For example, ADK residue 149 had a pLDDT of approximately 97 but a Cα deviation of approximately 18.4 Å after structural superposition.



This demonstrates that high AlphaFold local confidence does not necessarily imply close positional agreement with a particular experimental structure.



\### 3.3 Domain-level pattern in adenylate kinase



The ADK residue-level deviation profile showed two major regions of large structural disagreement.



Domain-level analysis produced the following results:



| Region | Residues | Mean deviation | Median deviation | Maximum deviation |

|---|---:|---:|---:|---:|

| CORE | 133 | 3.42 Å | 3.37 Å | 6.27 Å |

| NMP | 38 | 8.93 Å | 9.85 Å | 15.85 Å |

| LID | 43 | 10.56 Å | 10.45 Å | 18.44 Å |



The NMP and LID regions therefore showed substantially larger structural deviations than the CORE region.



!\[ADK structural deviation](../results/figures/ADK\_deviation\_by\_domain.png)



The 90th percentile of the ADK deviation distribution was approximately 13.17 Å.



Among residues within the highest 10% of the deviation distribution:



\- LID contained 13 residues

\- NMP contained 9 residues

\- CORE contained 0 residues



Therefore, the largest AlphaFold–4AKE structural disagreements were concentrated entirely within the NMP and LID regions in this analysis.



\---



\## 4. Discussion



The analysis produced two complementary observations.



First, pLDDT showed a consistent negative residue-level association with Cα deviation across all three proteins. This suggests that AlphaFold local confidence contains useful information about residue-level structural agreement: residues with higher confidence tended, on average, to agree more closely with the selected experimental structures.



Second, the ADK case demonstrates an important limitation of interpreting pLDDT as a direct measure of experimental structural agreement.



ADK showed generally high pLDDT values while simultaneously showing a global Cα RMSD of 7.20 Å from the selected 4AKE structure. Several high-confidence residues also displayed Cα deviations greater than 10 Å.



Importantly, these deviations were not randomly distributed. They were concentrated in the NMP and LID regions, while the CORE region showed substantially smaller deviations.



This pattern is consistent with a structured domain-level conformational difference rather than isolated random residue-level disagreement.



However, the current analysis does not establish which conformational state the AlphaFold ADK model most closely resembles. Comparison with additional experimental structures representing different ADK conformations would be required before drawing that conclusion.



The results therefore support a distinction between two concepts:



\*\*AlphaFold confidence\*\* describes confidence in the predicted local structural environment.



\*\*Experimental structural agreement\*\* describes how closely that prediction matches a particular experimentally observed conformational state.



These quantities are related but are not equivalent.



\---



\## 5. Limitations



This project is an exploratory case study involving only three proteins. The results therefore cannot be generalized to all AlphaFold predictions.



Only one experimental PDB structure was selected for each protein. Experimental structures may represent different ligand-bound, biochemical, crystallization, or conformational states.



Therefore, a large deviation from one selected PDB structure does not necessarily indicate that the AlphaFold prediction is incorrect.



Residue-level observations are also not statistically independent because neighboring residues are connected through the protein sequence and three-dimensional structure. The reported Spearman p-values should therefore be interpreted as nominal values, with greater emphasis placed on the direction and magnitude of the correlations.



The structural comparison focused on Cα atoms and backbone-level disagreement. Side-chain geometry was not evaluated.



Finally, the 90th percentile high-deviation threshold was an exploratory, protein-specific criterion rather than a biologically validated structural-error cutoff.



\---



\## 6. Conclusion



This project developed a residue-level workflow for comparing AlphaFold confidence with experimental structural deviation.



Careful UniProt–PDB residue mapping was required before structural comparison because experimental residue numbering and sequence positions cannot be assumed to correspond directly.



CA2 and TXN showed strong overall agreement between AlphaFold and the selected experimental structures, whereas ADK showed substantially greater structural disagreement.



Across all three proteins, higher pLDDT generally corresponded to lower residue-level Cα deviation. However, ADK demonstrated that residues with high pLDDT can still show large positional disagreement with a specific experimental conformation.



The concentration of the largest ADK deviations within the NMP and LID regions further suggests that conformational context is important when interpreting AlphaFold confidence and structural accuracy.



Future work will compare ADK with additional experimental conformational states, expand the protein dataset, and investigate whether domain motion and conformational flexibility systematically explain high-confidence / high-deviation cases.



\---



\## Data and Reproducibility



All raw input data, processed residue mappings, residue-level structural comparison tables, summary statistics, figures, and analysis notebooks are organized within the project repository.



Main analysis notebook:



`notebooks/01\_data\_check.ipynb`



Main processed structural comparison files:



`data/processed/CA2\_structure\_comparison.csv`



`data/processed/TXN\_structure\_comparison.csv`



`data/processed/ADK\_structure\_comparison.csv`



Main summary tables:



`results/tables/rmsd\_summary.csv`



`results/tables/correlation\_summary.csv`



`results/tables/ADK\_domain\_summary.csv`

## References

1. Jumper J, Evans R, Pritzel A, et al. Highly accurate protein structure prediction with AlphaFold. *Nature*. 2021;596:583–589. doi:10.1038/s41586-021-03819-2.

2. Varadi M, Anyango S, Deshpande M, et al. AlphaFold Protein Structure Database: massively expanding the structural coverage of protein-sequence space with high-accuracy models. *Nucleic Acids Research*. 2022;50(D1):D439–D444. doi:10.1093/nar/gkab1061.

3. Kabsch W. A solution for the best rotation to relate two sets of vectors. *Acta Crystallographica Section A*. 1976;32:922–923. doi:10.1107/S0567739476001873.

4. Whitford PC, Miyashita O, Levy Y, Onuchic JN. Conformational transitions of adenylate kinase: switching by cracking. *Journal of Molecular Biology*. 2007.

