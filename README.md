🏋️ Fitness Analytic Dashboard — Power BI

Track Revenue · Monitor Members · Calculate Health
A complete Power BI dashboard for gym operations, membership trends, finances, and personal wellness analytics.


📸 Dashboard Preview
HomeOverallShow ImageShow Image
CalculatorMembersShow ImageShow Image

📋 Overview
The Fitness Analytic Dashboard is a 4-page interactive Power BI report designed for gym owners and fitness managers. It combines financial tracking, membership management, and a personal health calculator into a single, beautiful dark-themed dashboard.

📄 Pages
🏠 1. Home

Hero landing page with navigation to all sections
Quick KPI preview: Total Members, Trainers, Revenue, Profit

📊 2. Overall (Finances)

Monthly Revenue, Expenses, and Profit trend chart
Membership breakdown by type: Platinum, Gold, Silver
Active vs Expired membership counts
Monthly member count with Min/Max toggle
Client membership progress table with SVG progress bars

🧮 3. Calculator

BMI Calculator with color-coded gauge (Underweight / Normal / Overweight / Obese)
BMR Calculator using the Mifflin-St Jeor equation
TDEE based on selected activity level
Calorie targets: Maintain, Mild Loss, Weight Loss, Extreme Loss
Gender toggle (Male / Female) with dynamic background image
Sliders for Age, Height (ft), and Weight (kg)

👥 4. Members

Members by Age Group and Gender (18–25, 25–40, 40–60)
Members by Age Group and Status (Active / Expired)
Full client membership table with progress bars


🧮 DAX Measures
<details>
<summary>Click to expand all DAX formulas</summary>
Core KPIs
daxUsers_Count = DISTINCTCOUNT(Users[UserID])
Revenue = SUM(Payments[Amount])
Expanses = SUM(Expenses[Amount])
Profit = [Revenue] - [Expanses]
Membership Progress
daxComplete_Days =
DATEDIFF(
    SELECTEDVALUE(Users[MembershipStart]),
    MIN(TODAY(), SELECTEDVALUE(Users[MembershipEnd])),
    DAY
)

ProgressPercent = DIVIDE([Complete_Days], [Total_Days], 0) * 100
BMI
daxBMI =
VAR WeightKg = SELECTEDVALUE(Weight_Slider[Weight_Slider])
VAR HeightFeet = SELECTEDVALUE(Height_Slider[Height_Slider])
VAR HeightMeters = HeightFeet * 0.3048
RETURN ROUND(WeightKg / (HeightMeters * HeightMeters), 1)
BMR (Mifflin-St Jeor)
daxBMR =
VAR _Weight = SELECTEDVALUE(Weight_Slider[Weight_Slider])
VAR HeightFeet = SELECTEDVALUE(Height_Slider[Height_Slider])
VAR HeightCm = HeightFeet * 30.48
VAR Age = SELECTEDVALUE(Age_Slider[Age_Slider])
VAR Gender = SELECTEDVALUE(Slider_Gender[Category])
RETURN
    IF(
        Gender = "Male",
        (10 * _Weight) + (6.25 * HeightCm) - (5 * Age) + 5,
        (10 * _Weight) + (6.25 * HeightCm) - (5 * Age) - 161
    )
TDEE & Calorie Targets
daxTDEE = [BMR] * SELECTEDVALUE(Slider_Activity[ActivityFactor])
Maintain Calories            = [TDEE]
Mild Weight Loss Calories    = [TDEE] * 0.92
Weight Loss Calories         = [TDEE] * 0.85
Extreme Weight Loss Calories = [TDEE] * 0.70
</details>

🗂️ Data Model
TableDescriptionUsersMember profiles — ID, Name, Gender, Age, Membership type, Start/End dates, StatusPaymentsRevenue transactions per memberExpensesGym operating expensesCalendarDate table for time intelligenceTrainersTrainer recordsWeight_SliderParameter table for weight input (kg)Height_SliderParameter table for height input (ft)Age_SliderParameter table for age inputSlider_GenderGender selection table (Male / Female)Slider_ActivityActivity factor table for TDEE multiplierColorCodesTheme color codes for dynamic SVG coloringMin_Max_SwitchToggle table for Monthly Members chartLast_RefreshStores last dashboard refresh timestamp

⚙️ Activity Factors (TDEE Multipliers)
Activity LevelMultiplierSedentary1.2Lightly Active1.375Moderately Active1.55Very Active1.725Extra Active2.0

🛠️ How to Use

Clone or download this repository
Open Fitness_Dashboard.pbix in Power BI Desktop
Go to Transform Data and update file paths to your local dataset if needed
Click Refresh to load the latest data
Use the sliders and toggles on the Calculator page to explore health metrics
Navigate between pages using the top nav bar: Home → Overall → Calculator → Members


📁 Repository Structure
fitness-analytic-dashboard/
│
├── Fitness_Dashboard.pbix        # Main Power BI file
├── Dataset/
│   └── Dataset.xlsx              # Source data (Members, Payments, Expenses)
├── DAX/
│   └── measures.dax              # All DAX measures documented
├── screenshots/
│   ├── home.png
│   ├── overall.png
│   ├── calculator.png
│   └── members.png
├── Business_Requirements.docx    # Project requirements document
└── README.md

🎨 Design

Theme: Dark mode with charcoal backgrounds (#1a1a1a, #2a2a2a)
Accent Color: Orange (#E8622A) for KPIs and highlights
Charts: Red progress bars, white/gray secondary elements
Typography: Clean sans-serif, bold KPI values


📌 Requirements

Power BI Desktop (latest version recommended)
No external plugins or custom visuals required
Dataset provided in .xlsx format


👤 Author
[Your Name]
📧 [your.email@example.com]
🔗 LinkedIn Profile
🐙 GitHub

📜 License
This project is licensed under the MIT License — feel free to use, modify, and share with attribution.
