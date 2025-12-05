# Pediatric-Pain-Analgesia-Analysis
Analysis and visualizations related to SHC Protocol SPO2101R, Posterior Spinal Fusion (PSF) Analgesia Study

# 🔬 Pediatric Pain Analgesia Analysis: Opioid Sparing Strategies

## Project Focus: Continuous Epidural Analgesia for Scoliosis Surgery

This repository contains the data engineering pipeline and reproducible analysis related to the study: **"Continuous epidural analgesia for scoliosis correction surgery: does it reduce opioid requirements, complications and length of stay?"**

As the primary Data Engineer and Analyst, this project showcases expertise in rigorous EMR data acquisition, multi-step standardization, clinically-informed imputation, and statistical preparation for published research.

<img width="688" height="516" alt="image" src="https://github.com/user-attachments/assets/1816565a-6e94-4647-b9fe-c4a63c927505" />



---

## 🛠️ Methodology: Data Engineering & Analysis Pipeline

### 1. Patient Selection and Cohort Definition

This study was a retrospective, multi-center cohort study (13 nationwide pediatric orthopedic hospital locations, 2011–2021). The final cohort was filtered based on strict clinical inclusion criteria (e.g., ASA Category 1/2, AIS diagnosis, specific hardware count) to ensure homogeneity.

#### Study Cohorts (Analgesia Type)
Patients were segmented into four distinct analgesia cohorts for comparison, with **Morphine-PCA** serving as the baseline comparator:

| Cohort | Description | Size (n) |
| :--- | :--- | :--- |
| **Morphine-PCA** | Patient Controlled Analgesia | 1160 |
| **Hydromorphone-PCA** | Patient Controlled Analgesia | 858 |
| **Continuous Epidural Analgesia (CEA)** | Bupivacaine $\pm$ low-dose opioid | 263 |
| **Spinal Block (Intrathecal) Opioid** | Intrathecal Opioid | 90 |

#### Timeline Standardization
All analysis was based on the **Post-Operative Hourly (POH)** timeline, with $\text{POH}=0$ defined uniformly as the **Patient-Out-Of-Surgery-Suite timestamp**. Data was aggregated into Post-Operative Day (POD) bins for clinical review (e.g., POD-0 = POH 0-15; subsequent days are 24 hours).

### 2. Data Standardization and Feature Creation

All raw data was extracted from a **Cerner/Oracle database** using **SAP Business Objects** SQL queries.

* **Extraction Query Example:** To demonstrate proficiency in EMR data acquisition, a sample of the complex SQL used to extract clinical event data (e.g., Peripheral IV details) for the defined cohort is available here: **[View Sample SQL Extraction Query](SQL-Sample_Query)**.

* ****Opioid Standardization (Oral Morphine Equivalents - OME):**** All opioid administrations (PCA, Epidural, Oral) were converted to a single metric: ****Oral Morphine Equivalents (OMEs)****, using the UCSF Pain Management rubric's IV scale for epidural opioids due to the opioid-naive cohort.

* **Multi-Modal Analgesia (MMA) Scaling:** Non-opioid analgesia doses (Acetaminophen, Ibuprofen) were standardized as a **%-Theo-Hourly-Max** to normalize dosage relative to published clinical safety and maximum limits.

### 3. Missing Data Imputation and Data Quality

A custom, clinically-informed approach was used to handle the significant missingness in high-frequency data.

* **Opioid Feature:** The primary feature for analysis was **Cumulative OME/kg**.
* **MMA Imputation:** MMA values were imputed over a **defined 6-hour drug run-off model** (dose/i for subsequent hours) to reflect expected pharmacokinetics.
* **VAPS Score Imputation:** Missing Verbal Analogue Pain Scores (VAPS) were imputed using a **custom decay-schedule-based Python function** that factors in both the **last reported score** and the **cumulative average**, ensuring clinically plausible estimations.

<img width="1363" height="600" alt="image" src="https://github.com/user-attachments/assets/69204220-81f4-4bd6-875b-9f3bb7894edb" />



### 4. Outcome Variable Definition (Complications and LOS)

This phase of the study focused on clinical outcomes using the previously cleaned and standardized cohort.

* **Length of Stay (LOS):** Calculated rigorously by subtracting the **Patient-Out-of-Room (POR) timestamp** from the **Discharge Timestamp**. This defined the final max POH value used in survival and regression models.
* **Complication Classification:** The following clinical events were categorized as complications for analysis; incidence determined (by POH) as either symptoms charted as Clinical Events, HIM diagnosis codes, or by contravening medication given with explicit orders. The heatmap summarizes statistical differences vs Morphine-PCA, faceted by Post-Operative-Day (POD).

    * **Muscle Spasm**
    * **NV:** **(Post-Operative Nausea and Vomiting)**
    * **POAE:** **(Possible Opioid Adverse Event** Respiratory depressive episode, clinically significant desaturation, >= moderate sedation, or rescue naloxone given with explicit orders)
    * **Pruritis:** **(Itching)** 
    * **Constipation** 

   <img width="1480" height="1019" alt="image" src="https://github.com/user-attachments/assets/7d76bee9-84fd-49a8-9234-a5f699556191" />
   

 **Showing relative incidence of muscule spasm vs post-operative-hour (POH), faceted by treatment type**
 
 <img width="1184" height="784" alt="image" src="https://github.com/user-attachments/assets/b6ef9e70-89f3-4d80-beaf-87065dc77822" />



---

## 📊 Results and Reproducibility

### Key Finding 1: Opioid Sparing Effect by Analgesia Cohort
The statistical difference between the two PCA modalities was of questionable significance. These were combined into a 'PCA Group' to simplify the model. The analysis validated a significant difference in post-operative opioid requirements across the three primary modalities. The cumulative OME/kg plot below demonstrates the superior opioid-sparing effect of both neuraxial modalities, particularly within the critical first 48 Post-Operative Hours (POH).

<img width="1384" height="584" alt="image" src="https://github.com/user-attachments/assets/5b30861b-f37c-4390-a815-9e1d602403db" />

### Key Finding 2: Gradual Decrease in Opioid Utilization Over Time
Year over year reliance on opioids decreased, most notably in the PCA group, while utilization of MMA and non-pharmacological interventions (NPI) served to improve aggregate pain scores over the same timeframe

<img width="1575" height="588" alt="image" src="https://github.com/user-attachments/assets/275e3f5b-2bec-48bc-878b-8afd0447be3a" />




### Publication Credit
This analysis contributed to findings published in the following peer-reviewed journal:

* [Primary Publication Link (PubMed/Semantic Scholar)](YOUR_PUBLICATION_URL_HERE)

### Reproducibility
Examples from our analytical pipeline are available in the accompanying notebooks.



* **[View Analysis Notebook (MMA_OBA_VAPS_Timeline.ipynb)](MMA_OBA_VAPS_Timeline.ipynb)**

* **[View Analysis Notebook (IM_complications_timeline_conda1.ipynd)](IM_complications_timeline_conda1.ipynb)**

  
* **[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/kirk-allen-ryan/Pediatric-Pain-Analgesia-Analysis/HEAD)** *(To run live code)*



