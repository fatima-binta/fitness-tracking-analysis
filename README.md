# Fitness Tracking Analysis Dashboard

![Fitness Tracking Analysis Dashboard](images/fitness-dashboard.png)

## Executive Summary
A Power BI dashboard that compares four workout types (Yoga, Strength, Cardio, HIIT) across weekly training hours, calories per hour, heart rate, body-fat percentage, BMI and water intake in a dataset of 1,359 user records. The analysis is descriptive. Differences between workout types are small for most measures, and nothing here establishes cause and effect.

## Business Problem & Objectives
**Problem:** How do workout types compare on training volume, energy expenditure, cardiovascular measures and body composition in this dataset?

**Objectives**
1. Compare weekly training hours and calories per hour by workout type.
2. Explore average BMI and body-fat percentage by workout type.
3. Compare average active and resting heart rate by workout type.
4. Explore average water intake across age groups.
5. Allow filtering by experience level (Beginner, Intermediate, Expert).

## Tools & Technologies
- Power BI Desktop (visuals, DAX measures and one calculated column)
- Power Query (Source, Navigation, Promoted Headers, Changed Type)
- Microsoft Excel (source workbook)

## Dataset Overview
- **File:** `Fitness_Tracking.xlsx`, sheet `gym_members_exercise_tracking_s`
- **Size:** 1,359 records, 16 columns. `Users` is a unique ID (1 to 1,359), so each row is one user record.
- **Key fields:** Age, Gender, Weight (kg), Height (m), Max_BPM, Avg_BPM, Resting_BPM, Session_Duration (hours), Calories_Burned, Workout_Type, Fat_Percentage, Water_Intake (liters), Workout_Frequency (days/week), Experience_Level, BMI
- **Workout types:** Strength 375, Yoga 343, Cardio 334, HIIT 307
- **Experience levels:** Intermediate 569, Beginner 512, Expert 278
- **Age range:** 18 to 59. **Gender:** Female 684, Male 675.
- **Source:** a publicly available, synthetic dataset. The data file is not included in this repository.

## Business Questions Addressed
1. How do weekly training hours and calories per hour compare across workout types?
2. How do average BMI and body-fat percentage compare across workout types?
3. How do average active and resting heart rate compare across workout types?
4. How does average water intake vary by age group?

## Key Insights
All figures were recalculated from the source workbook.
1. **Weekly hours** (average of Session_Duration x Workout_Frequency): Yoga 4.78, HIIT 4.76, Strength 4.68, Cardio 4.45.
2. **Calories per hour** (total Calories_Burned / total Session_Duration): Cardio 774, Yoga 747, Strength 730, HIIT 728. This assumes Calories_Burned is recorded per session.
3. **Average body-fat percentage** is close across types: Yoga 22.76%, Strength 23.52%, Cardio 23.66%, HIIT 23.84% (overall 23.43%).
4. **Heart rate** barely varies: average active BPM runs from 144.96 (HIIT) to 146.73 (Strength), and average resting BPM from 63.55 (Strength) to 64.17 (HIIT).
5. **Water intake by age group:** Below 40 = 2.75 L (867 users), 40-50 = 2.73 L (315), 51-60 = 2.64 L (177). The gap is small and untested.

## Recommendations
- Treat all comparisons as descriptive. The gaps between workout types are small, and no significance testing was done.
- Document how each measure is calculated, especially the calories-per-hour measure and its assumption about Calories_Burned.
- Review the BMI column before relying on BMI visuals (see Data Limitations).
- Further analysis would need confirmed data provenance and a suitable statistical design.

## Dashboard & Visualizations
Four KPI cards (weekly hours, calories per hour, user counts), a scatter plot of average BMI against fat percentage by workout type, a fat-percentage chart, an active versus resting BPM bar chart, an average water intake bar chart by age group, and slicers for experience level.

## Repository Structure
```text
fitness-tracking-analysis/
├── README.md
├── dashboard/
│   └── Fitness_Tracking_Analysis.pbix
└── images/
    └── fitness-dashboard.png
```

## Data Limitations & Disclaimer
- The dataset is public and synthetic. It contains no names or direct identifiers and does not describe real individuals.
- **The `BMI` column does not agree with Weight (kg) and Height (m).** BMI recalculated from weight and height averages 22.74, but the column averages 20.08, and the two are essentially uncorrelated. BMI visuals use the column as supplied.
- Max_BPM contains five text values with stray tab or newline characters. This does not affect the charts shown.
- Experience_Level values carry trailing spaces in the source.
- Calories_Burned is assumed to be per session. The dataset does not say.
- Analysis is descriptive. No significance testing, no causal claims, and no medical or fitness advice.

## Future Improvements
- Clean Max_BPM and Experience_Level in Power Query.
- Add a calculated BMI from weight and height, or resolve the BMI inconsistency with the source.
- Add sample sizes and statistical testing before drawing conclusions.
