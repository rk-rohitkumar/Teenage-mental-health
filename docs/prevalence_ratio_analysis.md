# Comparative Prevalence Rate Analysis Methodology

This document outlines the conceptual and technical approach for calculating comparative rates (Prevalence Ratio) within the data model, based on analyzing the measures `[pct of depressed]` and `[pct of depressed m/f ratio]`. This pattern can be generalized to create actionable insights for any binary outcome variable or dimension comparison.

## 1. Core Concept: Prevalence Rate Calculation (The Numerator Component)

The foundational step is calculating a **Prevalence Rate**—the percentage of individuals within a defined group who exhibit a specific condition or characteristic.

### The Measure Pattern: `[pct of <Condition>]`
This measure calculates the ratio of 'Count of Instances with Condition' to 'Total Count of Observations'.

*   **Purpose:** To determine the baseline likelihood (prevalence) of an event occurring within the dataset, allowing for apples-to-apples comparisons across different time periods or groups.
*   **Logic Breakdown:**
    1.  **Numerator Calculation (Count Affected):** Use `CALCULATE(COUNTROWS(Table), Table[Condition_Column] = 1)` to count records where the condition is present (assuming a binary/flag column).
    2.  **Denominator Calculation (Total Population):** Use `CALCULATE(COUNTROWS(Table), REMOVEFILTERS(Table[Condition_Column]))` or simply rely on implicit context if appropriate.
    3.  **Final Ratio:** `DIVIDE(<Count Affected>, <Total Count>)`.

> **💡 Generalization Tip:** To calculate the prevalence rate for a different condition (e.g., anxiety), replace `depression_label = 1` with `anxiety_level > X`, or adjust the filtering logic based on how that dimension is encoded in the fact table (`Teen_Mental_Health_Dataset`).

## 2. Advanced Concept: Comparative Prevalence Ratio (The Disparity Metric)

Once the basic prevalence rate is established, a significant analytical step is calculating a **Comparative Ratio** to highlight disparities between two distinct dimensions or groups (e.g., Male vs. Female, High Activity vs. Low Activity). This ratio quantifies how much more likely one group is to experience the condition compared to another.

### The Measure Pattern: `[pct of <Condition>] m/f ratio`
This measure compares the prevalence rate calculated in Step 1 across two specified categories (Group A and Group B) and outputs a formatted, descriptive difference.

*   **Purpose:** To move beyond simple comparison and quantify the *magnitude* of difference or disparity between groups for a specific outcome.
*   **Logic Breakdown:**
    1.  **Calculate Individual Rates:** Create two versions of the base prevalence measure, filtered by the comparison dimensions: `Rate_A = [Prevalence Measure] filtered by Group A`, and `Rate_B = [Prevalence Measure] filtered by Group B`.
    2.  **Determine Dominance:** Identify which rate is higher (`IF(Rate_A > Rate_B, ...)`). This dictates the structure of the ratio calculation to ensure the output always represents a positive "more chance" metric.
    3.  **Calculate Ratio/Difference:** The relative difference is calculated as `DIVIDE(<Higher Rate>, <Lower Rate>) - 1`. This value represents the percentage increase or decrease from the baseline (lower rate).
    4.  **Formatting & Interpretation:** Wrap the calculation in complex logic to return a highly readable, descriptive string that clearly communicates *who* has *how much more* risk/prevalence.

> **💡 Generalization Tip:** To compare two different dimensions (e.g., comparing 'High Social Media Users' vs. 'Low Physical Activity'), you must adjust Step 1 and Step 2 to filter by the relevant categorical columns (`daily_social_media_hours` groups, `Activity_Level`, etc.) instead of or in addition to the `gender` column.

## Summary Table for Implementation

| Analysis Goal | Measure Name Example | Underlying Calculation Concept | Generalizable Logic Flow |
| :--- | :--- | :--- | :--- |
| **Single Prevalence** | `% of depressed` | Binary Flag Rate (Count/Total) | `DIVIDE(CALCULATE(COUNTROWS(...), [Flag]=1), CALCULATE(COUNTROWS(...), ALL()))` |
| **Comparative Ratio** | `m/f ratio` | Relative Difference Quantification | 1. Calculate Group A Prevalence. <br> 2. Calculate Group B Prevalence. <br> 3. Determine $\text{Max}(\text{P}_A, \text{P}_B) / \text{Min}(\text{P}_A, \text{P}_B) - 1$. |

***
*This methodology provides a robust framework for quantifying risk disparities and should be applied consistently across all key outcome variables (e.g., Anxiety, Addiction).*