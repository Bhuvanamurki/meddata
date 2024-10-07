# Healthcare Data SQL Analysis


This project provides an in-depth analysis of healthcare data using SQL queries. Due to the sensitive nature of the dataset, which includes confidential patient information such as demographics, medical conditions, billing, hospitalizations, and insurance providers, public access to the data is restricted.

## Table of Contents
1. Introduction
2. Dataset Overview
3. SQL Queries and Analysis
   - Data Overview & Basic Statistics
   - Medical Conditions & Medications
   - Insurance Providers & Hospitals
   - Financial Analysis & Duration of Hospitalization
   - Blood Type Analysis & Donation Matching
   - Yearly Admissions & Insurance Analysis
   - Patient Risk Categorization
4. Setup
5. Contributing
6. License

---

## Introduction

This project uses SQL queries to explore a healthcare dataset, offering insights into patient demographics, medical conditions, financial data, and hospital performance. These SQL queries can be applied to a database to extract key insights that inform healthcare management, decision-making, and resource allocation.

---

## Dataset Overview

The dataset contains the following columns:
- **Name**: Patient's name.
- **Age**: Patient's age at admission.
- **Gender**: Patient's gender.
- **Blood Type**: Patient's blood type (e.g., A+, O-).
- **Medical Condition**: Primary medical condition or diagnosis.
- **Date of Admission**: Date of the patient's admission.
- **Doctor**: Name of the doctor responsible.
- **Hospital**: Name of the hospital where the patient was admitted.
- **Insurance Provider**: Patient's insurance provider.
- **Billing Amount**: Total billed amount for the patient's care.
- **Room Number**: Room number where the patient stayed.
- **Admission Type**: Emergency, Elective, or Urgent.
- **Discharge Date**: Date of the patient's discharge.
- **Medication**: Medications administered.
- **Test Results**: Results of medical tests (Normal, Abnormal, Inconclusive).

---

## SQL Queries and Analysis

### 1. Overview of Patient Data & Key Statistics

These queries provide a high-level view of the dataset, including record counts and patient demographics.

```sql
-- Total number of records in the dataset
SELECT COUNT(*) AS total_patients FROM healthcare_data;

-- Maximum, minimum, and average age of patients
SELECT MAX(Age) AS max_age, MIN(Age) AS min_age, AVG(Age) AS avg_age FROM healthcare_data;

-- Patient count by gender
SELECT Gender, COUNT(*) AS total_by_gender FROM healthcare_data GROUP BY Gender;

-- Patient count by blood type
SELECT Blood_Type, COUNT(*) AS total_by_blood_type FROM healthcare_data GROUP BY Blood_Type;
```

---

### 2. Analysis of Medical Conditions & Prescribed Medications

These queries explore the most frequent medical conditions and medications prescribed to patients.

```sql
-- Count of patients with each medical condition
SELECT Medical_Condition, COUNT(*) AS condition_count 
FROM healthcare_data 
GROUP BY Medical_Condition 
ORDER BY condition_count DESC;

-- Count of prescriptions for each medication
SELECT Medication, COUNT(*) AS medication_count 
FROM healthcare_data 
GROUP BY Medication 
ORDER BY medication_count DESC;

-- Medication prescribed for each medical condition
SELECT Medical_Condition, Medication, COUNT(*) AS medication_for_condition 
FROM healthcare_data 
GROUP BY Medical_Condition, Medication;
```

---

### 3. Insurance Providers & Hospital Preferences

This section analyzes patient preferences for insurance providers and hospitals.

```sql
-- Number of patients by insurance provider
SELECT Insurance_Provider, COUNT(*) AS provider_count 
FROM healthcare_data 
GROUP BY Insurance_Provider;

-- Number of admissions by hospital
SELECT Hospital, COUNT(*) AS admissions_count 
FROM healthcare_data 
GROUP BY Hospital 
ORDER BY admissions_count DESC;

-- Most frequent insurance providers by hospital
SELECT Hospital, Insurance_Provider, COUNT(*) AS provider_count 
FROM healthcare_data 
GROUP BY Hospital, Insurance_Provider 
ORDER BY provider_count DESC;
```

---

### 4. Financial Insights & Hospitalization Duration Analysis

Here, we explore the financial aspect and calculate the length of hospital stays.

```sql
-- Average and total billing amount by medical condition
SELECT Medical_Condition, AVG(Billing_Amount) AS avg_billing, SUM(Billing_Amount) AS total_billing 
FROM healthcare_data 
GROUP BY Medical_Condition;

-- Billing amounts by admission type
SELECT Admission_Type, AVG(Billing_Amount) AS avg_billing, SUM(Billing_Amount) AS total_billing 
FROM healthcare_data 
GROUP BY Admission_Type;

-- Calculate hospital stay duration for each patient
SELECT Name, DATEDIFF(Discharge_Date, Date_of_Admission) AS duration_of_stay 
FROM healthcare_data;
```

---

### 5. Blood Type Distribution & Donor Matching

This query explores the distribution of blood types and matches donors with recipients.

```sql
-- Count patients by blood type
SELECT Blood_Type, COUNT(*) AS count_by_blood_type 
FROM healthcare_data 
GROUP BY Blood_Type;

-- Create stored procedure to match blood donors and recipients
CREATE PROCEDURE Blood_Matcher (IN donor_blood_type VARCHAR(3), IN recipient_blood_type VARCHAR(3))
BEGIN
  SELECT Name, Age, Blood_Type, Hospital 
  FROM healthcare_data 
  WHERE Blood_Type = donor_blood_type 
  AND Blood_Type IN (recipient_blood_type);
END;
```

---

### 6. Annual Admission Trends & Insurance Billing Patterns

We analyze hospital admissions over specific years and examine insurance billing patterns.

```sql
-- Hospital admissions in 2024 and 2025
SELECT YEAR(Date_of_Admission) AS year_admitted, Hospital, COUNT(*) AS admissions_count 
FROM healthcare_data 
WHERE YEAR(Date_of_Admission) IN (2024, 2025) 
GROUP BY year_admitted, Hospital;

-- Total billing amounts by insurance provider
SELECT Insurance_Provider, SUM(Billing_Amount) AS total_billing 
FROM healthcare_data 
GROUP BY Insurance_Provider;
```

---

### 7. Categorization of Patient Risk Levels

Categorize patients into risk groups based on their medical conditions and test results.

```sql
-- Categorize patients into risk groups
SELECT Name, Medical_Condition, 
  CASE 
    WHEN Medical_Condition IN ('Heart Disease', 'Diabetes') OR Test_Results = 'Abnormal' THEN 'High Risk' 
    WHEN Medical_Condition = 'Hypertension' THEN 'Medium Risk' 
    ELSE 'Low Risk' 
  END AS risk_category 
FROM healthcare_data;
```

---
