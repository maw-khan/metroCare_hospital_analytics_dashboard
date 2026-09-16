# 🏥 MetroCare Hospital — Patient & Appointment Analytics Dashboard

<p align="center">

![Power BI](https://img.shields.io/badge/Power%20BI-Analytics-F2C811?style=for-the-badge\&logo=powerbi\&logoColor=black)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Database-4169E1?style=for-the-badge\&logo=postgresql\&logoColor=white)
![DAX](https://img.shields.io/badge/DAX-Data%20Analysis-0F6CBD?style=for-the-badge)
![Power Query](https://img.shields.io/badge/Power%20Query-ETL-742774?style=for-the-badge)
![Data Analytics](https://img.shields.io/badge/Data%20Analytics-Healthcare-2E8B57?style=for-the-badge)

</p>

<p align="center">
  <strong>End-to-end healthcare analytics project built with PostgreSQL and Microsoft Power BI</strong>
</p>

---

## 📌 Project Overview

**MetroCare Hospital — Patient & Appointment Analytics** is an end-to-end Business Intelligence project designed to help hospital administration understand operational and financial performance across patients, appointments, doctors, departments, insurance providers, and billing.

The project starts with structured hospital data stored in **PostgreSQL**, followed by data extraction, cleaning, transformation, modeling, DAX-based analysis, and interactive visualization in **Microsoft Power BI**.

The final dashboard provides a centralized analytical view of:

* 💰 Hospital revenue
* 📅 Appointment volume and trends
* 👥 Patient activity
* 👨‍⚕️ Doctor performance
* 🏥 Department performance
* 💳 Insurance vs. self-pay revenue
* ❌ Cancellation and no-show rates
* 📈 Revenue trends and time-based performance
* 🧾 Appointment-level billing details

The project follows a complete BI workflow:

```text
PostgreSQL
    ↓
Data Extraction
    ↓
Power Query
    ↓
Data Cleaning & Transformation
    ↓
Star Schema Data Model
    ↓
DAX Measures
    ↓
Power BI Visualizations
    ↓
Interactive Healthcare Dashboard
```

---

# 🎯 Business Problem

MetroCare Hospital operates a multi-department outpatient booking system where patients register, schedule appointments with doctors, and generate billing records for services such as consultations, laboratory tests, medicines, procedures, and room charges.

Hospital administration needs a way to answer questions such as:

* How much revenue is the hospital generating?
* How many appointments are being handled?
* How many unique patients are being served?
* Which departments generate the most revenue?
* Which doctors contribute the most revenue?
* How many appointments are cancelled or missed?
* What proportion of revenue comes from insured patients?
* How much revenue comes from self-pay patients?
* How is revenue changing over time?
* What was revenue in the previous month?
* How is month-over-month revenue changing?
* Where are patients located?
* Which patients generate the highest revenue?

The dashboard transforms the underlying transactional data into an interactive analytical system that can be used to investigate these questions.

---

# 🛠️ Technology Stack

| Technology           | Purpose                                                  |
| -------------------- | -------------------------------------------------------- |
| **PostgreSQL**       | Relational database and source data storage              |
| **SQL**              | Database setup and data management                       |
| **Power BI Desktop** | Data modeling, DAX and dashboard development             |
| **Power Query**      | Data cleaning and transformation                         |
| **DAX**              | Measures, calculated columns and analytical calculations |
| **Star Schema**      | Dimensional data modeling                                |
| **GitHub**           | Project documentation and version control                |

---

# 📂 Dataset & Database

The project uses a PostgreSQL database named:

```text
metrocare_hospital
```

The database contains six tables representing hospital departments, insurance providers, doctors, patients, appointments, and billing transactions. The original assignment specifies the six-table schema and approximate row counts below.

## Database Tables

| Table                 | Role      | Expected Rows |
| --------------------- | --------- | ------------: |
| `departments`         | Dimension |             8 |
| `insurance_providers` | Dimension |             6 |
| `doctors`             | Dimension |            26 |
| `patients`            | Dimension |           300 |
| `appointments`        | Fact      |         1,200 |
| `billing_items`       | Fact      |        ~2,315 |

### Main Relationships

```text
departments
     │
     │ 1 → many
     ▼
  doctors
     │
     │ 1 → many
     ▼
appointments ───────────────► billing_items
     ▲
     │
     │
  patients
     ▲
     │
insurance_providers

DimDate ───────────────► appointments
```

---

# 🗃️ Database Schema

## 1. Departments

Contains information about hospital departments.

Key fields include:

```text
department_id
department_name
floor_no
```

Example departments include:

* Cardiology
* Orthopedics
* Pediatrics
* Dermatology
* Neurology
* General Medicine
* ENT
* Gynecology

---

## 2. Insurance Providers

Contains insurance provider information and coverage details.

Key fields include:

```text
insurance_id
provider_name
coverage_pct
contact_number
```

The `contact_number` field is removed during Power Query transformation because it is not required for analytical reporting.

---

## 3. Doctors

Contains doctor-level information.

Key fields include:

```text
doctor_id
doctor_name
department_id
specialization
qualification
joining_date
consultation_fee
```

The data is transformed to make doctor information more suitable for business reporting.

---

## 4. Patients

Contains demographic and registration information.

Key fields include:

```text
patient_id
patient_name
gender
dob
city
phone
email
registration_date
blood_group
patient_category
insurance_id
```

Personal contact fields such as phone and email are removed from the analytical model because they are not required for the dashboard.

---

## 5. Appointments

The appointment table acts as one of the central fact tables.

Key fields include:

```text
appointment_id
patient_id
doctor_id
appointment_date
appointment_time
follow_up_date
appointment_status
payment_mode
appointment_mode
```

---

## 6. Billing Items

Contains individual billing transactions associated with appointments.

Key fields include:

```text
bill_item_id
appointment_id
item_type
quantity
unit_price
discount_pct
```

Billing data is used to calculate line-level and overall hospital revenue.

---

# 🔄 Project Workflow

The project was completed through four major phases.

## Phase 1 — PostgreSQL & Data Loading

The first phase established the database and connected Power BI to PostgreSQL.

### Steps

1. Created the `metrocare_hospital` database/schema in PostgreSQL.
2. Executed the provided SQL setup script.
3. Loaded the six hospital tables.
4. Verified table row counts.
5. Connected Power BI Desktop to PostgreSQL.
6. Selected **Import** mode.
7. Loaded the required tables into Power BI.

The assignment specifically requires verification of all six table row counts before proceeding with the analytical model.

---

# 🧹 Phase 2 — Power Query Data Cleaning

The raw hospital data intentionally contains several data-quality issues. These were addressed inside **Power Query** rather than modifying the original database.

This follows the project's intended ETL workflow: extract the data from PostgreSQL and perform reporting-specific transformations before loading it into the Power BI model.

---

## 2.1 Data Type Corrections

### Doctors

Converted:

```text
joining_date → Date
```

### Appointments

Converted:

```text
follow_up_date → Date
appointment_date → Date
appointment_time → Time
```

Blank follow-up dates were retained because a blank indicates that no follow-up appointment was booked.

### Billing Items

Converted:

```text
unit_price → Fixed Decimal Number
discount_pct → Fixed Decimal Number
```

This ensures that financial calculations retain their decimal precision.

---

# 🧹 2.2 Standardizing Appointment Status

The appointment status column contained inconsistent representations of the same categories.

Examples included:

```text
completed
COMPLETED
Cancelled
CANCELLED
NO-SHOW
no-show
NoShow
```

These were standardized into four consistent business categories:

```text
Completed
Cancelled
No-Show
Scheduled
```

This transformation is important because DAX treats differently spelled text values as different categories. Without standardization, status-based calculations such as cancellation and no-show rates can be inaccurate.

The column was also renamed:

```text
appointment_status
        ↓
Appointment Status
```

---

# 💳 2.3 Standardizing Payment Mode

The `payment_mode` column also contained inconsistent representations.

Examples included:

```text
Cash
CASH
cash

INS
Ins Claim
Insurance Claim
```

These were standardized to:

```text
Cash
Insurance Claim
```

The column was renamed:

```text
payment_mode
        ↓
Payment Mode
```

---

# 🧾 2.4 Billing Data Cleaning

Billing records containing invalid quantities were removed.

The filtering rule was:

```text
quantity > 0
```

Rows with:

```text
quantity = 0
quantity < 0
```

were treated as data-entry errors and excluded from the analytical model.

The assignment identifies these records as intentionally seeded data-quality issues that should be handled during Power Query transformation.

---

# 👨‍⚕️ 2.5 Doctor Data Transformation

The doctor table was transformed to improve readability and reporting usability.

Changes included:

* Converted `joining_date` to Date
* Split the doctor name field as required
* Renamed `doctor_name` to `Doctor Name`
* Renamed `cons_fee` / consultation fee to `Consultation Fee`

---

# 👥 2.6 Patient Data Transformation

Personal contact information that was not required for analytical reporting was removed.

Removed:

```text
phone
email
```

Renamed:

```text
patient_name → Patient Name
dob          → Date of Birth
```

Removing unnecessary personal information also keeps the analytical model focused on reporting requirements.

---

# 🏥 2.7 Insurance Provider Transformation

The following unnecessary field was removed:

```text
contact_number
```

The remaining provider information is sufficient for insurance-related analysis.

---

# 📊 2.8 Item Revenue Summary

A calculated **Line Revenue** value was created for billing items.

### Revenue Formula

```text
Line Revenue =
Quantity × (Unit Price − Discount Amount)
```

Equivalent DAX logic:

```DAX
Line Revenue =
billing_items[quantity]
    * (
        billing_items[unit_price]
        - (
            billing_items[unit_price]
            * billing_items[discount_pct]
            / 100
        )
      )
```

A reference query was then created from the billing table and grouped by service/item type.

The resulting summary contains:

```text
Service Type
Total Quantity
Total Revenue
```

This creates a pre-aggregated view of revenue by service category. The solution guide specifically emphasizes calculating revenue from the line-level revenue rather than summing unit prices.

---

# 🔗 2.9 Merge Queries

The appointments and patients tables were merged using:

```text
patient_id
```

A **Left Outer** join was used to preserve appointment records while retrieving the relevant patient-level category.

The patient category was expanded into the resulting data.

This transformation was used as a lookup/preview of patient-level information alongside appointment data.

---

# 📅 2.10 DimDate Table

A dedicated date dimension was created using DAX.

```DAX
DimDate =
ADDCOLUMNS (
    CALENDAR (
        DATE(2024,8,1),
        DATE(2025,7,31)
    ),
    "Year", YEAR ( [Date] ),
    "MonthNumber", MONTH ( [Date] ),
    "MonthName", FORMAT ( [Date], "MMMM" ),
    "Quarter", "Q" & FORMAT ( [Date], "Q" ),
    "DayName", FORMAT ( [Date], "dddd" ),
    "YearMonth", FORMAT ( [Date], "MMM YYYY" )
)
```

The date table contains:

| Column        | Purpose                    |
| ------------- | -------------------------- |
| `Date`        | Calendar date              |
| `Year`        | Calendar year              |
| `MonthNumber` | Numerical month            |
| `MonthName`   | Month name                 |
| `Quarter`     | Quarter label              |
| `DayName`     | Day name                   |
| `YearMonth`   | Month-year reporting label |

The `DimDate` table was marked as the official Power BI Date Table, and `MonthName` was sorted using `MonthNumber`.

A dedicated date table is used for time-intelligence calculations such as YTD and previous-month revenue.

---

# ⭐ Data Model — Star Schema

The final Power BI model follows a **star-schema structure**.

## Fact Tables

```text
appointments
billing_items
```

## Dimension Tables

```text
patients
doctors
departments
insurance_providers
DimDate
```

The relationships are:

| From                | To            | Cardinality |
| ------------------- | ------------- | ----------- |
| Patients            | Appointments  | 1 → Many    |
| Doctors             | Appointments  | 1 → Many    |
| Appointments        | Billing Items | 1 → Many    |
| Departments         | Doctors       | 1 → Many    |
| Insurance Providers | Patients      | 1 → Many    |
| DimDate             | Appointments  | 1 → Many    |

The completed model places the fact tables centrally and the dimension tables around them, producing the intended star-shaped analytical model.

---

# 📐 Phase 3 — DAX Analytics

A dedicated measure table was created to organize the analytical measures.

```DAX
Measure Table =
ROW("x", 1)
```

The automatically generated column was removed, leaving the table to serve as a dedicated container for measures.

---

# 📊 Key DAX Measures

## 1. Total Billed Revenue

Calculates total revenue after applying discounts.

```DAX
Total Billed Revenue =
SUMX(
    billing_items,
    billing_items[quantity]
        * (
            billing_items[unit_price]
            - (
                billing_items[unit_price]
                * billing_items[discount_pct]
                / 100
            )
        )
)
```

---

## 2. Distinct Appointments

```DAX
Distinct Appointments =
DISTINCTCOUNT(
    appointments[appointment_id]
)
```

Counts unique appointments rather than counting rows blindly.

---

## 3. Distinct Patients

```DAX
Distinct Patients =
DISTINCTCOUNT(
    patients[patient_id]
)
```

Counts unique patients represented in the dataset.

---

## 4. Average Revenue per Appointment

```DAX
Average Revenue =
DIVIDE(
    [Total Billed Revenue],
    [Distinct Appointments]
)
```

Calculates average billed revenue generated per appointment.

---

## 5. Completed Appointments

```DAX
Completed Appointments =
CALCULATE(
    [Distinct Appointments],
    appointments[Appointment Status] = "Completed"
)
```

Uses `CALCULATE` to evaluate appointment count under a specific status filter.

---

## 6. Cancellation Rate

```DAX
Cancelled Appointment % =
DIVIDE(
    CALCULATE(
        [Distinct Appointments],
        appointments[Appointment Status] = "Cancelled"
    ),
    [Distinct Appointments]
) * 100
```

Measures the percentage of appointments that were cancelled.

---

## 7. No-Show Rate

```DAX
No-Show Appointment % =
DIVIDE(
    CALCULATE(
        [Distinct Appointments],
        appointments[Appointment Status] = "No-Show"
    ),
    [Distinct Appointments]
) * 100
```

Measures the percentage of appointments where patients did not show up.

---

## 8. Active Insurance Revenue

```DAX
Active Insurance Revenue =
CALCULATE(
    [Total Billed Revenue],
    appointments[Payment Mode] = "Insurance Claim"
)
```

Calculates revenue associated with insurance-claim payment transactions.

---

## 9. Self-Pay Revenue

```DAX
Self Pay Revenue =
[Total Billed Revenue]
    - [Active Insurance Revenue]
```

Calculates revenue not attributed to insurance claims.

---

## 10. Total Patients

```DAX
Total Patients =
DISTINCTCOUNT(
    patients[patient_id]
)
```

---

## 11. Total Appointments

```DAX
Total Appointments =
DISTINCTCOUNT(
    appointments[appointment_id]
)
```

---

## 12. Revenue YTD

```DAX
Total YTD =
TOTALYTD(
    [Total Billed Revenue],
    DimDate[Date]
)
```

Calculates cumulative revenue from the beginning of the year through the selected date context.

---

## 13. Revenue — Previous Month

```DAX
Revenue Last Month =
CALCULATE(
    [Total Billed Revenue],
    DATEADD(
        DimDate[Date],
        -1,
        MONTH
    )
)
```

Retrieves revenue from the previous calendar month.

---

## 14. Month-over-Month Revenue Growth

```DAX
MoM Revenue Growth % =
DIVIDE(
    [Total Billed Revenue] - [Revenue Last Month],
    [Revenue Last Month]
) * 100
```

Measures percentage change in revenue compared with the previous month.

The project uses `SUMX`, `DISTINCTCOUNT`, `DIVIDE`, `CALCULATE`, `TOTALYTD`, and `DATEADD` to answer the eight core analytical questions specified by the assignment.

---

# 📈 Phase 4 — Dashboard & Report Design

The Power BI report consists of multiple analytical pages designed around different business questions.

---

# 1️⃣ Executive Overview

The Executive Overview provides a high-level snapshot of hospital performance.

### Key Components

**KPI Cards**

* Total Revenue
* Total Appointments
* Total Patients
* Cancellation Rate

**Revenue Trend**

A monthly line chart showing revenue over time.

**Revenue by Department**

A department-level revenue visualization showing how revenue is distributed across hospital departments.

**Slicers**

Interactive filters allow users to analyze the report by:

* Date
* Department
* Patient Category

The assignment requires the Executive Overview to provide revenue, appointment, patient, cancellation, monthly revenue-trend and department-level views.

---

# 2️⃣ Patient Insights

The Patient Insights page focuses on patient-level and geographic analysis.

### Components

**Patient Category Analysis**

Breakdown of revenue or patients by patient category.

**Top 10 Patients**

A table showing:

```text
Patient Name
Total Appointments
Total Revenue
```

The table is filtered to the top 10 patients based on revenue.

**Geographic Analysis**

A map visual showing patient distribution/revenue by city.

---

# 3️⃣ Doctor & Department Performance

This page evaluates operational performance across doctors and departments.

### Components

**Revenue by Doctor**

A ranked-style bar visualization showing revenue generated by individual doctors.

**Appointment Status by Department**

A stacked column chart showing appointment status across departments.

Categories include:

```text
Completed
Cancelled
No-Show
Scheduled
```

**No-Show KPI**

A dedicated KPI displays the no-show rate.

---

# 4️⃣ Appointment Details — Drillthrough

A dedicated drillthrough page provides detailed appointment-level information.

The table includes:

```text
Appointment ID
Patient Name
Doctor
Appointment Date
Appointment Status
Service Type
Quantity
```

The drillthrough is configured using:

```text
appointment_id
```

Users can right-click a relevant visual on another report page and navigate to the detailed appointment record.

The assignment requires the detailed page to be reachable through drillthrough from another report visual.

---

# 🎨 Report Design & Interactivity

The report incorporates Power BI functionality beyond basic charts.

### Features Implemented

* Consistent report theme
* KPI cards
* Line charts
* Bar charts
* Stacked column charts
* Donut/chart-based breakdowns
* Tables
* Geographic map
* Slicers
* Visual-level filters
* Page-level filters
* Conditional formatting
* Drillthrough navigation

These features correspond to the report-design requirements specified in the assignment.

---

# 🔍 Data Quality Issues Identified

The project intentionally contains several data-quality problems that simulate real-world reporting data.

| Issue                                    | Solution                                 |
| ---------------------------------------- | ---------------------------------------- |
| Incorrect date data types                | Converted relevant fields to Date/Time   |
| Inconsistent appointment status          | Standardized status labels               |
| Inconsistent payment modes               | Standardized payment labels              |
| Invalid billing quantities               | Removed records where `quantity <= 0`    |
| Unnecessary patient contact data         | Removed phone and email                  |
| Unnecessary insurance contact data       | Removed contact number                   |
| Decimal financial fields                 | Converted to Fixed Decimal               |
| Business-unfriendly field names          | Renamed relevant columns                 |
| Service-level revenue not pre-aggregated | Created Line Revenue and grouped summary |
| No dedicated calendar dimension          | Created and marked `DimDate`             |

The original dataset was intentionally designed with imperfect fields, including inconsistent casing/abbreviations, incorrect date types, blank follow-up dates and invalid billing quantities.

---

# 🧠 Key Power BI Concepts Demonstrated

This project demonstrates practical understanding of:

### Data Extraction

* PostgreSQL → Power BI connection
* Import mode
* Relational data sources

### Power Query

* Change Data Type
* Remove Columns
* Rename Columns
* Split Column
* Replace Values
* Filter Rows
* Reference Queries
* Group By
* Merge Queries
* Custom Columns

### Data Modeling

* Fact tables
* Dimension tables
* Primary/foreign key relationships
* One-to-many relationships
* Star schema
* Date dimension
* Date table configuration

### DAX

* `SUMX`
* `DISTINCTCOUNT`
* `DIVIDE`
* `CALCULATE`
* `TOTALYTD`
* `DATEADD`
* Calculated columns
* Measures
* Row context
* Filter context
* Time intelligence

### Visualization

* KPI cards
* Line charts
* Bar charts
* Stacked charts
* Donut charts
* Tables
* Maps
* Slicers
* Conditional formatting
* Drillthrough

---

# 📁 Repository Structure

```text
MetroCare-Hospital-PowerBI-Analytics/
│
├── README.md
│
├── data/
│   ├── departments.csv
│   ├── insurance_providers.csv
│   ├── doctors.csv
│   ├── patients.csv
│   ├── appointments.csv
│   └── billing_items.csv
│
├── sql/
│   └── 00_setup_metrocare_hospital.sql
│
├── powerbi/
│   └── Group5_MetroCare_Dashboard.pbix
│
├── report/
│   ├── MetroCare_Dashboard.pdf
│   └── MetroCare_Project_Summary.pdf
│
├── screenshots/
│   ├── executive_overview.png
│   ├── patient_insights.png
│   ├── doctor_department_performance.png
│   ├── appointment_details.png
│   ├── data_model.png
│   └── power_query.png
│
└── documentation/
    ├── data_cleaning.md
    ├── data_model.md
    └── dax_measures.md
```

> **Note:** The structure above is the recommended portfolio structure. Files can be adjusted according to the actual artifacts included in the repository.

---

# 🚀 How to Reproduce the Project

## 1. Set Up PostgreSQL

Create the required PostgreSQL database/schema using the SQL setup script:

```text
sql/00_setup_metrocare_hospital.sql
```

Execute the script using PostgreSQL/pgAdmin.

---

## 2. Verify the Data

Confirm that the six tables have the expected approximate row counts:

```text
departments          8
insurance_providers  6
doctors              26
patients             300
appointments        1200
billing_items       ~2315
```

---

## 3. Connect Power BI

Open Power BI Desktop and select:

```text
Get Data
    ↓
PostgreSQL Database
```

Connect to the `metrocare_hospital` database and select **Import** mode.

Load the six required tables.

---

## 4. Transform the Data

Open:

```text
Transform Data
```

and reproduce the Power Query transformations documented in this repository.

---

## 5. Build the Data Model

Create the `DimDate` table and configure the relationships between fact and dimension tables.

Verify that all relationships are:

```text
One-to-Many
Active
Single-direction
```

---

## 6. Create DAX Measures

Create the dedicated measure table and add the documented calculated column and measures.

---

## 7. Build the Dashboard

Recreate the report pages:

```text
Executive Overview
Patient Insights
Doctor & Department Performance
Appointment Details
```

---

# 📊 Analytical Questions Answered

The dashboard is designed around eight primary business questions:

|  # | Business Question                                          | Main DAX Concepts                   |
| -: | ---------------------------------------------------------- | ----------------------------------- |
|  1 | What is the hospital's total billed revenue?               | `SUMX`                              |
|  2 | How many distinct appointments and patients are there?     | `DISTINCTCOUNT`                     |
|  3 | What is the average revenue per appointment?               | `DIVIDE`                            |
|  4 | How many appointments were completed?                      | `CALCULATE`                         |
|  5 | What percentage of appointments were cancelled or no-show? | `CALCULATE`, `DIVIDE`               |
|  6 | How much revenue comes from insurance vs. self-pay?        | Relationship filtering, `CALCULATE` |
|  7 | What is the revenue YTD?                                   | `TOTALYTD`                          |
|  8 | What was previous-month revenue and the MoM change?        | `DATEADD`, `DIVIDE`                 |

These questions correspond directly to the analytical requirements defined in the project brief.

---

# 💡 Business Value

The dashboard converts raw transactional hospital data into a reporting layer that can help administration investigate:

* Financial performance
* Revenue trends
* Department-level contribution
* Doctor-level performance
* Appointment utilization
* Cancellation behavior
* No-show behavior
* Patient segments
* Geographic distribution
* Insurance vs. self-pay revenue

Instead of analyzing six independent tables, users can interact with a unified model and dynamically filter the analysis across multiple dimensions.

---

# 📸 Dashboard Preview

> Screenshots of the completed Power BI report can be added here.

### Executive Overview

screenshots/executive_overview.png

### Patient Insights

```text
[ Add patient_insights.png here ]
```

### Doctor & Department Performance

```text
[ Add doctor_department_performance.png here ]
```

### Appointment Details

```text
[ Add appointment_details.png here ]
```

### Data Model

```text
[ Add data_model.png here ]
```

---

# 📚 Learning Outcomes

This project provided hands-on experience with a complete Business Intelligence workflow:

```text
Raw Data
   ↓
Relational Database
   ↓
Data Extraction
   ↓
Data Cleaning
   ↓
Data Transformation
   ↓
Dimensional Modeling
   ↓
DAX Analytics
   ↓
Interactive Visualization
   ↓
Business Insights
```

The project particularly strengthened practical understanding of the relationship between **data preparation, data modeling and analytical calculations**. Cleaning and modeling decisions directly affect the accuracy of downstream DAX measures and dashboard visualizations.

---

# 🎓 Project Context

This project was completed as part of a **Power BI Group Project** covering the practical skills taught through the project's Day 1–Day 4 training sequence.

The assignment was designed to assess:

* PostgreSQL connectivity
* Power Query transformation
* Star-schema modeling
* DAX calculations
* Time intelligence
* Dashboard design
* Data visualization
* Filtering and drillthrough
* Business-oriented analysis

The original project requirements specify a `.pbix` dashboard, exported report, and project summary as the final deliverables.

---

# 👤 Skills Demonstrated

### Technical Skills

```text
PostgreSQL
SQL
Power BI
Power Query
DAX
Data Modeling
Star Schema
ETL
Data Cleaning
Time Intelligence
Data Visualization
Business Intelligence
```

### Analytical Skills

```text
Data Quality Analysis
KPI Development
Revenue Analysis
Operational Analysis
Patient Analytics
Trend Analysis
Comparative Analysis
Business Reporting
```

---

# ⚠️ Data Privacy Note

The project dataset is a provided/sample hospital dataset intended for educational and analytical purposes.

The Power BI model removes unnecessary personal contact fields such as patient phone numbers and email addresses because they are not required for the analytical objectives of the dashboard.

No real patient information should be added to or exposed through this repository.

---

# 📌 Project Status

**Completed ✅**

The project includes the complete workflow from PostgreSQL data preparation through Power BI modeling, DAX analysis and interactive dashboard development.

---

# ⭐ Key Takeaways

This project demonstrates how a BI solution can be developed from the ground up:

**PostgreSQL** provides the structured source data.

**Power Query** handles data-quality issues and transformations.

**Star-schema modeling** establishes reliable relationships between facts and dimensions.

**DAX** converts the model into analytical metrics.

**Power BI visualizations** transform those metrics into an interactive decision-support dashboard.

Together, these components form an end-to-end healthcare Business Intelligence solution for MetroCare Hospital.

---

## 📬 Contact

**Muhammad Ali Waris Khan**

For questions, feedback, or collaboration, feel free to connect through GitHub or LinkedIn.

---

<p align="center">
  <strong>MetroCare Hospital Analytics • PostgreSQL + Power BI • Business Intelligence</strong>
</p>
