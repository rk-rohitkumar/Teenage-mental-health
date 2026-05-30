# Teenage Mental Health – Power BI Report

This repository contains a one-page Power BI report built on a Kaggle teen mental health dataset, exploring how lifestyle factors (social media, sleep, physical activity) relate to stress, anxiety, and depression risk.

## Dataset

- Source: Kaggle “Teen_Mental_Health_Dataset” (teen mental health, habits, and digital usage; download separately from Kaggle and place the CSV in the expected folder path used in Power Query).
- Core fields (raw):
    - Demographics and usage: `age`, `gender`, `dailysocialmediahours`, `platformusage`, `socialinteractionlevel`.
    - Lifestyle: `sleephours`, `screentimebeforesleep`, `physicalactivity`, `academicperformance`.
    - Mental health indicators: `stresslevel`, `anxietylevel`, `addictionlevel`, `depressionlabel`.


## Model and engineered fields

The semantic model is defined using TMDL (PBIP format) for Git-friendly Power BI development.

Key engineered columns (Power Query):

- `totaldistress`: composite distress score $=$ stress + anxiety + addiction.
- `TotalDistressRisk`: categorical bucket (Very Low, Low, Medium, Very High) based on `totaldistress` thresholds.
- Additional risk labels: `AnxietyRisk`, `AddictionRisk`, `StressRisk`, `AcademicStatus`, `SocialMediaRisk`, `SleepPrepRisk`, `SleepQuality`, `ActivityLevel`, `DepressionStatus` (readable categorical views over numeric inputs).

## Report highlights

The one-page report focuses on:

- Depression prevalence by gender and distress risk buckets.
- Distribution of teens across `TotalDistressRisk` and `ActivityLevel` categories.
- High-level composite metrics (Total Distress Score, sleep and lifestyle indices) to contextualize risk patterns.

All insights are descriptive and specific to this Kaggle dataset; they are not intended for clinical diagnosis or individual-level decision-making.

## How to run

1. Clone this repository.
2. Download the Teen Mental Health dataset from Kaggle and place the CSV at the path referenced in `Teen_Mental_Health_Dataset.tmdl` (Power Query section).
3. Open the `.pbip` file in Power BI Desktop.
4. Refresh the dataset to load the CSV and navigate to the report page.