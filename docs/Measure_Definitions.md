# Derived Measures Dictionary

This document serves as the definitive reference for all calculated measures within the semantic model (`_calculations.tmdl`). These metrics combine multiple source columns to derive a single, actionable business understanding.

## 📏 Total Distress Score (TDS)

**Definition:** The TDS provides a comprehensive, composite score representing the cumulative psychological burden of a teenager based on key clinical indicators.

**Calculation:** It is calculated by summing the measured levels of stress, anxiety, and depression. 

**Formula Logic:** `SUM('stress_level') + SUM('anxiety_level') + SUM('depression_label')`

**Interpretation Guide:**
*   **What it measures:** Overall severity of mental distress.
*   **Scale Assumption:** Based on the source data, a **higher numerical value indicates a greater overall level of distress** and potential need for intervention. 
*   **Limitations:** This score treats all three clinical indicators (stress, anxiety, depression) as having equal weight and linear correlation, which may not reflect complex real-world dynamics.

---

## 💤 Sleep Efficiency Index (SEI)

**Definition:** SEI is a metric used to quantify the quality of sleep relative to pre-sleep digital exposure. It assesses how much restorative time was achieved versus the disruptive potential of screen use.

**Calculation:** `SUM('sleep_hours') / (1 + SUM('screen_time_before_sleep'))`

**Formula Logic:** The ratio compares total sleep hours to a normalized measure of pre-sleep screen time.

**Interpretation Guide:**
*   **What it measures:** Sleep hygiene and quality.
*   **Scale Assumption:** The goal is generally for the **score to be as high as possible.** A lower score suggests that screen use immediately before sleep significantly compromises restorative rest, indicating poor sleep efficiency.
*   **Note:** The formula includes a `+ 1` in the denominator to mathematically prevent division-by-zero errors, ensuring model stability.

---

## 💪 Lifestyle Balance Score (LBS)

**Definition:** LBS provides an aggregate measure of key positive lifestyle behaviors—specifically physical activity and adequate sleep. It serves as a potential predictor of overall well-being.

**Calculation:** `SUM('physical_activity') + SUM('sleep_hours')`

**Formula Logic:** Simple additive score combining two major contributors to general health.

**Interpretation Guide:**
*   **What it measures:** The combined level of proactive care for the body and rest. 
*   **Scale Assumption:** A **higher numerical value indicates a stronger, more balanced lifestyle**, suggesting better foundational support for mental health.