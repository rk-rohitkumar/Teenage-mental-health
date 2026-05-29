# Data Dictionary and Business Glossary for Teenage Mental Health Dataset

## 🚀 Project Overview
This dataset is derived from a Kaggle source, collecting various demographic, lifestyle, and mental health metrics of teenagers. It is designed to support exploratory analysis into the factors contributing to mental well-being in this age group.

**Goal:** To understand relationships between behavioral variables (sleep, screen time, physical activity) and clinical indicators (stress, anxiety, depression).
**Source:** CSV file (source url: [teen-wellbeing](https://www.kaggle.com/code/sadaffarhan/decoding-teen-wellbeing-a-data-driven-analysis)) loaded via Power Query.

***

## 📋 Column Definitions (Data Dictionary)

The dataset is structured with the following columns:

### A. Dimensions (Identifiers & Categoricals)
These fields define *who* or *where* the data point relates to and should generally not be aggregated numerically.

| Column Name | Data Type | Description/Definition | Notes |
| :--- | :--- | :--- | :--- |
| **age** | Integer (Int64) | The age of the teenager in years at the time of observation. | Should be treated as a continuous variable, but grouping/binning may improve visualization. |
| **gender** | Text (String) | The self-identified gender of the participant. | Useful for segmenting analysis by demographic group. |
| **platform_usage** | Text (String) | Indicates which social media platform was most frequently used. | Categorical variable. Should be analyzed via counts/percentages per platform. |
| **social_interaction_level** | Text (String) | A qualitative measure of the frequency or quality of in-person social interaction. | Assumed to be a predefined level (e.g., "High," "Medium," "Low"). |

### B. Key Metrics (Outcome Scores)
These are core measures intended to quantify mental health status. **CAUTION: The interpretation of scores is critical.**

| Column Name | Data Type | Definition / Scope | Assumed Scale/Interpretation |
| :--- | :--- | :--- | :--- |
| **stress_level** | Integer (Int64) | Measured psychological stress level. | **Assumption:** Higher score = higher stress. Scale likely 0 to X (e.g., 1-5). |
| **anxiety_level** | Integer (Int64) | Measured anxiety intensity or severity. | **Assumption:** Higher score = higher anxiety. |
| **addiction_level** | Integer (Int64) | Level of problematic substance use, gaming, or behavioral addiction indicators. | **Assumption:** Higher score = greater addiction risk/severity. |
| **depression_label** | Integer (Int64) | Measured indicator or severity level of depressive symptoms. | **Assumption:** Higher score = greater depressive symptom presence/severity. |
| **academic_performance**| Number (Double) | A numerical score reflecting overall academic achievement or performance. | **Assumption:** Higher score = better academic performance. |

### C. Behavioral & Lifestyle Factors (Independent Variables)
These metrics quantify daily behaviors and habits that may correlate with the outcome scores.

| Column Name | Data Type | Unit / Definition | Summary Measure Used | Analysis Focus |
| :--- | :--- | :--- | :--- | :--- |
| **daily_social_media_hours** | Number (Double) | Average hours spent daily on social media platforms. | Sum | Relationship with mental health scores (e.g., positive correlation with stress). |
| **sleep_hours** | Number (Double) | Average number of sleep hours per night. | Sum | Key indicator for overall health. Low values are a primary concern. |
| **screen_time_before_sleep** | Number (Double) | Estimated time spent looking at screens immediately before sleeping. | Sum | High values suggest potential disruption to natural sleep cycles. |
| **physical_activity** | Number (Double) | Measured level or duration of physical activity (e.g., hours/day). | Sum | Higher values are generally correlated with better mental health outcomes. |

***

## ⚠️ Key Assumptions and Limitations

Since this dataset was not provided with documentation, the following assumptions must be treated as hypotheses until validated by subject matter experts:

1.  **Score Interpretation:** All *level* columns (`stress_level`, `anxiety_level`, etc.) assume a monotonic relationship where **a higher numerical value indicates a worse or more severe state.**
2.  **Time Units:** Time-based metrics (hours/sleep/screen time) are assumed to be measured consistently in **hours**.
3.  **Data Point Granularity:** Each row represents a single, discrete observation (e.g., one person's data collected over a specific period).
4.  **Model Scope:** This dataset is limited and cannot be used for any analysis requiring longitudinal data tracking across multiple years or different contexts.

***

## 💡 Recommended Next Steps

1.  **Validation:** **Before proceeding with DAX, these assumptions must be validated by the source domain expert.**
2.  **Modeling Focus:** The primary analytical focus should be creating measures that calculate ratios and comparisons (e.g., `Sleep Efficiency = sleep_hours / daily_social_media_hours`) and calculating total severity scores (e.g., `Total Distress Score = stress_level + anxiety_level + depression_label`).
3.  **Data Modeling:** Once validated, we will begin building the semantic model structure around these measures.

***

### D. Derived Insights (Calculated / Derived Columns)
These fields are computed in Power Query and provide interpretation-ready categories or scores derived from base measures.

| Column Name | Data Type | Definition / Calculation | Notes |
| :--- | :--- | :--- | :--- |
| **total_distress** | Integer (Int64) | Sum of `stress_level`, `anxiety_level`, and `addiction_level` (`stress_level + anxiety_level + addiction_level`). | Aggregation: Sum. Used as a composite distress score. |
| **Total_Distress_Risk** | Text (String) | Categorical risk label derived from `total_distress` (e.g., 'Very High Risk', 'Medium Risk', 'Low Risk', 'Unclassified'). | Useful for segmenting by overall distress severity. |
| **Anxiety_Risk** | Text (String) | Risk label derived from `anxiety_level` thresholds ('High Risk', 'Medium Risk', 'Low Risk'). | Classification thresholds defined in Power Query. |
| **Addiction_Risk** | Text (String) | Risk label derived from `addiction_level` thresholds. | Classification thresholds defined in Power Query. |
| **Stress_Risk** | Text (String) | Risk label derived from `stress_level` thresholds. | Classification thresholds defined in Power Query. |
| **Academic_Status** | Text (String) | Categorizes `academic_performance` into `High Performing`, `At Risk/Medium Performance`, or `Needs Support`. | Helpful for quick cohort segmentation. |
| **SocialMedia_Risk** | Text (String) | Categories based on `daily_social_media_hours` (e.g., 'High Risk (Excessive Usage)', 'Medium Risk', 'Low Risk'). | Behavioral-derived category for social media impact. |
| **SleepPrep_Risk** | Text (String) | Categories based on `screen_time_before_sleep` thresholds. | Helps identify sleep-disruptive behaviors. |
| **Sleep_Quality** | Text (String) | Categorizes `sleep_hours` into `Optimal`, `Acceptable`, `Poor`. | Derived health quality indicator. |
| **Activity_Level** | Text (String) | Categorizes `physical_activity` into `High Activity`, `Medium Activity`, `Low Activity`. | Derived active/ sedentary classification. |
| **Depression_Status** | Text (String) | Binary label derived from `depression_label` (e.g., 'Present' if 1 else 'Absent'). | Use with caution; not a clinical diagnosis. |

**Model note:** These derived fields are organized in the semantic model under the `Derived Insights` display folder for reporting convenience.