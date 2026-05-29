                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    # Suggested Columns for Mental Health Analysis

This document proposes several calculated columns to enhance the analysis of the `Teen_Mental_Health_Dataset`. These metrics move beyond simple additive scores by creating composite indices and behavioral risk measurements, allowing for a more nuanced understanding of psychological distress. The suggested calculations use predefined thresholds derived from data profiling and clinical best practices.

## 1. Comprehensive Behavioral & Symptom Risk Categorization
These columns convert continuous or ordinal scores into discrete risk labels (Low, Medium, High) for easier visualization and filtering.

### A. Overall Distress Index (`Total_Distress_Risk`)
**Purpose:** Classifies the overall severity of distress based on the combined score (`total_distress`).
**Thresholds:**
*   `> 16`: Very High Risk
*   `9 < Total <= 16`: Medium Risk
*   `<= 9`: Low Risk

### B. Anxiety Level (`Anxiety_Risk`)
**Purpose:** Assesses the level of reported anxiety severity.
**Thresholds (Based on 1-10 scale):**
*   `> 7`: High Risk
*   `4 < Score <= 7`: Medium Risk
*   `<= 4`: Low Risk

### C. Addiction Level (`Addiction_Risk`)
**Purpose:** Assesses the level of reported addiction severity.
**Thresholds (Based on 1-10 scale):**
*   `> 6`: High Risk
*   `3 < Score <= 6`: Medium Risk
*   `<= 3`: Low Risk

### D. Stress Level (`Stress_Risk`)
**Purpose:** Assesses the level of reported stress severity.
**Thresholds (Based on 1-10 scale):**
*   `> 7`: High Risk
*   `4 < Score <= 7`: Medium Risk
*   `<= 4`: Low Risk

### E. Academic Performance (`Academic_Status`)
**Purpose:** Categorizes academic performance to identify students needing support.
**Thresholds (Based on 2-4 scale):**
*   `>= 3.5`: High Performing
*   `2.5 <= Score < 3.5`: At Risk/Medium Performance
*   `< 2.5`: Needs Support

## 2. Behavioral and Lifestyle Indices
These metrics quantify habits and lifestyle factors associated with mental health risk.

### A. Daily Social Media Hours (`SocialMedia_Risk`)
**Purpose:** Quantifies the potential negative impact of social media usage.
**Thresholds (Based on hours):**
*   `> 5`: High Risk (Excessive Usage)
*   `2 < Hours <= 5`: Medium Risk (Moderate/Cautionary Usage)
*   `< 2`: Low Risk (Healthy Usage)

### B. Screen Time Before Sleep (`SleepPrep_Risk`)
**Purpose:** Measures the risk associated with blue light exposure near bedtime.
**Thresholds (Based on hours):**
*   `> 2`: High Risk (Significant Disruption)
*   `1 < Hours <= 2`: Medium Risk (Caution Advised)
*   `< 1`: Low Risk

### C. Sleep Hours (`Sleep_Quality`)
**Purpose:** Assesses sleep duration relative to recommended guidelines (7-9 hours).
**Thresholds (Based on hours):**
*   `> 8`: Optimal
*   `6 <= Hours <= 8`: Acceptable
*   `< 6`: Poor

### D. Physical Activity (`Activity_Level`)
**Purpose:** Categorizes physical activity frequency/intensity.
**Thresholds (Ordinal Scale: 0-2):**
*   `= 2`: High Activity
*   `= 1`: Medium Activity
*   `< 1`: Low Activity

### E. Depression Status (`Depression_Status`)
**Purpose:** Provides a simple binary status flag for diagnosis presence.
**Thresholds (Binary: 0 or 1):**
*   `= 1`: Present
*   `= 0`: Absent
## Implementation Details (M Code Changes)
The following M code steps must be appended to the existing Power Query logic within `Teen_Mental_Health_Dataset.tmdl`. This preserves all current calculations and adds the new derived features, ensuring data integrity by chaining the updates sequentially.


