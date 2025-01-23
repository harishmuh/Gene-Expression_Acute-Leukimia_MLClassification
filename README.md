![image_ALL_AML](https://github.com/harishmuh/Gene-Expression_Acute-Leukimia_MLClassification/blob/main/ALL%20vs%20AML.PNG?raw=true)

# **Gene Expression Data for Acute Leukimia Classification**

## **Problem and Business Understanding**

### **Context**

**Leukimia**

Leukimia is a type of cancer that affects blood and bone marrow (blood forming tissues). This cancer disrupts the normal function of the healthy blood cells that leads to the overproduction of abnormal white blood cells that crowd out healthy blood cells, which impairs the body's ability to fight infections and can interfere with the production of red blood cells and platelets.
According to the World Health Organization (WHO), leukemia accounts for 2.5% to 3% of all cancer deaths globally. In addition, Global Burden of Disease (GBD) Study 2020 estimates that around 310,000 deaths annually due to leukemia worldwide. The mortality rate of this cancer varies significantly by region and limited access to healthcare facilities often leads to higher mortality rates due to late diagnosis and inadequate treatment.

**Hematopoietic stem cell**

Hematopoietic stem cell is an immature cell that can develop into all type of blood cells, including white blood cells. Hematopoietic stem cells are found in the peripheral blood and the bone marrow. Based on the origins of hematopoietic cells, acute leukimia can be divided into AML (Acute Myeloid Leukimia) and ALL (Acute Lymphoblastic Leukimia). 

![hematopoietic stem cell](https://nci-media.cancer.gov/pdq/media/images/526219-571.jpg)

[Hematopietic cell - National Institute of Health](https://www.cancer.gov/publications/dictionaries/cancer-terms/def/hematopoietic-stem-cell)

**AML** 
* Associated with high expression of genes related to myeloid cells, which are cells that normally turn into red blood cells, platelets, and some types of white blood cells (like neutrophils). 
* The leukemia cells are myeloblasts, which may have special features like Auer rods (tiny needle-like structures seen under a microscope).

**ALL**
* Characterized by genes expressed in the lymphoid cells, which are cells that normally turn into lymphocytes (a type of white blood cell that helps fight infections and builds immunity).
* The leukemia cells are lymphoblasts, which are simpler-looking cells without special features like Auer rods.

**Challenges in differentiating ALL and AML**
  
The appearance and symptoms of ALL and AML are highly similar. Both ALL and AML can present with similar symptoms such as fatigue, frequent infections, fever, bleeding, bruising, and pallor (caused by anemia, low platelets, or dysfunctional white blood cells). Unfortunately, the cure rate is remarkably diminished when ALL therapy is used for AML. Chemotherapy for ALL requires chemical substances that different with AML. In addition,  Thus, accurate distinction of acute leukemias is crucial for successful treatment. Since the mere inspection of the appearance of acute leukemias for classification has significant limitations, more systematic approach of gene expression monitoring is used.


**Microarray Gene Expression Data**

Microarray technology offers a high-throughput, comprehensive snapshot of gene expression levels in cells. AML and ALL origins from distinct blood cell lineages—myeloid and lymphoid progenitors, which express different sets of genes. Microarray analysis can capture these molecular differences, providing a more objective basis for classification. 

### **Objective**
* To build prediction model based on the gene expression dataset (from DNA microarray) for classifying acute leukemia patients into two classes, acute myeloid leukemia (AML) and acute lymphoblastic leukemia (ALL).

### **Analytical Approach and Measurement Metrics**

We want to analyze data to learn about pattern that can differentiate which patients who are more likely to have ALL and who are more likely to have AML based on gene expression data from DNA microarray. Furthermore, we want to build binary classification model that can help distinguish ALL and AML using gene expression data. In this case, we want to focus on classification metric such as accuracy as both class of ALL and AML have similar importance. Accuracy itself can be defined as the ratio of correctly predicted instances to total instances. However, as the dataset tend to moderately imbalance, we will also employ ROC-AUC curve/score metric for additional evaluation criteria and minimize bias. 


### **Data Source**

* The dataset is originated based on scientific publication by [Golub et.al](https://pubmed.ncbi.nlm.nih.gov/10521349/) "Molecular Classification of Cancer: Class Discovery and Class Prediction by Gene Expression Monitoring".
* These datasets contain gene expression measurements corresponding to ALL and AML samples obtained from Bone Marrow and Peripheral Blood of 72 patients. The RNA was extracted from bone marrow for microarray analysis (focused on gene expression which is reflected in RNA)
* Initially, there were 3 datasets containing training and testing datasets, and a dataset consisting of the labels. The dataset contains 7129 features.

## **Results**

**Data Understanding and EDA**
Based on our observation, we can get some understanding of the gene expression dataset as
* The dataset contains 72 rows (number of patients) and 7129 features. The high number of features can cause curse of dimensionality that potentially makes overfitting and inhibit generalization.
* Many features of the dataset have assymetric distribution (3250 features) and 2959 features contain high percentage of extreme values or outliers. These outliers need to be treated before generating machine learning models.
* There were no missing values and duplicate in the dataset. Thus, we can proceed without additional imputation.
* The target labels were not balanced as the proportion ALL and AML (about 71.1%: 28.9%). Therefore, we need to conduct resampling technique to prevent bias toward the major class.

### **Principle Component Analysis**
Principal Component Analysis (PCA) is A dimensionality reduction technique that transforms correlated features into a smaller set of uncorrelated components. We used PCA to treat high number of features (7129) into smaller number.

**PCA Visualization (2D)**

![PCA 2D visual](https://github.com/harishmuh/Gene-Expression_Acute-Leukimia_MLClassification/blob/main/PCA%202D.PNG?raw=true)

**Around 95% of variance is explained by the 31 features**
![Cumulative variance explained by PCA](https://github.com/harishmuh/Gene-Expression_Acute-Leukimia_MLClassification/blob/main/Cumulative%20variance%2031%20PC%20-%20PCA.PNG?raw=true)


### **Model Benchmarking**

**Model Performance (Accuracy and ROC-AUC Score) - Without Tuning**

![Model Performance Before Tuning](https://github.com/harishmuh/Gene-Expression_Acute-Leukimia_MLClassification/blob/main/ML%20classification%20performance%20accuracy%20and%20AUC%20score.PNG?raw=true)



**Accuracy score comparison Before After Hyperparameter tuning**

| Model | Conditions | Train score  | Test score |
| --- | --- | --- | ---|
| XGBoost Classifier | Before Tuning |  0.868 | 0.971 |
| XGBoost Classifier |  After Tuning | 0.893 | 0.971 | 
| LGBM Classifier | Before Tuning |  0.896 | 0.971 |
| LGBM Forest Classifier |  After Tuning | 0.893 | 0.971 | 
| Random Forest Classifier | Before Tuning |  0.871 | 0.941 |
| Random Forest Classifier |  After Tuning | 0.975 | 0.971 | 

**ROC AUC score comparison Before After Hyperparameter tuning**

| Model | Before Tunning | After Tunning | 
| --- | --- | --- | 
| XGBoost Classifier |  0.989 | 0.989 |
| LGBM Classifier | 0.989 | 0.982 | 
| Random Forest Classifier |  0.986 | 0.993 |

**ROC AUC Score Visualization**
![ROC AUC Score 3 models](https://github.com/harishmuh/Gene-Expression_Acute-Leukimia_MLClassification/blob/main/ROC%20AUC%203%20models%20after%20tuning.PNG?raw=true)


**Confusion Matrix**

![Confusion matrix 3 models](https://github.com/harishmuh/Gene-Expression_Acute-Leukimia_MLClassification/blob/main/Confusion%20matrix%203%20models.PNG?raw=true)

**Best Model: Random Forest After Tuning**

**Learning Curve - Accuracy**

![Learning curve accuracy](https://github.com/harishmuh/Gene-Expression_Acute-Leukimia_MLClassification/blob/main/Learning%20curve%20accuracy.PNG?raw=true)

**Learning Curve - ROC AUC Score**

![Learning curve random forest](https://github.com/harishmuh/Gene-Expression_Acute-Leukimia_MLClassification/blob/main/Learning%20curve%20ROC%20AUC%20score.PNG?raw=true)


**SHAP Explanation model**
![SHAP bar](https://github.com/harishmuh/Gene-Expression_Acute-Leukimia_MLClassification/blob/main/SHAP%20Bar%20Plot.PNG?raw=true)

![SHAP Beeswarm](https://github.com/harishmuh/Gene-Expression_Acute-Leukimia_MLClassification/blob/main/SHAP%20swarm%20bee%20plot.PNG?raw=true)



**PC1 Gene Components**

There are many genes that contribute to PC1. We will highlight 10 most important genes that contributes to model. These genes have variety of functions that intersect with mechanisms of leukimia development, such as disrupted cell signaling, metabolism, immune evasion, and uncontrolled proliferation. Some genes (like U82311_at and D86968_at) may serve as novel biomarkers, requiring further investigation to fully understand their roles. These genes and their function can be seen on table below

![Top features PC1](https://github.com/harishmuh/Gene-Expression_Acute-Leukimia_MLClassification/blob/main/PC1%20gene%20components.PNG?raw=true)


| Gene Code                | Gene Name                                    | Gene Function                                                                                      | Role in Model Prediction              |
|--------------------------|----------------------------------------------|----------------------------------------------------------------------------------------------------|---------------------------------------|
| HG2090-HT2152_s_at       | External Membrane Protein, 130 KDa          | Involved in cellular membrane processes.                                                          | Cellular interactions and signaling. |
| U58048_at                | PRSM1 Metallopeptidase 1 (33 kD)            | Enzyme involved in protein degradation and turnover.                                               | Protein turnover in cancer cells.    |
| U34380_rna1_s_at         | TEC (Tyrosine Kinase) Gene                  | Tyrosine kinase implicated in signal transduction pathways.                                        | Cancer cell signaling.               |
| L27584_s_at              | Calcium Channel Beta3 Subunit               | Modulates calcium ion transport in cells.                                                         | Calcium signaling in leukemia cells. |
| U82311_at                | Unknown Protein                             | Unknown function, possibly novel or poorly characterized protein.                                  | Potential marker or unknown role.    |
| HG371-HT26388_at         | Mucin 1, Epithelial, Alt. Splice 9          | Glycoprotein associated with cellular adhesion and immune evasion.                                 | Immune evasion in leukemia cells.    |
| HG2379-HT3996_s_at       | Serine Hydroxymethyltransferase, Cytosolic  | Key enzyme in one-carbon metabolism and nucleotide biosynthesis.                                   | Supports rapid cancer cell growth.   |
| X60188_at                | Extracellular Signal-Regulated Kinase 1     | Critical in the MAPK pathway for cellular growth and survival.                                     | Promotes leukemia progression.       |
| X80754_at                | GTP-binding Protein                         | Involved in signal transduction, regulating cellular processes like growth and differentiation.    | Cancer cell growth and signaling.    |
| D86968_at                | KIAA0213 Gene                               | Gene with partial characterization, possibly linked to cell structure or signaling.                | Potentially supports cancer cell survival. |


**PC7 Gene Components**

PC7 are crucial components after PC1. These PC also contains some genes that relevant to our leukimia model prediction. These genes can be seen on table below. 

![Top features PC7](https://github.com/harishmuh/Gene-Expression_Acute-Leukimia_MLClassification/blob/main/PC7%20gene%20components.PNG?raw=true)
| Gene Code        | Gene Name                            | Gene Function                                                                 | Role in Model Prediction                |
|-------------------|--------------------------------------|-------------------------------------------------------------------------------|-----------------------------------------|
| M13981_at        | INHA Inhibin, alpha                 | Regulates hormone production and inhibits cell growth, linked to cancer progression | Regulation of cell growth in leukemia  |
| X82494_at        | FBLN2 Fibulin 2                     | Involved in extracellular matrix organization and cell adhesion                | ECM remodeling relevant to cancer cells|
| X07384_at        | GLI Glioma-associated oncogene       | Zinc finger transcription factor, regulates cell proliferation and differentiation | Oncogene associated with leukemia      |
| U79266_at        | Clone 23627 mRNA                    | Unknown function                                                              | Potentially linked to leukemia          |
| U84569_at        | YF5 mRNA                            | Unknown function                                                              | Potential role in leukemia development  |
| X17651_at        | MYOG Myogenin (myogenic factor 4)   | Regulates muscle differentiation                                              | May indicate differentiation state in leukemia |
| AF001359_f_at    | DNA mismatch repair protein (hMLH1) | Plays a role in DNA repair and genomic stability                               | Genomic stability linked to cancer prevention|
| L48516_at        | Paraoxonase 3 (PON3)                | Antioxidant enzyme, protects cells from oxidative damage                       | Oxidative stress in cancer progression  |
| V00574_s_at      | c-Ha-ras1 proto-oncogene            | Proto-oncogene, involved in cell signaling and proliferation                   | Key oncogene in leukemia pathogenesis  |
| M32879_at        | CYP11B1 Cytochrome P450 11 beta     | Enzyme involved in steroid metabolism                                         | Hormonal regulation linked to cancer    |


## **Conclusion**
* The complexity and high number of features from Gene expression dataset have been reduced by using PCA technique from 7129 features into 31 features. 
* The prediction model has been built using the post tunning random forest model that shows excellent performance on testing dataset (0.971 accuracy). This result matches the other high performing models, such as XGBoost and LGBM. Additionaly, this model achieves the highest ROC-AUC score (0.993) that also demonstrates high discriminatory power and ensures strong generalization to unseen data.
* The best parameters of this tuned random forest model consist of,
  * n_estimators: 300
  * min_samples_split: 5
  * min_samples_leaf: 1
  * max_features': 'log2'
  * max_depth': 30
  * model__bootstrap': True
* Based on the SHAP explanation model, the most importance features come from principal components of PC1 and PC7.
  * PC1 contain several genes such as HG2090-HT2152_s_at (External Membrane Protein, 130 Kda (Gb:Z22971)), U58048_at (PRSM1 Metallopeptidase 1 (33 kD)), U34380_rna1_s_at (TEC gene extracted from Human protein tyrosine kinase TEC (tec) gene), L27584_s_at (CAB3b mRNA for calcium channel beta3 subunit), U82311_at (GB DEF = Unknown protein mRNA, partial cds), HG371-HT26388_at (Mucin 1, Epithelial, Alt. Splice 9), HG2379-HT3996_s_at (Serine Hydroxymethyltransferase, Cytosolic, Alt. Splice 2), X60188_at (Extracellular signal-regulated Kinase 1 gene)		
X80754_at (GTP-binding protein), D86968_at (KIAA0213 gene, partial cds)
  * PC7 contain several genes such as M13981_at (INHA Inhibin, alpha gene),	X82494_at (FBLN2 Fibulin 2 gene), X07384_at (GLI Glioma-associated oncogene homolog (zinc finger protein) gene),	U79266_at (Clone 23627 mRNA gene), U84569_at (YF5 mRNA gene), X17651_at (MYOG Myogenin (myogenic factor 4) gene), AF001359_f_at (GB DEF = DNA mismatch repair protein (hMLH1) mRNA gene), L48516_at (GB DEF = Paraoxonase 3 (PON3) mRNA gene),	V00574_s_at (genomic clones lambda-[SK2-T2, HS578T]; cDNA clones RS-[3,4, 6]) c-Ha-ras1 proto-oncogene, M32879_at (CYP11B1 Cytochrome P450 11 beta gene)
