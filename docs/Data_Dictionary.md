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

| Column Name | Data Type | Description / Expected Values | Recommended Aggregation / Use |
| :--- | :--- | :--- | :--- |
| **age** | Integer (Int64) | Age in years. Typical teenage range 10–19 (verify). May contain missing or out-of-range values — validate and clean. | Use as continuous measure (average, median) or bin into groups (e.g., 10–12,13–15,16–19). Do NOT sum. |
| **gender** | Text (String) | Self-reported gender label (e.g., 'Male', 'Female', 'Other', 'Prefer not to say'). | Use for segmentation (counts, percentages). Consider standardizing values. |
| **platform_usage** | Text (String) | Primary social media platform used (e.g., 'Instagram', 'TikTok', 'Facebook', 'None'). | Categorical analysis (mode, top N platforms, cross-tab with risk). Standardize spelling/casing. |
| **social_interaction_level** | Text (String) | Qualitative level of in-person social interaction (expected values: 'High', 'Medium', 'Low' or similar). | Use as categorical segment; consider ordering if levels are ordinal. |

### B. Key Metrics (Outcome Scores)
These are core measures intended to quantify mental health-related outcomes. Interpret with caution and validate scales with domain experts.

| Column Name | Data Type | Definition / Expected Range | Recommended Aggregation / Interpretation |
| :--- | :--- | :--- | :--- |
| **stress_level** | Integer (Int64) | Numeric stress score. Source logic uses thresholds (`>7` high, `>4` medium), implying a 0–10 or similar scale — confirm with source. | Use mean/median, distributions, and percent above threshold. Avoid summing across people. |
| **anxiety_level** | Integer (Int64) | Numeric anxiety score. PQ thresholds use `>7` (high), `>4` (medium). | Use distributions and threshold-based cohorts (`High`/`Medium`/`Low`). |
| **addiction_level** | Integer (Int64) | Numeric measure of addictive/problematic behavior. PQ thresholds: `>6` (high), `>3` (medium). | Use for cohort segmentation; validate exact scale. |
| **depression_label** | Integer (Int64) | Encoded depression indicator. Power Query treats `1` as 'Present' and other values as 'Absent' — this suggests binary or categorical encoding. | Treat as binary flag unless documentation indicates severity. Use counts/percentages. |
| **academic_performance** | Number (Double) | Performance score (examples in model use cutoffs `>=3.5` and `>=2.5` — suggests a 0–4 scale, e.g., GPA). Confirm scale. | Use average, distribution, and cohort buckets (`High Performing`, `At Risk`, `Needs Support`). |

### C. Behavioral & Lifestyle Factors (Independent Variables)
Metrics that capture daily behaviors and exposures which may correlate with outcome scores.

| Column Name | Data Type | Unit / Expected Values | Recommended Aggregation / Notes |
| :--- | :--- | :--- | :--- |
| **daily_social_media_hours** | Number (Double) | Hours per day spent on social media (expected 0–24). Outliers possible (validate). | Use mean/median, histogram, and threshold-based categories (`Low`/`Medium`/`High`). |
| **sleep_hours** | Number (Double) | Average sleep hours per night (0–24). | Use mean/median and sleep-quality derived categories. Watch for implausible values. |
| **screen_time_before_sleep** | Number (Double) | Hours of screen exposure before sleep (expected 0–24). PQ thresholds: `>2` (high risk), `>1` (medium). | Use to derive `SleepPrep_Risk`. Consider converting to minutes if needed. |
| **physical_activity** | Number (Double) | Encoded value representing activity. In the query logic it appears to be categorical-coded (2 = High, 1 = Medium, else Low). Confirm whether this is hours or category code. | If categorical-coded, treat as ordinal; map codes to labels. If numeric duration, use average. Validate source. |

***

## ⚠️ Key Assumptions and Limitations

Since this dataset was not provided with formal documentation, treat the following as assumptions to validate with subject matter experts:

