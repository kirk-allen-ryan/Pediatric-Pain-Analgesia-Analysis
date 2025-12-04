# Pediatric-Pain-Analgesia-Analysis
Analysis and visualizations related to SHC Protocol SPO2101R, Posterior Spinal Fusion (PSF) Analgesia Study

# 🔬 Pediatric Pain Analgesia Analysis: Opioid Sparing Strategies

## Project Focus: Continuous Epidural Analgesia for Scoliosis Surgery

This repository contains the data engineering pipeline and reproducible analysis related to the study: **"Continuous epidural analgesia for scoliosis correction surgery: does it reduce opioid requirements, complications and length of stay?"**

As the primary Data Engineer and Analyst, this project showcases expertise in rigorous EMR data acquisition, multi-step standardization, clinically-informed imputation, and statistical preparation for published research.

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

* **Opioid Standardization (Oral Morphine Equivalents - OME):** All opioid administrations (PCA, Epidural, Oral) were converted to a single metric: **Oral Morphine Equivalents (OMEs)**, using the UCSF Pain Management rubric's IV scale for epidural opioids due to the opioid-naive cohort.
* **Multi-Modal Analgesia (MMA) Scaling:** Non-opioid analgesia doses (Acetaminophen, Ibuprofen) were standardized as a **%-Theo-Hourly-Max** to normalize dosage relative to published clinical safety and maximum limits.

### 3. Missing Data Imputation and Data Quality

A custom, clinically-informed approach was used to handle the significant missingness in high-frequency data.

* **Opioid Feature:** The primary feature for analysis was **Cumulative OME/kg**.
* **MMA Imputation:** MMA values were imputed over a **defined 6-hour drug run-off model** (dose/i for subsequent hours) to reflect expected pharmacokinetics.
* **VAPS Score Imputation:** Missing Verbal Analogue Pain Scores (VAPS) were imputed using a **custom decay-schedule-based Python function** that factors in both the **last reported score** and the **cumulative average**, ensuring clinically plausible estimations. *The Python function is detailed in the accompanying notebook.*

---

## 📊 Results and Reproducibility

### Publication Credit
This analysis contributed to findings published in the following peer-reviewed journal:

* [Primary Publication Link (PubMed/Semantic Scholar)](YOUR_PUBLICATION_URL_HERE)

### Reproducibility
The full data cleaning and analytical pipeline is available in the accompanying notebook.

* **[View Cleaned Analysis Notebook (analysis_psf.ipynb)](ANALYSIS_NOTEBOOK_FILE_NAME.ipynb)**
* **[Launch Interactive Environment via Binder](YOUR_BINDER_LINK_HERE)** *(Recommended for recruiters to run the code live)*



