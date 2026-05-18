# 🏋️ Fitness Analytic Dashboard — Power BI

> **Track Revenue · Monitor Members · Calculate Health**  
> A complete Power BI dashboard for gym operations, membership trends, finances, and personal wellness analytics.

---

## 📸 Dashboard Preview

| Home | Overall |
|------|---------|
| ![](<screenshots/Fitness Project-1.png>) | ![](<screenshots/Fitness Project-2.png>) |

| Calculator | Members |
|------------|---------|
| ![](<screenshots/Fitness Project-3.png>) | ![](<screenshots/Fitness Project-4.png>) |

---

## 📋 Overview

The **Fitness Analytic Dashboard** is a 4-page interactive Power BI report designed for gym owners and fitness managers. It combines financial tracking, membership management, and a personal health calculator into a single, beautiful dark-themed dashboard.

---

## 📄 Pages

### 🏠 1. Home
- Hero landing page with navigation to all sections
- Quick KPI preview: Total Members, Trainers, Revenue, Profit

### 📊 2. Overall (Finances)
- Monthly Revenue, Expenses, and Profit trend chart
- Membership breakdown by type: Platinum, Gold, Silver
- Active vs Expired membership counts
- Monthly member count with Min/Max toggle
- Client membership progress table with SVG progress bars

### 🧮 3. Calculator
- **BMI Calculator** with color-coded gauge (Underweight / Normal / Overweight / Obese)
- **BMR Calculator** using the Mifflin-St Jeor equation
- **TDEE** based on selected activity level
- Calorie targets: Maintain, Mild Loss, Weight Loss, Extreme Loss
- Gender toggle (Male / Female) with dynamic background image
- Sliders for Age, Height (ft), and Weight (kg)

### 👥 4. Members
- Members by Age Group and Gender (18–25, 25–40, 40–60)
- Members by Age Group and Status (Active / Expired)
- Full client membership table with progress bars

---

## 🧮 DAX Measures

<details>
<summary>Click to expand all DAX formulas</summary>

### Core KPIs
```dax
Users_Count = DISTINCTCOUNT(Users[UserID])
Revenue = SUM(Payments[Amount])
Expanses = SUM(Expenses[Amount])
Profit = [Revenue] - [Expanses]
```

### Membership Progress
```dax
Complete_Days =
DATEDIFF(
    SELECTEDVALUE(Users[MembershipStart]),
    MIN(TODAY(), SELECTEDVALUE(Users[MembershipEnd])),
    DAY
)

ProgressPercent = DIVIDE([Complete_Days], [Total_Days], 0) * 100
```

### BMI
```dax
BMI =
VAR WeightKg = SELECTEDVALUE(Weight_Slider[Weight_Slider])
VAR HeightFeet = SELECTEDVALUE(Height_Slider[Height_Slider])
VAR HeightMeters = HeightFeet * 0.3048
RETURN ROUND(WeightKg / (HeightMeters * HeightMeters), 1)
```

### BMR (Mifflin-St Jeor)
```dax
BMR =
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
```

### TDEE & Calorie Targets
```dax
TDEE = [BMR] * SELECTEDVALUE(Slider_Activity[ActivityFactor])
Maintain Calories            = [TDEE]
Mild Weight Loss Calories    = [TDEE] * 0.92
Weight Loss Calories         = [TDEE] * 0.85
Extreme Weight Loss Calories = [TDEE] * 0.70
```

</details>

---

## 🗂️ Data Model

| Table | Description |
|-------|-------------|
| `Users` | Member profiles — ID, Name, Gender, Age, Membership type, Start/End dates, Status |
| `Payments` | Revenue transactions per member |
| `Expenses` | Gym operating expenses |
| `Calendar` | Date table for time intelligence |
| `Trainers` | Trainer records |
| `Weight_Slider` | Parameter table for weight input (kg) |
| `Height_Slider` | Parameter table for height input (ft) |
| `Age_Slider` | Parameter table for age input |
| `Slider_Gender` | Gender selection table (Male / Female) |
| `Slider_Activity` | Activity factor table for TDEE multiplier |
| `ColorCodes` | Theme color codes for dynamic SVG coloring |
| `Min_Max_Switch` | Toggle table for Monthly Members chart |
| `Last_Refresh` | Stores last dashboard refresh timestamp |

---

## ⚙️ Activity Factors (TDEE Multipliers)

| Activity Level | Multiplier |
|----------------|------------|
| Sedentary | 1.2 |
| Lightly Active | 1.375 |
| Moderately Active | 1.55 |
| Very Active | 1.725 |
| Extra Active | 2.0 |

---

## 🛠️ How to Use

1. **Clone or download** this repository
2. Open `Fitness_Dashboard.pbix` in **Power BI Desktop**
3. Go to **Transform Data** and update file paths to your local dataset if needed
4. Click **Refresh** to load the latest data
5. Use the sliders and toggles on the **Calculator** page to explore health metrics
6. Navigate between pages using the top nav bar: Home → Overall → Calculator → Members

---

## 📁 Repository Structure

```
fitness-analytic-dashboard/
│
├── PowerBI/
│   └── Fitness_Dashboard.pbix        # Main Power BI file
├── DAX/
│   └── measures.dax                  # All DAX measures documented
├── Dataset/
│   └── Dataset.xlsx                  # Source data (Members, Payments, Expenses)
├── Dashboard_Reports/
│   └── Fitness_Project.pdf           # Exported dashboard report
├── screenshots/
│   ├── Fitness Project-1.png         # Home page
│   ├── Fitness Project-2.png         # Overall page
│   ├── Fitness Project-3.png         # Calculator page
│   └── Fitness Project-4.png         # Members page
└── README.md
```

---

## 🎨 Design

- **Theme:** Dark mode with charcoal backgrounds (`#1a1a1a`, `#2a2a2a`)
- **Accent Color:** Orange (`#E8622A`) for KPIs and highlights
- **Charts:** Red progress bars, white/gray secondary elements
- **Typography:** Clean sans-serif, bold KPI values

---

## 📌 Requirements

- Power BI Desktop (latest version recommended)
- No external plugins or custom visuals required
- Dataset provided in `.xlsx` format

---

## 👤 Author

**Rabie Mohamed**  
🔗 [LinkedIn](https://www.linkedin.com/in/mohamed-rabie-0928b63aa/)  
🐙 [GitHub](https://github.com/mohamedsamir132)

---

## 📜 License

This project is licensed under the MIT License — feel free to use, modify, and share with attribution.