1. **Score Interpretation:** Numeric *level* fields (stress, anxiety, addiction) are assumed monotonic: higher = worse. Confirm exact scales and directionality.
2. **Time Units:** Time metrics are assumed to be measured in hours.
3. **Coded Categories:** Some fields (e.g., `physical_activity`, `depression_label`) may be numeric codes representing categories — validate encoding and map to descriptive labels.
4. **Row Granularity:** Each row represents a single participant observation; dataset is not longitudinal by default.

***

## 💡 Recommended Next Steps

1. **Validate scales and encodings** with the dataset author or domain expert (especially `stress_level`, `anxiety_level`, `addiction_level`, `physical_activity`, `academic_performance`, and `depression_label`).
2. **Clean and standardize** categorical values (e.g., `gender`, `platform_usage`, `social_interaction_level`) to avoid mismatches during reporting.
3. **Create a `Measure_Definitions.md`** listing suggested DAX measures (averages, threshold counts, ratios) that reference these cleaned columns.

***

### D. Derived Insights (Calculated / Derived Columns)
These fields are computed in Power Query and provide interpretation-ready categories or scores derived from base measures. Keep the source PQ step as the source-of-truth for exact thresholds and logic.

| Column Name | Data Type | Calculation / Thresholds | Interpretation / Use |
| :--- | :--- | :--- | :--- |
| **total_distress** | Integer (Int64) | `stress_level + anxiety_level + addiction_level` | Composite severity score summarizing multiple dimensions of distress. Use for ranking or cohorting, and aggregate by mean/sum at group level. |
| **Total_Distress_Risk** | Text (String) | `Very High Risk` if `total_distress > 16`; `Medium Risk` if `>12`; `Low Risk` if `>9`; else `Unclassified`. | Bucketed risk label for `total_distress`. Use in filters and visual segments. |
| **Anxiety_Risk** | Text (String) | `High Risk` if `anxiety_level > 7`; `Medium Risk` if `>4`; else `Low Risk`. | Derived anxiety risk category. |
| **Addiction_Risk** | Text (String) | `High Risk` if `addiction_level > 6`; `Medium Risk` if `>3`; else `Low Risk`. | Derived addiction risk category. |
| **Stress_Risk** | Text (String) | `High Risk` if `stress_level > 7`; `Medium Risk` if `>4`; else `Low Risk`. | Derived stress risk category. |
| **Academic_Status** | Text (String) | `High Performing` if `academic_performance >= 3.5`; `At Risk/Medium Performance` if `>= 2.5`; else `Needs Support`. | Helpful for education outcome segmentation. Confirm score scale (likely 0–4). |
| **SocialMedia_Risk** | Text (String) | `High Risk (Excessive Usage)` if `daily_social_media_hours > 5`; `Medium Risk` if `>2`; else `Low Risk`. | Behavioral-category for social media exposure. |
| **SleepPrep_Risk** | Text (String) | `High Risk` if `screen_time_before_sleep > 2`; `Medium Risk` if `>1`; else `Low Risk`. | Indicates potential sleep-disruptive behavior. |
| **Sleep_Quality** | Text (String) | `Optimal` if `sleep_hours > 8`; `Acceptable` if `>= 6`; else `Poor`. | Simple sleep quality categorization. |
| **Activity_Level** | Text (String) | If `physical_activity = 2` → `High Activity`; `= 1` → `Medium Activity`; else `Low Activity`. | Maps coded activity levels to human labels. Validate whether `physical_activity` is encoded or numeric duration. |
| **Depression_Status** | Text (String) | If `depression_label = 1` → `Present`; else `Absent`. | Binary indicator derived from the depression measure; **not** a clinical diagnosis. |

**Model note:** These derived fields are organized in the semantic model under the `Derived Insights` display folder for reporting convenience. Keep Power Query steps as the canonical source for exact logic and thresholds.