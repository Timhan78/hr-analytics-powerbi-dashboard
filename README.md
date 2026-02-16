# HR Analytics Dashboard: Atlas Labs Case Study

![Project Status](https://img.shields.io/badge/Status-Completed-success)
![Tools](https://img.shields.io/badge/Tools-Power_BI_|_DAX_|_Power_Query-blue)
![Source](https://img.shields.io/badge/Source-DataCamp_Case_Study-orange)

---
## 📂 Project Files

| File | Description |
|------|-------------|
| `AtlasLabs_HR_Analytics.pbix` | Full Power BI project file | |
| `screenshots/` | Final dashboards, Dashboard visuals, model view, DAX measures, and Power Query steps |


---

## 📋 Project Overview

This project involves the end-to-end development of an interactive HR dashboard for a fictional company, **Atlas Labs**.

Built as part of the DataCamp Power BI case study, with independent problem-solving, debugging, and documentation of lessons learned throughout the process.

**Primary goal:** Monitor key HR metrics (Total Employees, Attrition Rate) and provide deep insights into the factors driving employee turnover.

---

## 🛠 Tech Stack

- **Power BI Desktop** (Data Modeling & Visualization)
- **Power Query / M** (ETL Processing)
- **DAX** (Business Logic & Measures)

---

## 🚀 Key Implementation Phases

### 1️⃣ Data Preparation (ETL)

- Verified and corrected data types for five core tables.
- Created calculated fields (`FullName`, `AgeBins`) in Power Query for model stability.
- Applied conditional column logic with correct boundary handling (`<=` instead of `<`).

### 2️⃣ Data Modeling

- Implemented a **Star Schema** architecture with Fact/Dim naming convention.
- Built a dynamic **DAX Calendar Table (DimDate)** that adjusts automatically based on `MIN`/`MAX` employee hire dates.
- Managed **Inactive Relationships** using `USERELATIONSHIP()` to analyse hiring trends by `HireDate` without conflicting with active relationships.
- Applied the one-active-relationship rule across multiple dimension connections (`DimRatingLevel`, `DimSatisfiedLevel`).

### 3️⃣ DAX Measures

Developed key business measures stored in a dedicated `_Measures` table:

- **Core HR Metrics:** `TotalEmployees`, `ActiveEmployees`, `InactiveEmployees`, `% Attrition Rate`
- **Time-based Analysis:** `TotalEmployeesDate` and `InactiveEmployeesDate` using `USERELATIONSHIP()`
- **Performance Tracking:** `LastReviewDate` with `ISBLANK()` handling; `NextReviewDate` with 365-day offset using `VAR/RETURN`
- **Satisfaction Metrics:** `EnvironmentSatisfaction`, `RelationshipSatisfaction`, `WorkLifeBalance`, `JobSatisfaction` — each activating inactive relationships via `CALCULATE()` + `USERELATIONSHIP()`

### 4️⃣ Visualization & Design

The report includes four interactive pages:

1. **Overview** — High-level KPIs, hiring trends (stacked column), department breakdown (treemap)
2. **Demographics** — Age distribution, gender split, ethnicity & average salary comparison
3. **Performance Tracker** — Individual employee slicer with review dates and satisfaction ratings over time
4. **Attrition** — Turnover drivers by overtime, travel frequency, tenure, department & job role

**Design standards applied:**
- Custom theme (`HR Analytics`) with consistent fonts and colours
- `#f7f7fc` canvas background with visual shadows for depth
- Navigation bar and Page Navigator for seamless page switching
- Page-level filters for Active/Inactive employee toggle

---

## 🔧 Challenges & Solutions

These are real problems I encountered during the build and the lessons I took from each one.

### Date Column Imported as Text
**Problem:** The `ReviewDate` column was imported as Text instead of Date. Everything appeared normal until I noticed that `MAX()` was returning `3/25/2019` instead of `3/24/2022` — because text sorts alphabetically, not chronologically.

**Impact:** Spent ~1 hour debugging before finding the root cause.

**Solution:** Changed the column type to Date in Power Query. Established a personal rule: always check the column icon in the Fields pane (calendar = Date, ABC = Text) before writing any DAX.

---

### Cyclic Reference from DAX Calculated Column
**Problem:** Created a `FullName` column using `COMBINEVALUES()` as a DAX calculated column inside `DimEmployee`. Power BI threw a cyclic dependency error because the calculated column depended on the table that was still being evaluated.

**Solution:** Moved the column creation to Power Query using `Table.AddColumn()`. This eliminated the dependency issue and improved model stability.

**Lesson:** If a column does not require DAX filter context, create it in Power Query (ETL stage), not in the data model.

---

### 237 Missing Employees from Hidden Filter
**Problem:** My `TotalEmployees` card showed 1,233 instead of the expected 1,470. After checking the data source and all DAX measures, I discovered a page-level filter (`Attrition = No`) that was silently excluding 237 inactive employees from every visual on the page.

**Solution:** Removed the unintended filter. Created a personal validation checklist: when totals don't match, check all three filter levels (Visual → Page → Report) before debugging DAX.

---

### AgeBins Boundary Error
**Problem:** Age groups `20-29` and `30-39` were missing employees aged exactly 29 and 39. The conditional column used `less than` instead of `less than or equal to`, causing these ages to fall between groups.

**Solution:** Changed all conditions to use `<=` (e.g., `Age <= 29` → "20-29"). A small logical error, but it affected data accuracy across the entire Demographics page.

---

## 📈 Key Insights

- **Overtime is the strongest attrition signal:** Employees required to work overtime show a significantly higher attrition rate compared to those who do not.
- **First 2 years are the highest-risk period:** The majority of employee departures happen within the first two years of tenure, suggesting onboarding and early engagement are critical.
- **Technology dominates headcount:** The Technology department has the largest active workforce, followed by Sales and Human Resources.
- **Travel frequency correlates with turnover:** Employees who travel frequently show higher attrition than those who rarely or never travel.

---

## 🧠 What I Would Do Differently

- **Start with a data type audit.** Before any DAX, systematically verify every column type in Power Query. This would have saved the most debugging time.
- **Keep all transformations in Power Query.** The cyclic reference taught me that Power Query is for data shaping; DAX is for business logic.
- **Document filter states.** Page-level and report-level filters are invisible unless you check. A pre-publish checklist would catch these earlier.

---

## 📬 Contact

**Tim Siraziev** — [LinkedIn](https://www.linkedin.com/in/timursiraziev/) | [GitHub](https://github.com/Timhan78)

Currently completing a Level 4 Data Analyst Apprenticeship (Corndel) while working at British Red Cross. Open to data analyst opportunities in the UK charity sector.
