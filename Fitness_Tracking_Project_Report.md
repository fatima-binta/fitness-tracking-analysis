# Fitness Tracking Analysis Dashboard: Project Report

**Author:** Fatima Binta Mohammed
**Tools:** Power BI, DAX, Power Query, Microsoft Excel

## 1. Executive Summary
The Fitness Tracking Analysis Dashboard is an interactive Power BI project built on a public, synthetic dataset of 1,359 user records. It compares four workout types (Yoga, Strength, Cardio and HIIT) on weekly training hours, calories per hour, average and resting heart rate, body-fat percentage, BMI, and water intake by age group. All dashboard values were recalculated independently from the source workbook. The analysis is descriptive: differences between workout types are small on most measures, and nothing in the dataset establishes that a workout type causes a particular outcome.

## 2. Business Problem and Objectives
**Problem:** How do Yoga, Strength, Cardio and HIIT compare on training volume, energy expenditure, cardiovascular measures and body composition in this dataset?

**Objectives**
1. Compare weekly training hours and calories per hour across workout types.
2. Explore average BMI and body-fat percentage by workout type.
3. Compare average active and resting heart rate by workout type.
4. Explore average water intake across the age groups in the dataset.
5. Allow filtering by experience level (Beginner, Intermediate, Expert).

## 3. Business Questions
1. How do weekly training hours and calories per hour compare across Yoga, Strength, Cardio and HIIT? (KPI cards)
2. How do average BMI and body-fat percentage compare across workout types? (scatter plot and fat-percentage chart)
3. How do average active and resting heart rate compare across workout types? (BPM bar chart)
4. How does average water intake vary by age group? (water intake bar chart)

Limitation: the dashboard cannot answer questions about cause and effect, individual outcomes, health risk or the effectiveness of a program.

## 4. Dataset and Methodology
**Source:** `Fitness_Tracking.xlsx`, sheet `gym_members_exercise_tracking_s`. The data is public and synthetic, and the data file is not included in the repository.

**Structure:** 1,359 records and 16 columns, with no blank cells and no duplicate rows. `Users` is a unique ID from 1 to 1,359, so each row is one user record. Age runs from 18 to 59. Gender is Female 684 and Male 675.

| Field | Categories |
|---|---|
| `Workout_Type` | Strength 375, Yoga 343, Cardio 334, HIIT 307 |
| `Experience_Level` | Intermediate 569, Beginner 512, Expert 278 |
| Age groups (calculated) | Below 40 = 867, 40-50 = 315, 51-60 = 177 |

**Power Query:** the applied steps are Source, Navigation, Promoted Headers and Changed Type. No other transformation was applied.

**DAX:**
- `Age_Group` is a calculated column built with `SWITCH(TRUE(), ...)` on `Age`.
- Weekly hours per workout is `AVERAGEX` of `Session_Duration (hours)` x `Workout_Frequency (days/week)`, filtered by `Workout_Type`.
- Calories per hour per workout is total `Calories_Burned` divided by total `Session_Duration (hours)`, filtered by `Workout_Type`. This assumes `Calories_Burned` is recorded per session. The dataset does not say.
- User counts per workout use `COUNTROWS` filtered by `Workout_Type`.

**Visuals:** four KPI cards, a scatter plot (average BMI against average fat percentage by workout type), a fat-percentage chart, a clustered bar chart of active and resting BPM, a bar chart of water intake by age group, and experience-level slicers.

**Validation:** every displayed value was recalculated from the workbook, and the results are summarised in section 8. Validation led to several fixes before publication: swapped card labels, a calories measure that divided per-session calories by weekly hours, a chart showing shares of combined averages instead of the averages, a misspelled age-group label, and a duplicated BPM series.

## 5. Key Insights and Observations
All figures were recalculated from the workbook.

