# **Gene Expression Data for Acute Leukemia Classification**

![image_ALL_AML](https://github.com/harishmuh/Gene-Expression_Acute-Leukimia_MLClassification/blob/main/ALL%20vs%20AML.PNG?raw=true)



## **Problem and Business Understanding**

### **Context**

**Leukimia**

Leukemia is a type of cancer that affects blood and bone marrow (blood forming tissues). This cancer disrupts the normal function of the healthy blood cells that leads to the overproduction of abnormal white blood cells that crowd out healthy blood cells, which impairs the body's ability to fight infections and can interfere with the production of red blood cells and platelets.
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
  
The appearance and symptoms of ALL and AML are highly similar. Both ALL and AML can present with similar symptoms such as fatigue, frequent infections, fever, bleeding, bruising, and pallor (caused by anemia, low platelets, or dysfunctional white blood cells). Unfortunately, the cure rate is remarkably diminished when ALL therapy is used for AML. Chemotherapy for ALL requires chemical substances that are different from AML. In addition, accurate distinction of acute leukemias is crucial for successful treatment. Since the mere inspection of the appearance of acute leukemias for classification has significant limitations, a more systematic approach of gene expression monitoring is used.


**Microarray Gene Expression Data**

Microarray technology offers a high-throughput, comprehensive snapshot of gene expression levels in cells. AML and ALL origins from distinct blood cell lineages—myeloid and lymphoid progenitors, which express different sets of genes. Microarray analysis can capture these molecular differences, providing a more objective basis for classification. 

### **Objective**
* To build prediction model based on the gene expression dataset (from DNA microarray) for classifying acute leukemia patients into two classes, acute myeloid leukemia (AML) and acute lymphoblastic leukemia (ALL).

### **Analytical Approach and Measurement Metrics**

We want to analyze data to learn about patterns that can differentiate which patients are more likely to have ALL and which are more likely to have AML based on gene expression data from DNA microarray. Furthermore, we want to build a binary classification model that can help distinguish ALL and AML using gene expression data. In this case, we want to focus on classification metric such as accuracy as both class of ALL and AML have similar importance. Accuracy itself can be defined as the ratio of correctly predicted instances to total instances. However, as the dataset tend to moderately imbalance, we will also employ ROC-AUC curve/score metric for additional evaluation criteria and minimize bias. 


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

**Confusion Matrix**

![Confusion matrix 3 models](https://github.com/harishmuh/Gene-Expression_Acute-Leukimia_MLClassification/blob/main/Confusion%20matrix%203%20models.PNG?raw=true)

**ROC AUC Score Visualization**

![ROC AUC Score 3 models](https://github.com/harishmuh/Gene-Expression_Acute-Leukimia_MLClassification/blob/main/ROC%20AUC%203%20models%20after%20tuning.PNG?raw=true)

### **Model Performance Analysis (After Hyperparameter Tuning)**

**XGBoost Classifier**

* Before Tuning: Training accuracy of 0.868 and testing accuracy of 0.971 indicate strong generalization. Furthermore, the ROC-AUC score of 0.989 reflects excellent class discrimination, making it highly reliable for classification.
* After Tuning: Training accuracy increased to 0.893, while testing accuracy remained at 0.971, showing no overfitting and consistent performance. The ROC-AUC score remained steady at 0.989, indicating that tuning did not significantly impact its discriminatory performance.

**LGBM Classifier**

* Before Tuning: Training accuracy of 0.896 and testing accuracy of 0.971 demonstrate strong initial performance. The ROC-AUC of 0.989 highlights its robust ability to differentiate between classes.
* After Tuning: Training accuracy slightly decreased to 0.893, with testing accuracy staying at 0.971, suggesting reduced overfitting. However, the ROC-AUC decreased slightly to 0.982, indicating a slight decline in its ability to discriminate between classes.

**Random Forest Classifier**

* Before Tuning: Training accuracy of 0.871 and testing accuracy of 0.941 indicate moderate underfitting. The ROC-AUC of 0.986 is relatively strong but still lower than XGBoost or LGBM models.
* After Tuning: Training accuracy significantly improved to 0.975, and testing accuracy increased to 0.971, aligning closely with the other models. The ROC-AUC rose to 0.993, making it the highest among all models and showing a marked improvement in class discrimination.



**Best Model: Random Forest After Tuning**

We select the **post-tuning random forest** as the final model due to the excellent performance of training and testing accuracy that matches XGBoost and LGBM, with the highest ROC-AUC (0.993) that demonstrates high discriminatory power and ensures strong generalization to unseen data.


### **Learning Curve**
Learning curve is a graphical representation that shows how a models's performance changes overtime or with amount of training dataset. 

**Learning Curve - Accuracy**

![Learning curve accuracy](https://github.com/harishmuh/Gene-Expression_Acute-Leukimia_MLClassification/blob/main/Learning%20curve%20accuracy.PNG?raw=true)

* The model displays high variance and its overfitting the data. This has been showed by the presence of the gap between the training score and cross validation score. However, this overfitting is expected as the dataset size is small and the model has high complexity (7129 features). The high complexity of the model is the reason we want to simplify it using PCA.
* Additionaly, The cross-validation accuracy starts lower for small training sizes but shows a slight upward improvement trend as the training set size increases. We notice after dataset of 32, we can see improvement trend.

**Learning Curve - ROC AUC Score**

![Learning curve random forest](https://github.com/harishmuh/Gene-Expression_Acute-Leukimia_MLClassification/blob/main/Learning%20curve%20ROC%20AUC%20score.PNG?raw=true)

* Similar to the learning curve using accuracy, the ROC-AUC learning curve also shows overfitting as the dataset is complex with 7129 features.
* However, the cross validation scores shows improvement overtime as long as the training set size increases. The learning curve suggests that as training data increases, the model achieves more stable performance (~0.95 ROC AUC). This indicates that the model is beginning to generalize better despite the dataset's challenges.


### **SHAP Explanation model**

SHAP or Shapley Additive exPlanations it's a method designed to help us interpret the machine learning models. To put it simple, SHAP values tell how much each feature in the dataset contributes to the prediction of the model - whether it is pushing the prediction higher or pulling it down.

![SHAP bar](https://github.com/harishmuh/Gene-Expression_Acute-Leukimia_MLClassification/blob/main/SHAP%20Bar%20Plot.PNG?raw=true)

**Bar Plot Insights**

This bar plot SHAP focusses specifically on the positive class (AML). 

**Top most important features**
* **PC1**: This feature has the highest mean SHAP value (~0.14), indicating it is the most influential in determining whether a patient has AML (1) or ALL (0).
* **PC7**: This feature is the second most important, with a mean SHAP value of ~0.05.
* Both PC1 and PC7 capture critical variance in the gene expression data, making them key predictors in the model for distinguishing between AML and ALL.
* The other features, such as PC31, PC23, etc, have lower mean values (~0.03 or less), contributing relatively small to the model's prediction.

![SHAP Beeswarm](https://github.com/harishmuh/Gene-Expression_Acute-Leukimia_MLClassification/blob/main/SHAP%20swarm%20bee%20plot.PNG?raw=true)

**Beeswarm plot insights**

The beeswarm plot highlights how each feature impacts individual predictions, showing the spread of SHAP values for each feature across all samples.

**Top important features**
* **PC1** and **PC7** are again identified as the most critical features, with PC1 being the dominant factor influencing predictions.
* PCI Shows a wide range of SHAP values, spanning both positive and negative impacts on model predictions. The gradient in the feature value (from blue to pink) suggests that higher values of PC1 push predictions towards AML (1), while lower values lean towards ALL (0).
* PC7 Also shows variability in SHAP values, but with a smaller range compared to PC1.

### **PC gene components**

Based on previous step, we know that both PC1 and PC7 are the most important features for model prediction. In this step, we want to breakdown the original features or gene components that contributes to principal components, PC1 and PC7.


**PC1 Gene Components**

There are many genes that contribute to PC1. We will highlight 10 most important genes that contributes to model. These genes have variety of functions that intersect with mechanisms of leukimia development, such as disrupted cell signaling, metabolism, immune evasion, and uncontrolled proliferation. Some genes (like U82311_at and D86968_at) may serve as novel biomarkers, requiring further investigation to fully understand their roles. These genes and their function can be seen on table below

![Top features PC1](https://github.com/harishmuh/Gene-Expression_Acute-Leukimia_MLClassification/blob/main/PC1%20gene%20components.PNG?raw=true)

**PC1 Gene Components**

| Gene Code | Gene Name | Gene Function | Biological Relevance to Leukemia or Cancer | Reference |
|---|---|---|---|---|
| HG2090-HT2152_s_at | CD163 (provisional annotation) | Helps immune cells remove damaged blood components. | Immune cells can influence cancer growth and the tumor environment. | [Etzerodt and Moestrup, 2013](https://pubmed.ncbi.nlm.nih.gov/?term=Etzerodt+Moestrup+CD163+2013) |
| U58048_at | PRSM1 | Helps break down proteins. | Protein breakdown is important for normal cell function and cancer cell survival. | [Scott et al., 1996](https://pubmed.ncbi.nlm.nih.gov/8863740/) |
| U34380_rna1_s_at | TEC | Helps transmit signals inside cells. | Abnormal cell signaling can contribute to blood cancer development. | [Mano et al., 1995](https://pubmed.ncbi.nlm.nih.gov/7678927/) |
| L27584_s_at | Calcium channel beta subunit (identity unconfirmed) | Helps regulate calcium movement into cells. | Calcium signaling helps control cell growth and survival. | No relevant gene-specific study identified. |
| U82311_at | Uncharacterized transcript | Function has not been confirmed. | No relevant study identified. |
| HG371-HT26388_at | MUC1 (provisional annotation) | Helps protect cell surfaces and regulate cell signaling. | MUC1 has been studied as a potential therapeutic target in AML. | [Stroopinsky et al., 2013](https://pubmed.ncbi.nlm.nih.gov/23867470/) |
| HG2379-HT3996_s_at | SHMT1 (provisional annotation) | Helps cells produce building blocks for DNA. | Changes in cell metabolism have been studied in AML. | [SHMT1 expression in AML, 2021](https://pubmed.ncbi.nlm.nih.gov/33867731/) |
| X60188_at | MAPK3 (ERK1) | Helps control cell growth and signaling. | Abnormal growth signaling is common in cancer. | [Roberts and Der, 2007](https://doi.org/10.1038/sj.onc.1210422) |
| X80754_at | DRG2 | Helps regulate basic cellular processes. | No relevant gene-specific leukemia or cancer study identified. | No relevant gene-specific study identified. |
| D86968_at | MAP3K4 | Helps cells respond to stress signals. | Stress-response signaling can influence cancer cell behavior. | [NCBI sequence annotation](https://www.ncbi.nlm.nih.gov/nuccore/D86968.2) |


**PC7 Gene Components**

PC7 is a crucial component after PC1. This PC also contains some genes that are relevant to our leukemia model prediction. These genes can be seen on table below. 

![Top features PC7](https://github.com/harishmuh/Gene-Expression_Acute-Leukimia_MLClassification/blob/main/PC7%20gene%20components.PNG?raw=true)

**PC7 Gene Components**

| Gene Code | Gene Name | Gene Function | Biological Relevance to Leukemia or Cancer | Reference |
|---|---|---|---|---|
| M13981_at | INHA | Helps regulate hormone-related signaling. | Related signaling pathways can influence cell growth and cancer development. | [NCBI sequence annotation](https://www.ncbi.nlm.nih.gov/nuccore/M13981.1) |
| X82494_at | FBLN2 | Helps organize the material surrounding cells. | Changes in this material can affect tumor growth and spread. | [NCBI Gene: FBLN2](https://www.ncbi.nlm.nih.gov/gene/2199) |
| X07384_at | GLI1 | Controls genes involved in cell development. | Abnormal GLI signaling has been studied in AML and other cancers. | [GLI signaling in AML, 2015](https://pubmed.ncbi.nlm.nih.gov/25745035/) |
| U79266_at | SAC3D1 (provisional annotation) | Helps organize structures involved in cell division. | Abnormal cell division is a feature of cancer. | [NCBI Gene: SAC3D1](https://www.ncbi.nlm.nih.gov/gene/29901) |
| U84569_at | CFAP410 (provisional annotation) | Supports cellular structures involved in cilia function. | No relevant gene-specific leukemia or cancer study identified. | No relevant gene-specific study identified. |
| X17651_at | MYOG | Controls genes involved in muscle development. | No relevant gene-specific leukemia or cancer study identified. |
| AF001359_f_at | MLH1 (provisional annotation) | Helps repair DNA errors. | Defects in DNA repair can contribute to cancer development. | [NCBI accession annotation](https://www.ncbi.nlm.nih.gov/nuccore/AF001359) |
| L48516_at | PON3 (provisional annotation) | Helps protect cells from certain harmful chemical reactions. | Oxidative stress can influence cancer development. | [UniProt: PON3](https://www.uniprot.org/uniprotkb/Q15166) |
| V00574_s_at | HRAS (provisional annotation) | Helps control cell growth and signaling. | Abnormal RAS signaling is associated with several cancers, including blood cancers. | [RAS signaling in acute leukemia](https://pubmed.ncbi.nlm.nih.gov/?term=RAS+mutations+acute+leukemia) |
| M32879_at | CYP11B1 (provisional annotation) | Helps produce steroid hormones. | No relevant gene-specific leukemia or cancer study identified. | No relevant gene-specific study identified. |

## **Conclusion**
* The complexity and high number of features from Gene expression dataset have been reduced by using PCA technique **from 7129 features into 31 features**. 
* The prediction model has been built using the **post-tuning random forest model** that shows excellent performance on the testing dataset **(0.971 accuracy)**. This result matches the other high-performing models, such as XGBoost and LGBM. Additionally, this model achieves the **highest ROC-AUC score (0.993)** that also demonstrates high discriminatory power and ensures strong generalization to unseen data.
* The best parameters of this tuned random forest model consist of,
  * n_estimators: 300
  * min_samples_split: 5
  * min_samples_leaf: 1
  * max_features': 'log2'
  * max_depth': 30
  * model__bootstrap': True
* Based on the SHAP explanation model, the most important features come from the principal components of **PC1** and **PC7**.
  * PC1 contain several genes such as HG2090-HT2152_s_at (External Membrane Protein, 130 Kda (Gb:Z22971)), U58048_at (PRSM1 Metallopeptidase 1 (33 kD)), U34380_rna1_s_at (TEC gene extracted from Human protein tyrosine kinase TEC (tec) gene), L27584_s_at (CAB3b mRNA for calcium channel beta3 subunit), U82311_at (GB DEF = Unknown protein mRNA, partial cds), HG371-HT26388_at (Mucin 1, Epithelial, Alt. Splice 9), HG2379-HT3996_s_at (Serine Hydroxymethyltransferase, Cytosolic, Alt. Splice 2), X60188_at (Extracellular signal-regulated Kinase 1 gene)		
X80754_at (GTP-binding protein), D86968_at (KIAA0213 gene, partial cds)
  * PC7 contain several genes such as M13981_at (INHA Inhibin, alpha gene),	X82494_at (FBLN2 Fibulin 2 gene), X07384_at (GLI Glioma-associated oncogene homolog (zinc finger protein) gene),	U79266_at (Clone 23627 mRNA gene), U84569_at (YF5 mRNA gene), X17651_at (MYOG Myogenin (myogenic factor 4) gene), AF001359_f_at (GB DEF = DNA mismatch repair protein (hMLH1) mRNA gene), L48516_at (GB DEF = Paraoxonase 3 (PON3) mRNA gene),	V00574_s_at (genomic clones lambda-[SK2-T2, HS578T]; cDNA clones RS-[3,4, 6]) c-Ha-ras1 proto-oncogene, M32879_at (CYP11B1 Cytochrome P450 11 beta gene)


## References

1. Golub, T. R., Slonim, D. K., Tamayo, P., et al. (1999). Molecular classification of cancer: Class discovery and class prediction by gene expression monitoring. *Science, 286*(5439), 531–537. [https://doi.org/10.1126/science.286.5439.531](https://doi.org/10.1126/science.286.5439.531)
2. Scott, I. C., Halila, R., Jenkins, J. M., et al. (1996). Molecular cloning, expression and chromosomal localization of a human gene encoding a 33 kDa putative metallopeptidase (PRSM1). *Gene, 174*(1), 135–143. [https://doi.org/10.1016/0378-1119(96)00510-0](https://doi.org/10.1016/0378-1119(96)00510-0)
3. Mano, H., Mano, K., Tang, B., et al. (1995). Expression and characterization of TEC tyrosine kinase in hematopoietic cells. [PubMed search for the original TEC study](https://pubmed.ncbi.nlm.nih.gov/?term=Mano+TEC+tyrosine+kinase+hematopoietic+1995)
4. Stroopinsky, D., Rosenblatt, J., Ito, K., et al. (2013). MUC1 is a potential target for the treatment of acute myeloid leukemia stem cells. *Cancer Research, 73*(17), 5569–5579. [https://doi.org/10.1158/0008-5472.CAN-13-0677](https://doi.org/10.1158/0008-5472.CAN-13-0677)
5. Evaluation of LKB1 and Serine-Glycine Metabolism Pathway Genes (SHMT1 and GLDC) Expression in AML. (2021). [PubMed record](https://pubmed.ncbi.nlm.nih.gov/33867731/)
6. Roberts, P. J., & Der, C. J. (2007). Targeting the Raf-MEK-ERK mitogen-activated protein kinase cascade for the treatment of cancer. *Oncogene, 26*, 3291–3310. [https://doi.org/10.1038/sj.onc.1210422](https://doi.org/10.1038/sj.onc.1210422)
7. Expression of Hedgehog Pathway Mediator GLI Represents a Negative Prognostic Marker in Human Acute Myeloid Leukemia and Its Inhibition Exerts Antileukemic Effects. (2015). [PubMed record](https://pubmed.ncbi.nlm.nih.gov/25745035/)
8. Etzerodt, A., & Moestrup, S. K. (2013). CD163 and inflammation: Biological, diagnostic, and therapeutic aspects. [PubMed search for the CD163 review](https://pubmed.ncbi.nlm.nih.gov/?term=Etzerodt+Moestrup+CD163+2013)

## **Assets**
* [Jupyter notebook](https://github.com/harishmuh/Gene-Expression_Acute-Leukimia_MLClassification/blob/main/Gene_expression_AcuteLeukimia_classification.ipynb)
* [Google colab notebook](https://github.com/harishmuh/Gene-Expression_Acute-Leukimia_MLClassification/blob/main/Gene_Expression_Acute_Leukimia_Classification.ipynb)
* [Google colab notebook - Heatmap and Clustermap significant gene visualization](https://github.com/harishmuh/Gene-Expression_Acute-Leukimia_MLClassification/blob/main/Heatmap_visualization_Acute_leukemia_AML_VS_ALL.ipynb)
