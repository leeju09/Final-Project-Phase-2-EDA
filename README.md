# Pathways to Scholarships — Phase 2: Exploratory Data Analysis


An exploratory data analysis (EDA) of a combined scholarship dataset for the **Pathways to Scholarships** final project (DSCI 2001-51, Fall 2026).

Seven student-collected Excel workbooks were combined into a single master DataFrame of **160 scholarship records** and analyzed for Phase 2: cleaning, standardization, summary statistics, frequency distributions, data quality assessment, and initial comparisons across states, grade levels, categories, award amounts, and deadlines.

---

## Table of Contents

- Overview
- Dataset
- Phase 2 Objectives
- Project Structure
- Analysis Walkthrough
- Key Findings
- Author
- Acknowledgements

---

## Overview

This project is the **Phase 2 (EDA)** stage of a larger **Pathways to Scholarships** class project. The goal is to build a structured, cleaned dataset that helps middle and high school students and parents understand what scholarships exist, who is eligible, how much funding is available, when applications are due, and what factors affect access.

Phase 2 specifically focuses on:

- Cleaning and standardizing messy, mixed-format columns
- Computing summary statistics and frequency distributions
- Identifying missing values, duplicates, expired listings, and inconsistent categories
- Performing initial comparisons to surface patterns worth deeper analysis in Phase 3

The notebook is self-contained, reproducible, and fully documented with markdown commentary explaining every step.

---

## Dataset

**Source:** Seven student-collected Excel workbooks combined into one master DataFrame:

| File | Content | Rows |
|---|---|---|
| `Kone_Cheick_National_Scholarships.xlsx` | National general scholarships | 25 |
| `Kone_Cheick_NH_Scholarships.xlsx` | New Hampshire scholarships | 25 |
| `Haji-Said_Yahya_National_Scholarships.xlsx` | National women-focused scholarships | 18 |
| `Haji-Said_Yahya_Indiana_Scholarships.xlsx` | Indiana women-focused scholarships | 22 |
| `Welzant_Kayla_scholarships_NY_leadership_civic_N-Z.xlsx` | New York leadership/civic scholarships | 15 |
| `Lee_Julia_Data.xlsx` | New Mexico STEM scholarships | 23 |
| `Jaibrien_Brooks Scholarship Data.xlsx` | California + national teaching scholarships | 32 |
| **Total** | | **160** |

All files share the same **29-column template schema**:

| Column | Type | Description |
|---|---|---|
| Scholarship name | text | Name of the award |
| Scholarship provider/organization | text | Who offers it |
| Provider type | text | Foundation, corporation, college, government, etc. |
| Official scholarship URL | text | Link to the provider's page |
| Discovery source | text | Where the record was found |
| State/region | text | Geographic scope |
| National vs. state/local | text | Geographic classification |
| Eligible grade levels | text | Grade(s) eligible |
| Middle school / high school / both | text | Grade-level category |
| Scholarship category | text | Merit, need, STEM, service, etc. |
| Field/area of interest | text | Academic or career field |
| Award amount | text | Human-readable dollar amount |
| Number of awards | text | Count of awards offered |
| Renewable | text | Yes / No / Unknown |
| Maximum potential value | text | Full award value (often text) |
| Application open date | text | When applications open |
| Application deadline | text | When applications close |
| Residency requirement | text | Geographic eligibility |
| Minimum GPA | text | GPA cutoff |
| Financial need required | text | Yes / No / Unknown |
| Essay required | text | Yes / No / Unknown |
| Recommendation required | text | Yes / No / Unknown |
| Transcript/test score required | text | Yes / No / Unknown |
| Community service requirement | text | Yes / No / Unknown |
| Leadership requirement | text | Yes / No / Unknown |
| Other major eligibility requirements | text | Free-text notes |
| Application method | text | Online, mail, nomination, etc. |
| Verification date | date | When the record was last checked |
| Notes | text | Researcher comments |

---

## Phase 2 Objectives

The notebook explicitly addresses all four Phase 2 objectives:

1. **Clean and standardize** — award amount, grade level, deadline, scholarship type, state, and eligibility fields
2. **Calculate summary statistics and frequency distributions** — for numerical and categorical fields
3. **Identify data quality issues** — missing values, duplicate scholarships, expired listings, inconsistent categories
4. **Perform initial comparisons** — across states, grade levels, scholarship categories, award amounts, and deadlines

Each objective has a dedicated section in the notebook with clear output and markdown commentary.

---

## Project Structure

│
├── README.md # Project documentation (this file)