1. **Weekly training hours are close across types.** Yoga 4.78, HIIT 4.76, Strength 4.68, Cardio 4.45. Cardio is about 0.3 hours below Yoga.
2. **Calories per hour are highest for Cardio.** Cardio 774, Yoga 747, Strength 730, HIIT 728. The difference between the top and bottom is about 6%. On average calories per record, Yoga is highest (1,073), then Cardio (1,052), Strength (1,011) and HIIT (1,003).
3. **Average body-fat percentage is similar across types.** Yoga 22.76%, Strength 23.52%, Cardio 23.66%, HIIT 23.84%, against an overall average of 23.43%. The spread is about 1.1 percentage points.
4. **Heart rate barely differs.** Average active BPM runs from 144.96 (HIIT) to 146.73 (Strength), and average resting BPM from 63.55 (Strength) to 64.17 (HIIT).
5. **Water intake varies slightly by age group.** Below 40 averages 2.75 L, 40-50 averages 2.73 L and 51-60 averages 2.64 L.
6. **By workout type, average water intake is also close:** Cardio 2.77 L, HIIT 2.74 L, Strength 2.73 L, Yoga 2.70 L.

Limitations: no significance testing was done, so none of these gaps should be described as meaningful or causal. Group sizes differ (for example 177 users aged 51-60 against 867 under 40).

## 6. Recommendations
**Supported by this analysis**
1. Present workout comparisons as descriptive, and state that the differences are small.
2. Label every measure with its exact calculation and unit. In particular, state that calories per hour assumes `Calories_Burned` is per session.
3. Resolve the BMI inconsistency (section 7) before relying on any BMI result.
4. Add a Trim step for `Experience_Level` and clean `Max_BPM` in Power Query.

**Requiring further work**
5. Add sample sizes and statistical testing before reporting differences between workout types as meaningful.
6. Confirm the original data source and licence before sharing any data file.

Nothing here is fitness, health or medical advice.

## 7. Data Limitations and Disclaimer
- The dataset is public and synthetic. It contains no names or direct identifiers and does not describe real individuals.
- **The `BMI` column does not agree with `Weight (kg)` and `Height (m)`.** BMI recalculated from weight and height averages 22.74, but the column averages 20.08, and the two are essentially uncorrelated. BMI visuals use the column as supplied.
- `Max_BPM` is stored as text and contains five values with stray tab or newline characters. After cleaning, its overall average is 180.22. It is not shown on the dashboard.
- `Experience_Level` values carry trailing spaces in the source, and no Trim step has been applied.
- `Age_Group` includes unused categories (61-69 and 70+), because no user is over 59.
- `Calories_Burned` is assumed to be per session.
- The analysis is descriptive. No significance testing was performed, and no causal claims are made.

## 8. Validation Summary
| Dashboard item | Displayed | Calculated | Status |
|---|---|---|---|
| Total users | 1,359 | 1,359 unique `Users` | Exact |
| Users by type (Yoga / Strength / Cardio / HIIT) | 343 / 375 / 334 / 307 | 343 / 375 / 334 / 307 | Exact |
| Weekly hours (Yoga / Strength / Cardio / HIIT) | 4.78 / 4.68 / 4.45 / 4.76 | 4.776 / 4.682 / 4.455 / 4.764 | Rounding |
| Calories per hour (Yoga / Strength / Cardio / HIIT) | 747 / 730 / 774 / 728 | 746.8 / 730.5 / 773.9 / 728.5 | Rounding |
| Average fat % (Yoga / Strength / Cardio / HIIT) | 22.76 / 23.52 / 23.66 / 23.84 | 22.756 / 23.521 / 23.655 / 23.837 | Rounding |
| Water by age group (Below 40 / 40-50 / 51-60) | Bars | 2.753 / 2.727 / 2.642 | Consistent |
| Active and resting BPM | Bars | 144.96 to 146.73 and 63.55 to 64.17 | Consistent |
| BMI scatter | Points near 19.8 to 20.6 | Matches the `BMI` column | Matches, but the column is inconsistent with weight and height |

## 9. Skills Demonstrated
- Dashboard design in Power BI
- DAX measures and a calculated column
- KPI definition and validation against source data
- Descriptive and comparative analysis
- Data quality review and documentation
- Communicating findings without overstating them
