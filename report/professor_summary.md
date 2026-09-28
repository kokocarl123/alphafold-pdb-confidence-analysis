\# Mini Structural Bioinformatics Project 專題摘要

\## AlphaFold Confidence vs Experimental Structural Deviation



\### 一、專題目的



本專題以結構生物資訊學方法，探討 AlphaFold 所提供的 residue-level confidence（pLDDT）與實驗蛋白質結構之間的 residue-level structural deviation 是否具有關聯。



專題核心問題為：



\*\*AlphaFold 的 pLDDT 越高，是否代表該 residue 與實驗結構越接近？\*\*



同時進一步探討：即使 AlphaFold 對某些 residue 具有高度信心，是否仍可能因蛋白質存在不同 conformational states，而與特定 experimental structure 出現明顯差異。



\---



\### 二、分析蛋白質



本專題選擇三個蛋白質作為案例：



| Protein | UniProt | Experimental PDB |

|---|---|---|

| Human carbonic anhydrase II (CA2) | P00918 | 2CBA |

| Human thioredoxin (TXN) | P10599 | 1ERT |

| E. coli adenylate kinase (ADK) | P69441 | 4AKE |



資料來源包含 UniProt canonical sequence、Protein Data Bank experimental structures，以及 AlphaFold Protein Structure Database 預測模型。



\---



\### 三、分析流程



1\. 檢查 FASTA、experimental PDB 與 AlphaFold PDB 資料完整性。

2\. 將 UniProt canonical sequence 與 experimental PDB-derived sequence 進行 sequence alignment。

3\. 建立 UniProt position 與 experimental PDB residue 的 residue-level mapping。

4\. 驗證所有 mapped residues 的 amino-acid identity。

5\. 擷取 experimental 與 AlphaFold 對應 residue 的 Cα coordinates。

6\. 使用 Kabsch algorithm 進行 structural superposition，消除整體 translation 與 rotation。

7\. 計算 global Cα RMSD 與 per-residue Cα deviation。

8\. 擷取 AlphaFold pLDDT。

9\. 使用 Spearman correlation 分析 pLDDT 與 residue-level deviation 的關係。

10\. 進一步分析 high-confidence / high-deviation residues 與 ADK domain-level structural pattern。



\---



\### 四、主要結果



經 structural superposition 後：



| Protein | Global Cα RMSD |

|---|---:|

| CA2 | 0.531 Å |

| TXN | 0.384 Å |

| ADK | 7.200 Å |



CA2 與 TXN 的 AlphaFold model 與所選 experimental structures 整體高度接近；ADK 則呈現明顯較大的 structural disagreement。



三個蛋白中，pLDDT 與 Cα deviation 均呈負向 Spearman association：



\- CA2：ρ = −0.421

\- TXN：ρ = −0.481

\- ADK：ρ = −0.585



表示在各蛋白內部，較高 pLDDT 的 residues 整體上傾向具有較小的 structural deviation。



然而，高 pLDDT 並不保證與特定 experimental conformation 重合。例如 ADK residue 149 的 pLDDT 約為 97，但在 superposition 後仍具有約 18.4 Å 的 Cα deviation。



ADK 的 deviation 亦呈現明顯 domain-level pattern：



\- CORE median deviation：約 3.37 Å

\- NMP median deviation：約 9.85 Å

\- LID median deviation：約 10.45 Å



ADK deviation 最高的 top 10% residues 全部位於 NMP 或 LID regions，而 CORE 為 0 個。



\---



\### 五、目前結論



本專題結果顯示，AlphaFold pLDDT 對 residue-level structural agreement 具有一定資訊價值，但 pLDDT 並不能直接視為「與某一特定 experimental structure 的相似程度」。



特別是在 ADK 這類具有明顯 conformational motion 的蛋白質中，即使 AlphaFold 對局部結構具有高度 confidence，仍可能與特定 experimental conformational state 出現大幅 positional disagreement。



因此，解讀 AlphaFold structure 時，需要同時考慮模型 confidence、experimental structural state，以及蛋白質本身的 conformational dynamics。



\---



\### 六、專題中實際建立的能力



本專題實際完成並驗證：



\- Python 資料處理與結構分析

\- UniProt / PDB / AlphaFold DB 資料整合

\- FASTA 與 PDB format 處理

\- Sequence alignment 與 residue mapping

\- Structural coordinate extraction

\- Kabsch structural superposition

\- Global RMSD 與 residue-level structural deviation

\- pLDDT interpretation

\- Spearman correlation

\- Matplotlib scientific visualization

\- Git version control

\- Reproducible project organization

\- 結果 QC、limitations 與 biological interpretation



\---



\### 七、後續延伸



目前版本為可重現的 mini structural bioinformatics project。



後續預計：



1\. 將 ADK AlphaFold model 與不同 experimental conformational states 比較。

2\. 擴增分析 protein / PDB 數量。

3\. 進行 sensitivity analysis。

4\. 增加蛋白質三維結構視覺化。

5\. 將分析流程進一步模組化，提高 reproducibility。

