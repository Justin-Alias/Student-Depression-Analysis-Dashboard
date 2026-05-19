# 🧠 Student Depression Analysis

A full end-to-end data analytics project examining mental health indicators among university students — from raw data cleaning in SQL Server to interactive visual analysis in Tableau.

---

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Tools & Technologies](#tools--technologies)
- [Project Structure](#project-structure)
- [Dataset](#dataset)
- [Data Pipeline](#data-pipeline)
  - [Step 1 — Data Cleaning (SQL Server)](#step-1--data-cleaning-sql-server)
  - [Step 2 — Visualization (Tableau)](#step-2--visualization-tableau)
- [Dashboard Breakdown](#dashboard-breakdown)
- [Key Questions Explored](#key-questions-explored)
- [How to Run This Project](#how-to-run-this-project)

---

## Project Overview

This project investigates the relationship between academic lifestyle factors and depression outcomes in university students. Using a dataset of 502 student records, the data was first cleaned and transformed using SQL Server, then connected live to Tableau for visual analysis.

The goal was to identify which stressors — academic pressure, financial stress, sleep habits, study hours, and study satisfaction — have the strongest correlation with depression rates.

---

## Tools & Technologies

| Tool | Purpose |
|---|---|
| **Microsoft SQL Server** | Data storage, cleaning, and transformation |
| **SQL (T-SQL)** | Data wrangling — standardizing, categorizing, encoding |
| **Tableau Desktop** | Live SQL connection and dashboard building |
| **CSV** | Raw source data format |

---

## Project Structure

```
Student Depression Analysis/
│
├── Data 
      ├── Depression+Student+Dataset.csv   # Raw source data (502 students, 11 columns)
├── SQL
      ├── SQLQuery1.sql                    # SQL Server cleaning & transformation script
└── Dashboard
      ├── Tableau Dashboard 3.twb          # Tableau workbook — live SQL Server connection
```

---

## Dataset

**Source:** `Depression+Student+Dataset.csv`
**Records:** 502 students
**Format:** CSV → imported into SQL Server

| Column | Type | Description |
|---|---|---|
| `Gender` | string | Student gender (raw: Male/Female → cleaned: M/F) |
| `Age` | integer | Student age in years |
| `Academic Pressure` | float | Self-reported academic pressure score (1–5) |
| `Study Satisfaction` | float | Self-reported study satisfaction score (1–5) |
| `Sleep Duration` | string | Hours of sleep per night (e.g. "5-6 hours") |
| `Dietary Habits` | string | Dietary quality (Healthy / Moderate / Unhealthy) |
| `Have you ever had suicidal thoughts?` | string | Yes / No |
| `Study Hours` | integer | Daily study hours |
| `Financial Stress` | integer | Self-reported financial stress level (1–5) |
| `Family History of Mental Illness` | string | Yes / No |
| `Depression` | string | Target variable — raw: 0/1 → cleaned: No/Yes |

---

## Data Pipeline

```
Raw CSV  →  SQL Server  →  SQLQuery1.sql (clean & transform)  →  Tableau Dashboard 3
```

### Step 1 — Data Cleaning (SQL Server)

The script `SQLQuery1.sql` performs four key transformations on the imported table:

#### 1. Standardize Gender
```sql
UPDATE [dbo].[Depression+Student+Dataset]
SET Gender = 'F' WHERE Gender = 'Female'

UPDATE [dbo].[Depression+Student+Dataset]
SET Gender = 'M' WHERE Gender = 'Male'
```

#### 2. Add Age Group Category
Groups students into three age brackets:

```sql
ALTER TABLE [dbo].[Depression+Student+Dataset]
ADD Age_Group VARCHAR(MAX)

UPDATE [dbo].[Depression+Student+Dataset]
SET Age_Group =
  CASE WHEN Age BETWEEN 18 AND 24 THEN 'A1'
  ELSE CASE WHEN Age BETWEEN 25 AND 30 THEN 'A2'
  ELSE 'A3' END END
```

| Group | Age Range |
|---|---|
| A1 | 18 – 24 |
| A2 | 25 – 30 |
| A3 | 30+ |

#### 3. Add Auto-Increment Index
```sql
ALTER TABLE [dbo].[Depression+Student+Dataset]
ADD Index_Column INT IDENTITY(1,1)
```

#### 4. Convert Depression to Text
```sql
ALTER TABLE [dbo].[Depression+Student+Dataset]
ALTER COLUMN depression VARCHAR(MAX)

UPDATE [dbo].[Depression+Student+Dataset]
SET Depression = 'No' WHERE Depression = 0

UPDATE [dbo].[Depression+Student+Dataset]
SET Depression = 'Yes' WHERE Depression = '1'
```

The script also includes exploratory `GROUP BY` counts for every key column to understand value distributions before visualizing.

---

### Step 2 — Visualization (Tableau)

`Tableau Dashboard 3.twb` connects **live to SQL Server** (localhost, database: `Tableau Project 1`) and builds the dashboard directly from the cleaned table — no extract needed.

---

## Dashboard Breakdown

The dashboard **Student Count Analysis** contains five worksheets, each pairing a stressor variable against a score metric:

| Sheet | Abbreviation | What it shows |
|---|---|---|
| AP & SC | Academic Pressure & Score | Depression rate by academic pressure level |
| FS & SC | Financial Stress & Score | Depression rate by financial stress level |
| SD & SC | Sleep Duration & Score | Depression rate by sleep duration band |
| SH & SC | Study Hours & Score | Depression rate by daily study hours |
| SS & SC | Study Satisfaction & Score | Depression rate by study satisfaction level |

---

## Key Questions Explored

- Does higher academic pressure correlate with higher depression rates?
- Which sleep duration group shows the highest prevalence of depression?
- Is financial stress a stronger predictor than academic pressure?
- Do students who are more satisfied with their studies report lower depression?
- How does daily study time relate to mental health outcomes?

---

## How to Run This Project

### Prerequisites
- Microsoft SQL Server (any recent version)
- Tableau Desktop (with SQL Server connector)

### Steps

1. **Import the CSV into SQL Server**
   - Create a database called `Tableau Project 1`
   - Import `Depression+Student+Dataset.csv` into a table named `Depression+Student+Dataset`

2. **Run the cleaning script**
   - Open `SQLQuery1.sql` in SQL Server Management Studio (SSMS)
   - Run all queries against your database to clean and transform the data

3. **Open the Tableau workbook**
   - Open `Tableau Dashboard 3.twb` in Tableau Desktop
   - Update the data source connection to point to your localhost SQL Server instance
   - The dashboard will populate automatically from the cleaned table

> **Note:** The workbook uses a live connection to `localhost`. If your SQL Server is on a different host or uses a named instance, update the connection string in Tableau under **Data → Edit Connection**.

---
