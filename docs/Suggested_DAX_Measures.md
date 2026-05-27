# 🧠 Semantic Model and DAX Measures Strategy

This document outlines the strategic approach for building the semantic model, focusing on creating robust measures that yield interpretable results suitable for business reporting. We emphasize **Averages**, **Ratios**, and **Composite Scores** over simple sums to ensure accurate comparisons between demographic groups (e.g., comparing average risk between genders or platforms).

***

## 📈 Foundational Analytical Principles

1.  **Average is King:** For comparative analysis (e.g., "How stressed are females compared to males?"), we must use `AVERAGE()` rather than `SUM()`.
2.  **Normalization via Ratios:** Simple scores are often meaningless on their own. Combining two variables into a ratio (e.g., Activity / Screen Time) creates a standardized, actionable metric for analysis.
3.  **Composite Scoring:** Grouping multiple related metrics into one total score provides an immediate 'Risk Profile' or 'Well-being Index,' simplifying complex models.

***

## 🛠 Core DAX Measures (Using Table: `Teen_Mental_Health_Dataset`)

The following measures are suggested to form the backbone of the semantic model, providing calculated insights rather than just raw data columns.

### I. Outcome & Distress Measures (Focus on Averages)

These track the typical severity of symptoms for a given group.
| Measure Name | DA Formula (Conceptual) | Description |
| :--- | :--- | :--- |
| **Avg Total Distress Score** | `AVERAGE([Stress Level] + [Anxiety Level] + [Depression Label])` | Calculates the average combined severity score for a single observation. This is the primary metric for tracking general risk. |
| **Composite Mental Health Index** | `1 - (DIVIDE(AVERAGE([Stress Level]), 5) + DIVIDE(AVERAGE([Anxiety Level]), 5))` | *Conceptual:* Normalizes all core scores into an index where a higher number indicates better mental health (e.g., scaled from 0 to 1). |
| **Total Addiction Risk** | `AVERAGE('Teen_Mental_Health_Dataset'[addiction_level])` | The average level of behavioral or substance addiction risk within the selected context. |

### II. Behavioral & Lifestyle Measures (Focus on Ratios)

These measures standardize behavior by comparing inputs to each other, making them highly actionable for interventions.

| Measure Name | DA Formula (Conceptual) | Description |
| :--- | :--- | :--- |
| **Sleep Quality Ratio** | `DIVIDE('Teen_Mental_Health_Dataset'[sleep_hours], 1 + 'Teen_Mental_Health_Dataset'[screen_time_before_sleep])` | Measures how much sleep time is preserved relative to screen disruption. A high ratio indicates less disruption. |
| **Activity/Socialization Balance** | `DIVIDE('Teen_Mental_Health_Dataset'[physical_activity], 1 + 'Teen_Mental_Health_Dataset'[social_interaction_level])` | Standardizes physical activity by the level of in-person social interaction. Helps understand if *some* external factor is mediating health gains. |

### III. Key Group Aggregates (Focus on Segmentation)

| Measure Name | DA Formula (Conceptual) | Description |
| :--- | :--- | :--- |
| **High Risk Count** | `CALCULATE(COUNT('Teen_Mental_Health_Dataset'[age]), 'Teen_Mental_Health_Dataset'[Stress Level] >= 4)` | Counts the number of observations within a group that meet a defined high-risk threshold (e.g., Stress Level $\ge 4$). |
| **Avg Academic Performance Gap** | `AVERAGE('Teen_Mental_Health_Dataset'[academic_performance])` | The average academic score, used as a baseline for calculating the 'gap' relative to desired outcomes. |

***

## 💡 Visual Use Guidance

When creating visualizations:

*   **Comparison:** Use **Averages** (e.g., Average Stress Level) on column/line charts comparing groups (gender, platform).
*   **Trend Analysis:** Use Measures like `Avg Total Distress Score` plotted against a time dimension (if available).
*   **Severity Impact:** Use measures like `High Risk Count` combined with percentages to show the *proportion* of individuals falling into crisis categories.

