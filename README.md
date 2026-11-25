# 🏥 Hospital Analysis Dashboard — Power BI

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-0078D4?style=for-the-badge)
![Power Query](https://img.shields.io/badge/Power%20Query-217346?style=for-the-badge)
![Excel](https://img.shields.io/badge/Excel-217346?style=for-the-badge&logo=microsoftexcel&logoColor=white)

---

## 📌 Project Overview

This project is an end-to-end interactive **Hospital Analysis Dashboard** built in **Microsoft Power BI**. It brings together patient, doctor, clinical operations and financial data into one report, covering the full analytical pipeline — from raw data ingestion and transformation in **Power Query**, to KPI calculations and what-if analysis in **DAX**, to a polished multi-page dashboard designed for hospital administrators, department heads, clinicians and finance teams.

The dashboard spans **6 report pages**: **Home, Overview, Patient, Doctor, Hospital, and Finance** — each delivering targeted insights to a different group of stakeholders.

---

## 🎯 Problem Statement

Hospitals generate large volumes of data across admissions, appointments, surgeries, lab tests, bed management, pharmacy and billing. When this data sits in separate systems, it becomes difficult to see how the hospital is performing as a whole — or to spot problems such as bed shortages, revenue leakage or uneven doctor workloads.

This project addresses the following key business questions:

- How many patients are currently admitted, and what share have been discharged?
- Which charge types (surgery, room, tests, medicine, fees) drive the most revenue?
- How are patients distributed across age groups, and what does that mean for care planning?
- How is bed capacity being used across ICU, General and Private wards?
- How much does each doctor generate in patient spend, fees and commission?
- Which medicines sell the most, how do sales trend month to month, and how do supplier sales compare with stock?
- How do patients rate the care they received?

The goal is to provide a **single source of truth** that lets hospital stakeholders monitor KPIs, track patient journeys and make data-driven operational and financial decisions.

---

## 🛠️ Tools & Technologies

| Tool / Technology | Purpose |
|---|---|
| Microsoft Power BI Desktop | Dashboard development and data visualization |
| Power Query (M Language) | Data ingestion, cleaning, and transformation (ETL) |
| DAX (Data Analysis Expressions) | KPI measures, ratios, what-if parameters |
| Data Modeling | Star schema relationships between fact and dimension tables |
| What-If Parameters | Interactive commission calculator on the Doctor page |
| Custom Visual Design | Card-based layout, 3D illustrations and rounded containers |
| Page Navigation Buttons | Cross-page navigation for a seamless app-like UX |

---

## 🗂️ Dataset Description

The dataset simulates a real-world hospital environment (May – October 2023) and includes data across the following domains:

| Table / Domain | Key Fields |
|---|---|
| Patients | patient_name, age, gender, weight, address, diagnosis, department, bed_id, admission_date, discharge_date, patient_status |
| Doctors | doctor_name, specialization, department, salary, availability, commission_rate |
| Staff | staff_id, role, department |
| Appointments | doctor_name, patient_name, appointment_date, time, reason, status |
| Surgeries | patient_name, surgery_date, time, status |
| Lab Tests | patient_name, test_status, result, notes |
| Rooms / Beds | room_type (ICU / General / Private), bed_id, occupancy_status |
| Billing | bill_id, patient_name, charge_type, amount, fees, total_bill_amount |
| Medicine Sales | medicine_name, quantity, sales, stock, supplier, sale_date |
| Suppliers | supplier_name |
| Ratings | patient_name, doctor_name, rating, feedback |
| Date Table | Month, Day of Week (used for time-series and heatmap analysis) |

**Key DAX measures created include:**

```DAX
Patient Count        = DISTINCTCOUNT(Patients[patient_id])
Doctor Count         = DISTINCTCOUNT(Doctors[doctor_id])
Staff Count          = DISTINCTCOUNT(Staff[staff_id])
Bill Count           = COUNTROWS(Billing)
Total Bill Amount    = SUM(Billing[amount])
Med Sales Qty        = SUM(MedicineSales[quantity])
Doctor Salary        = SUM(Doctors[salary])
Admitted Patients    = CALCULATE([Patient Count], Patients[patient_status] = "Admitted")
Discharged Patients  = CALCULATE([Patient Count], Patients[patient_status] = "Discharged")
Discharge %          = DIVIDE([Discharged Patients], [Patient Count], 0)
Patient Spend        = SUM(Billing[total_bill_amount])
Commission Earned    = [Patient Spend] * [Commission Rate]
Estimated Commission = -- What-if measure driven by the Commission Rate and Patient Spend parameters
```

---

## 📊 Power BI Dashboards

The report consists of **6 interactive pages**:

### 🏠 Page 1 — Home

A branded landing page with a 3D hospital illustration and navigation cards that describe each section and jump directly to it.

![Home](Images/Home.png)

---

### 🔍 Page 2 — Overview

A cross-functional summary giving hospital leadership a bird's-eye view of operations.

**KPI Cards:** Patient Count · Doctor Count · Staff Count · Bill Count

**Visuals:**
- 📈 Patient Discharge by Date (area chart, May – Oct 2023)
- 📊 Patient Charges Type (column chart — Surgery, Room, Test, Other, Fees, Discount, Medicine)
- 🗓️ Medicine Tracking by Month and Day (matrix heatmap)
- ⭐ Overall Patient Ratings (star rating)
- 🟥 Patient Admission Summary — admitted patients, discharged count and Discharge % with progress bar
- 📋 Upcoming Appointments (card list with doctor photos, time and date)

![Overview](Images/Overview.png)

---

### 🧑‍🦱 Page 3 — Patient

A patient-level profile page that follows an individual's journey from admission to discharge.

**KPI Cards:** Admission Date · Discharge Date · Medicine QTY

**Visuals:**
- 🪪 Patient Profile Card — photo, status (Admitted / Discharged), doctor, age, diagnosis, rating and feedback
- 📇 Personal Details — address, weight, diagnosis, doctor, age
- 📇 Patient Details — department, bed ID, diagnosis, doctor
- 🗓️ Medicine Tracking by Month and Day (matrix heatmap)
- 📊 Charges Type (bar chart — Room, Fees, Other, Medicine, Discount)
- 📊 Medicine Quantity by Drug (bar chart)

![Patient](Images/Patient.png)

---

### 👨‍⚕️ Page 4 — Doctor

Evaluates individual doctor performance, earnings and patient load, with an interactive commission calculator.

**KPI Cards:** Commission Earned · Commission Rate · Patient Spend · Doctor Fees · Estimated Commission

**Visuals:**
- 🪪 Doctor Profile Card — photo, availability, specialization, department, salary and patient ratings
- 🎚️ Commission Calculator (gauge) driven by **what-if parameters** for Commission Rate and Patient Spend
- 📋 Patient Table — photo, patient name, status, gender, total bill amount and fees
- 📋 Appointments (card list with patient name, time and date)

![Doctor](Images/Doctor.png)

---

### 🏨 Page 5 — Hospital

Monitors clinical operations — surgeries, lab results, bed capacity and appointments.

**Visuals:**
- 📊 Patients by Age Group (bar chart — 18–30, 31–45, 46–60, 60+)
- 📊 Bed Availability by Room Type (100% stacked column — ICU, General, Private; Available vs Occupied)
- 📋 Lab Test Results (table — patient, status, result, notes)
- 📋 Surgery Schedule (card list with patient, time and date)
- 📋 Doctor's Appointments (table — doctor, patient, reason, status)
- 🔘 Slicer: Patient name filter

![Hospital](Images/Hospital.png)

---

### 💰 Page 6 — Finance

Analyzes billing, salary costs and pharmacy performance.

**KPI Cards:** Patient Count · Doctor Count · Staff Count · Bill Count · Total Bill Amount · Med Sales Qty · Doctor Salary

**Visuals:**
- 📊 Patient Charges Type (column chart)
- 📊 Monthly Medicine Sales (column chart, May – Oct)
- 📊 Medicine Quantity vs Sales by Drug (100% stacked bar chart)
- 📊 Sales vs Stock by Supplier (100% stacked bar chart — MediCore Enterprises, HealthCare Supplies, MediPharm Distributors)
- 🔘 Slicer: Month selector

![Finance](Images/Finance.png)

---

## ✅ Analytical Outcomes

The dashboard delivers the following actionable insights:

- **Revenue Driver Identification** — Surgery is by far the largest charge type (~$0.43M), followed by room charges (~$0.17M), showing where billing accuracy and pricing matter most.
- **Patient Flow Monitoring** — Discharge % and discharges by date help administrators track throughput and plan for admission peaks.
- **Bed Capacity Management** — Available vs occupied beds by ward type highlight pressure points across ICU, General and Private rooms.
- **Patient Demographics** — Most patients fall in the 31–45 and 46–60 age groups, supporting staffing and service planning.
- **Doctor Performance & Compensation** — Patient spend, fees and commission per doctor, plus a what-if calculator, support fair and transparent compensation decisions.
- **Pharmacy & Supplier Analysis** — Monthly medicine sales, top-selling drugs and supplier sales vs stock support procurement and inventory planning.
- **Patient Experience Tracking** — Star ratings and written feedback at patient and doctor level surface the quality of care delivered.

---

## Conclusion 🎯

This **Hospital Analysis Dashboard** demonstrates how Power BI can unify clinical, operational and financial hospital data into a single, interactive analytics platform. By combining Power Query ETL, DAX measures, what-if parameters and an app-style visual design, the dashboard gives hospital administrators, department heads, doctors and finance teams the information they need to monitor performance, manage capacity and make evidence-based decisions — all from one report.

The project serves as a strong portfolio piece showcasing real-world healthcare analytics skills applicable across hospitals, clinics, healthcare networks and health-insurance organizations.

---

## Contributions

Explore the Power BI dashboard for interactive visualizations: [Power BI File](#)

Explore the Excel file for query and data analysis: [Excel Data File](https://github.com/VickyPatel-MSPI/Hospital/tree/main/Excel%20Data)

⭐ If you found this project helpful, consider giving it a star — it helps others discover it!
