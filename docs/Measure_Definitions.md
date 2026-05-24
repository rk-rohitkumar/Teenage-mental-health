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

---

## 🔥 Activity vs. Screen Ratio (ASR)

**Definition:** ASR quantifies the relative strength of an individual's physical activity compared to their exposure to pre-sleep digital distraction. It assesses behavioral resilience—the ability of healthy habits to counterbalance poor sleep hygiene.

**Calculation:** `SUM('physical_activity') / (1 + SUM('screen_time_before_sleep'))`

**Formula Logic:** The ratio divides total accumulated physical activity by a normalized measure of screen time before bed.

**Interpretation Guide:**
*   **What it measures:** Behavioral balance and resilience.
*   **Scale Assumption:** A **higher score is generally desirable**, suggesting that high physical activity levels effectively mitigate the negative impact suggested by pre-sleep screen exposure. The `+ 1` in the denominator ensures model stability.

---

## 📉 Distress vs. Lifestyle Balance Ratio (TDS/LBS)

**Definition:** This ratio normalizes the overall distress score by factoring in general lifestyle balance, providing a holistic view of strain relative to proactive self-care efforts.

**Calculation:** `[Total Distress Score] / (1 + [Lifestyle Balance Score])`

**Formula Logic:** Divides the combined psychological burden (TDS) by a normalized measure of positive lifestyle habits (LBS).

**Interpretation Guide:**
*   **What it measures:** The severity of distress relative to foundational coping mechanisms.
*   **Scale Assumption:** A **higher ratio indicates that an individual's baseline habits are insufficient to offset their measured psychological burden**, suggesting a greater need for intervention.

---

## 🏃 Strain-to-Activity Ratio (SAR)

**Definition:** SAR quantifies the intensity of emotional strain relative to physical activity level. It measures how much psychological distress remains even after accounting for healthy behavioral efforts.

**Calculation:** `SUM('stress_level') / (1 + SUM('physical_activity'))`

**Formula Logic:** Divides the total stress score by a normalized measure of physical activity.

**Interpretation Guide:**
*   **What it measures:** The proportion of emotional strain that cannot be mitigated by physical exercise alone.
*   **Scale Assumption:** A **higher score is concerning**, suggesting that an individual's psychological distress is disproportionately high compared to their healthy behavioral efforts, indicating a potential need for mental health support beyond routine activity.

---

## 🚨 Compounded Risk Score (CRS)

**Definition:** CRS quantifies the overall compounded risk by multiplying key psychological distress indicators (Stress and Anxiety) by the degree of poor digital hygiene (pre-sleep screen time).

**Calculation:** `SUM('stress_level') + SUM('anxiety_level') * SUM('screen_time_before_sleep')`

**Formula Logic:** Adds raw stress level to an anxiety/screen time product. This calculation emphasizes individuals who suffer from both emotional burden and poor sleep hygiene habits.

**Interpretation Guide:**
*   **What it measures:** The confluence of psychological distress and high digital disruption risk.
*   **Scale Assumption:** A **higher score indicates a heightened, compounded risk**, signaling the highest priority for targeted intervention or follow-up care.
