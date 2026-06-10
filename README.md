# 📊 30-Day Industrial Plant Maintenance Turnaround Monitor



![Dashboard Preview](https://github.com/ChaneSmithGit/Plant-Shutdown-PowerBI/blob/main/Shutdown_Dashboard.PNG)


---

## 🚨 1. The Business Problem & Origin Story
During a critical 30-day scheduled heavy machinery shutdown, an Operations Director requires immediate visibility into financial consumption, schedule drift, and workplace safety compliance. Manual data tracking across engineering departments causes significant visibility blind spots, threatening costly timeline overruns where daily downtime costs millions in deferred production revenue.

Because realistic data for these major industrial events is highly confidential, **I independently engineered and architected this entire dataset from scratch in Microsoft Excel**, modeling realistic operational dependencies across Mechanical, Electrical, Piping, and Process engineering teams for two distinct facilities. 

---

## 🎯 2. What Is Expected? (Strategic KPI Metrics)
To satisfy executive requirements, the dashboard was engineered to track three core operational pillars from left to right, answering critical business questions within 2 seconds of looking at the screen:

*   **Operational Safety Incidents (Current Value: 2):** Keeps teams fully accountable to the corporate **"Zero Harm"** workplace mandate. When projects run late, teams are tempted to take dangerous shortcuts. This metric ensures safety is never compromised for speed.
*   **Financial Expenditures - CapEx (Current Value: 1.95M):** Tracks the active Capital Expenditure financial burn rate against allocated turnaround maintenance budgets to prevent severe project overspending.
*   **Schedule Variance Status (Current Value: 6 Days Delayed):** Measures real-time schedule compliance against the strict 30-day window, translating raw numerical changes into direct business context.

---

## 🛠️ 3. Action Taken: What I Did & How
I ingested my custom-built Excel data architecture into Power BI Desktop and developed optimized analytical measures using **DAX (Data Analysis Expressions)** to automatically uncover operational blind spots:

### 💻 Complete DAX Formulas Implemented

#### A. Baseline Schedule Delta Tracking
This formula continuously calculates the net variance between planned project hours and actual execution time across all engineering teams.
```dax
Days Variance = SUM(ShutdownData[Actual Days]) - SUM(ShutdownData[Planned Days])
```

#### B. Dynamic Status Text Generation
To remove ambiguity for senior leadership, I wrote a logical `SWITCH` statement that automatically appends user-friendly text based on whether the project is ahead of, behind, or exactly on schedule. It utilizes the `ABS` (Absolute Value) function to eliminate confusing negative signs when the project is running ahead of schedule.
```dax
Schedule Variance Status = 
VAR CurrentVariance = [Days Variance]
RETURN
    SWITCH(
        TRUE(),
        CurrentVariance > 1, CurrentVariance & " Days Delayed",
        CurrentVariance = 1, CurrentVariance & " Day Delayed",
        CurrentVariance = 0, "On Schedule",
        CurrentVariance = -1, ABS(CurrentVariance) & " Day Ahead",
        CurrentVariance < -1, ABS(CurrentVariance) & " Days Ahead"
    )
```

#### C. Capital Expenditure (CapEx) Aggregator
This formula rolls up the total financial investment across all maintenance tasks. I utilized the modeling tools tab to explicitly format the output as currency, rounding to two decimal places for a cleaner executive presentation.
```dax
Financial Expenditures (CapEx) = SUM(ShutdownData[Budget Spent])
```

---

## 🎛️ 4. Dynamic Data Filtering & Slicing
To allow managers to drill down from a global group level into specific root causes, I implemented two highly accessible interactive filters on the left-side control panel:

*   **Facility Slicer:** Formatted as an easily navigable list allowing users to view the entire enterprise at once, or isolate data strictly for **Plant Alpha** or **Plant Beta**. 
*   **Date Slicer:** Configured as a chronological checklist representing the 30-day window timeline. Users can select specific deployment dates to see exactly what activities occurred on a single day or over a specific week during the turnaround.

---

## 💡 5. Executive Outcomes: Driving Insightful Decisions
This control center shifts a company from a *reactive* firefighting posture to a *proactive* decision-making state:

*   **Identifying the Bottleneck:** At the macro level, the model catches things a human would miss. It reveals that while the electrical team saved a day on the turbine overhaul, compounding delays in piping (`Main Piping Infrastructure`), flare repairs (`Primary Flare Header Repair`), and vessel cleaning combined into a critical **6 Days Delayed** timeline threat.
*   **Data-Driven Interventions:** With these insights, an Operations Director can make immediate, high-impact decisions: they can approve overtime budgets specifically for the *Piping* and *Process* departments to compress the 6-day delay, without wasting resources on the *Electrical* department which is already performing ahead of schedule.

---

## ♿ Inclusive Design & Accessibility Features
*   **Color-Blind Safe Palette:** Avoided confusing red/green color models. Relies entirely on high-contrast Deep Navy Blue (`#002060`) and Accessible Orange (`#D66000`).
*   **Visual Anchor Optimization:** Features clean data labels directly on the bars, removing cluttered background gridlines and vertical axes to decrease visual cognitive load.
*   **Assistive Tech Layout:** Equipped with comprehensive descriptive Alternative Text (Alt Text) on all charts for screen-reader compatibility.