├── Lee_Julia_Phase2_EDA.ipynb # Main Phase 2 EDA notebook

├── combined_scholarships_cleaned.xlsx # Cleaned dataset (output of notebook)

│
└── data/ # Original source files

├── Kone_Cheick_National_Scholarships.xlsx

├── Kone_Cheick_NH_Scholarships.xlsx

├── Haji-Said_Yahya_National_Scholarships.xlsx

├── Haji-Said_Yahya_Indiana_Scholarships.xlsx

├── Welzant_Kayla_scholarships_NY_leadership_civic_N-Z.xlsx

├── Lee_Julia_Data.xlsx

└── Jaibrien_Brooks Scholarship Data.xlsx

## Analysis Walkthrough

## **Part 1** — Combining the Seven Files

- Loaded all 7 Excel files with pd.read_excel using sheet_name=0 (the data sheet)

- Added a _source_file tracking column so each record remains traceable to its origin

- Concatenated with pd.concat(ignore_index=True) into a 160 × 30 master DataFrame

## **Part 2** — Cleaning & Standardization

Award amounts:

- Regex-extracted maximum dollar value into a new award_max_num column

- Bucketed into standard ranges (Under $1,000, $1,000–$4,999, $5,000–$9,999, $10,000 or more)

Grade levels:

- Parsed min/max grade numbers into grade_min and grade_max

- Correctly handles ranges like "11–12" → (11, 12)

Deadlines:

- Converted to real datetime via pd.to_datetime(..., fuzzy=True) into deadline_dt

- Added deadline_month and deadline_season for temporal comparison

Yes/No/Unknown fields:

- Regex-prefix normalized into *_clean columns for reliable filtering

Provider type and state:

- Title-cased provider type into provider_type_clean

- Extracted primary state into state_clean

## **Part 3** — Summary Statistics
Overall size: 160 records, 7 source files, 140 unique providers

Award amount distribution: mean $13,321.89**, median **$4,000, max $120,000**, min **$250

Grouped summaries by provider type, state, national vs. state/local, and source file

## Part 4 — Frequency Distributions
Provider type, national vs. state/local, grade level, category, renewable status, essay requirement, recommendation requirement, financial need, award bucket, deadline season, and state

## Part 5 — Data Quality Assessment
Missing values: 14.7% of cells missing (1,056 out of 7,200); only 13 of 45 columns 100% complete

Duplicates: 0 exact rows, 0 duplicate names, 0 cross-file overlaps

Expired listings: 22 of 58 dated records already past their deadline

Inconsistent categories: Provider type, Renewable, and Essay fields all show heavy format variance — now normalized

## Part 6 — Initial Comparisons
By state: National (48), Indiana (23), Unknown (22), New Mexico (20), California (16), New York (15), etc.

By grade level: High school dominates (105 of 160), middle school is rare (5)

By category: Merit-based (19) leads, followed by Women-focused / STEM (7) and Need-based (6)

By award amount: Heavily right-skewed; median ($4,000) is a better typical-value indicator than mean ($13,322)

By deadline: Winter (28) and Spring (22) dominate; January and March are the busiest months

## Key Findings
160 scholarships were combined across 7 source files with no duplicate records or duplicate names.

Award amounts range from $250 to $120,000, with a median of $4,000** but a mean of **$13,322 — heavily right-skewed because a few national awards (NSA Stokes $120k, Davidson Fellows $100k) pull the average up.

Foundations and nonprofits dominate the provider landscape (68 of 160, 42.5%).

State vs. national is nearly split — 83 state/local, 64 national.

High-school-only awards dominate (105 of 160), with middle-school-specific opportunities rare (5 records).

Jan and Mar are the busiest deadline months, confirming the typical late-winter/early-spring scholarship cycle.

14.7% of all cells are missing — the biggest data-quality limitation of the combined dataset.

22 of 58 dated records have already expired, so any Phase 3 recommendations must filter or re-verify them.

Most important insight: Scholarship award size is not the only indicator of value — small $500–$2,000 local awards often have far less competition than the $40,000+ national prizes, making them strategically worthwhile for many students.

## Author
Julia Lee

Course: DSCI 2001-51 Data Science I

Semester: Fall 2026

Project: Pathways to Scholarships

GitHub: @leeju09

Email: julia.lee@mynhcc.edu

## Acknowledgements
Project structure and Phase 2 objectives provided by DSCI 2001-51 course staff

Data collected by seven student researchers across national, women-focused, Indiana, New York, New Mexico, New Hampshire, and California scholarship sources

Official scholarship information sourced from each provider's website (see Discovery source and Notes columns in the dataset)



